# Using Custom Collections

Empirica allows you to store data in different places for each game \(e.g., game, round, stage, player, etc.\). But you might want to store and use data in a special structure or have certain data accessible across all games \(e.g., global configs, data from all participants\). To do so, you need to create and use custom collections in your database. Thankfully, Empirica uses Meteor, which makes this process easy \(see [their collections API docs](https://docs.meteor.com/api/collections.html)\).

_Note: you can use custom collections during development without_ [_connecting your experiment to MongoDB_](the-settings-file/connecting-locally-to-mongodb.md)_. However, it is easier to see what is happening if you are connected to a cloud database that you can monitor._

### Using custom collections just on the server side

You need to create a file to connect to your custom collection. I usually put these in an `api` folder. Create a `.js` file for the collection of interest. For example, `playerScores.js`.

The contents should look something like this:

```javascript
import { Mongo } from 'meteor/mongo';

export const PlayersScores = new Mongo.Collection('players-scores');
```

The string you pass to the `new Mongo.Collection` is the name of the collection.

Then you can access this collection in your other server side file with an import statement, for example:

```javascript
import { PlayersScores } from "./api/playerScores"
```

Then you can use this collection by updating it, or setting it to elements that are easy to access on the client side \(e.g., game, round, stage\).

To get elements from the collection \(example\):

```javascript
PlayersScores.find({}).fetch()
```

To insert a new element to the collection \(example\):

```text
PlayersScores.insert({ playerId: player._id, score: player.get("score") })
```

### Using custom collections on the client side

If you want to use the custom collection directly on the client side, you should set the files in a `shared` folder in the root of your experiment files so that it can be access both on the client and server side. **Set the file for the collection as explained above.**

In `server/main.js` , import the collection and publish it.  
_For example with a 'admin-global-configs' custom collection._

```javascript
import { Configs } from "../shared/api/collectionAdminGlobalConfigs.js";

Meteor.publish('admin-global-configs', function publishTasks() {
  return Configs.find({})
})
```

On the client side you need to use a special tracker from Meteor to follow when the collection is available and when it has been changed. To use it, you want to create three parts to your component:

* One part to hold the other parts,
* one part to connect to the collection and send the information as props to
* the last part where the information will be displayed

For example we create a waiting consent component where we want participants to wait until a certain time before they can join the experiment.

Create the `WaitingConsent.jsx` component \(don't  forget it to set it in the `client/main.js`\). 

Import the important elements:

```javascript
import { withTracker } from "meteor/react-meteor-data"
import { Configs } from '../../../shared/api/collectionAdminGlobalConfigs'
```

Then create the first part in which you call the second part.

```jsx
export default class WaitingConsent extends Component {
    render() {

        return (
            <div>
                {/* Load the db data which loads the page contents */}
                <WaitingConsentPageContents {...this.props} />
            </div>
        )
    }
}
```

Create the third and second part \(in this order\):

```jsx
class WaitingConsentPage extends Component {

    render() {
        const { loading, now, timeToStart } = this.props

        if (loading) {
            return (
                <div>Loading...</div>
            )
        }

        let difference = +timeToStart - +now

        // If waiting for the time, show the countdown and instructions
        if (difference > 1) {
            return (
                <div>
                    <h3>Countdown</h3>
                    ...
                    </div>
                </div>

            )
        }

        // otherwise return consent page
        return (
            <div>
               <h3>Consent</h3>
                ...
            </div>
        )
    }
}

WaitingConsentPageContents = withTracker(rest => {

    // Suscribe to collection information, and return nothing as long as it is loading
    const loading = !Meteor.subscribe("admin-global-configs").ready()
    if (loading) {
        return {
            loading
        }
    }

    // Get the globalConfigs collection
    const globalConfigs = Configs.find({}).fetch()[0] ?? {}
    const timeToStart = new Date(globalConfigs.timeToStart)

    // Get time now (makes sur the whole process is synced)
    const now = new Date(TimeSync.serverTime(null, 1000))

    return {
        loading,
        timeToStart,
        now,
    };

})(WaitingConsentPage)
```

You can see how in the last component we are subscribing to the collection, waiting for it to load, and then sending this information to the other component.

