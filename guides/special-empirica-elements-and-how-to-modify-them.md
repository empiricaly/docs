# Special Empirica Elements \(and how to modify them\)

## Centered

The **Centered component** is a custom element from Empirica that you can use to surround other elements of your components. This will make them centered on the page.

Import the element with:

```text
import { Centered } from "meteor/empirica:core";
```

And use it like this:

```jsx
<Centered>        
    <div> Other elements </div>    
</Centered>
```

## Timer

There is a `Timer.jsx` component that represents the stage timer and shows players how much time is left at that stage. 

For the Timer to work, the `stage` has to be passed down to it as a prop. For example, in the `PlayerProfile.jsx` of [your first Empirica experiment](tutorial-your-first-experiment/your-first-experiment.md):

```jsx
return (
      <aside className="player-profile">
        {this.renderProfile()}
        {this.renderScore()}
        <Timer stage={stage} />
      </aside>
    );
```

Inside the `Timer.jsx` component, what makes the timer is that it extracts the `remainingSeconds` from the `stage`, and that it imports the `StageTimeWrapper` to export the Timer with `export default (Timer = StageTimeWrapper(timer));`. 

Here is the code for a basic Timer component:

```jsx
import { StageTimeWrapper } from "meteor/empirica:core";
import React from "react";

class timer extends React.Component {
  render() {
    const { remainingSeconds } = this.props;

    const classes = ["timer"];
    if (remainingSeconds <= 5) {
      classes.push("lessThan5");
    } else if (remainingSeconds <= 10) {
      classes.push("lessThan10");
    }

    return (
      <div className={classes.join(" ")}>
        <h4>Timer</h4>
        <span className="seconds">{remainingSeconds}</span>
      </div>
    );
  }
}

export default (Timer = StageTimeWrapper(timer));
```

## About Section

The **About section** is a small window that will upon when players click the top right corner button `About`. For example:

![](../.gitbook/assets/capture.png)

In `client/main.js` you can set your custom component for the About section. 

Import the component to the `client/main.js` with:

```text
import <component name> from <path>;
```

And then set it with:

```text
Empirica.about(<component name>);
```

However, setting an About section is completely optional, you can get rid of it by not writing the `Empirica.about` line in `client/main.js` or deleting it if it is already written.

## Header

The **Header** is the blue rectangle at the top of the page in an Empirica app. 

See more about the structure of the Header component [here](../overview/api.md#empirica-header-component).

Import the component to the `client/main.js` with:

```text
import <component name> from <path>;
```

And then set it with:

```text
Empirica.header(<component name>);
```

Or you can get rid of the Header with:

```text
Empirica.header(() => null);
```

## Special Dev Wrap Header

If you don't want the blue header to show, especially not when you deploy your experiment, but you still want to access the `Reset Player` and `New Player` buttons during development, you can surround each of your important components \(e.g., Round.jsx, Intro steps, etc.\) with this special component:

```jsx
import React from 'react'

export default function DevWrapper({
    children,
    showOpenAltPlayer,
    onOpenAltPlayer,
    showReset,
    onReset, }) {

    const isDev = showReset || showOpenAltPlayer

    return (
        <div>
            {isDev &&
                <div style={{
                    display: "flex"
                    , justifyContent: "flex-end"
                    , alignItems: "center"
                    , margin: "5px"
                }}>
                    <p style={{
                        fontSize: "1rem"
                        , color: "grey"
                        , margin: "0"
                    }}>Dev buttons:</p>
                    {showReset &&
                        <button style={devButtons} onClick={onReset}>Reset Player</button>
                    }
                    {showOpenAltPlayer &&
                        <button style={devButtons} onClick={onOpenAltPlayer}>New Player</button>
                    }

                </div>
            }

            { children}
        </div >
    )
}

// Styling
const devButtons = {
    backgroundColor: "transparent"
    , fontSize: "1rem"
    , color: "grey"
    , margin: "0 2.5px"
    , cursor: "pointer"
}
```

For example, to have it show in each round, import it into the round component and use it this way:

```jsx
export default class Round extends Component {
	render() {
		const { round, stage, player, game } = this.props

		return (
			<DevWrapper {...this.props}>
				<div>
					The Rounds
				</div>
			</DevWrapper>
		)
	}
}
```

## Breadcrumb

The **Breadcrumb** is the track at the top of a Round that shows the player which Stage of the Round they are currently at.

See more about the structure of the Breadcrumb component [here](../overview/api.md#empirica-breadcrumb-component).

![](../.gitbook/assets/example_breadcrumb.png)

In `client/main.js` you can set your custom component for the Breadcrumb. 

Import the component to the `client/main.js` with:

```text
import <component name> from <path>;
```

And then set it with:

```text
Empirica.breadcrumb(<component name>);
```

Or you can get rid of the Breadcrumb with:

```text
Empirica.breadcrumb(() => null);
```

## NewPlayer \(where players set their id\)

Players have to set their id at a **NewPlayer page** just after the consent page. This could be a name, an MTurk/Prolific id, a student id, an email, etc. 

You might want to change its design, format, or instructions. For example, if you want participants to provide a student id but no other personal information, you would want to make your own clear instructions.

The default NewPlayer.jsx page looks like this:

```jsx
import React, { Component } from 'react'
import { Centered } from "meteor/empirica:core";

export default class NewPlayer extends Component {
  state = { id: "" };

  handleUpdate = event => {
    const { value, name } = event.currentTarget;
    this.setState({ [name]: value });
  };

  handleSubmit = event => {
    event.preventDefault();

    const { handleNewPlayer } = this.props;
    const { id } = this.state;
    handleNewPlayer(id);
  };

  render() {
    const { id } = this.state;

    return (
      <Centered>
        <div >
          <form onSubmit={this.handleSubmit}>
            <h1>Identification</h1>

            <p>
              Please enter your id:
                        </p>

            <input
              dir="auto"
              type="text"
              name="id"
              id="id"
              value={id}
              onChange={this.handleUpdate}
              required
              autoComplete="off"
            />

            <br />

            <p>
              <button type="submit">Submit</button>
            </p>

          </form>
        </div>
      </Centered>
    )
  }
}
```

The important parts of your component is to have methods to handle when the player is changing the id they write \(`handleUpdate`\) and when the player clicks the submit button \(`handleSubmit`\) so that in the end it is using the `handleNewPlayer()` method that will set the player's id as the string submitted and moves the player to the first Intro Step.

You can either modify the `NewPlayer.jsx` file, or create a new one and set it in the `client/main.js`.

**If you want to create a new file:**

Import the component to the `client/main.js` with:

```text
import <component name> from <path>;
```

And then set it with:

```text
Empirica.newPlayer(<component name>);
```

## Lobby

In the **Admin Panel**, in the **Configuration tab**, in **Lobby Configurations**, you can set important aspects of the Lobby such as:

* The `Timeout Type` \(whole lobby or individual\)
* The `Timeout Duration in Seconds`
* The `Timeout Strategy` \(fail, ignore\)
* The `Extend Count`

But you might want to customise your Lobby even further \(e.g., change what it looks like or what is written\). To do so, you can create a **custom Lobby component**. It will receive two props, the `player` and the `gameLobby`. The `gameLobby` can be used to extract the following properties in the customising of your Lobby:

* `gameLobby.treatment.playerCount` will give you how many players the Game expects
* `gameLobby.queuedCount` will give you the total number of players queued for this game, including ready players and players currently going through the intro steps. 
* `gameLobby.readyCount` will give you the number of players ready to play. They have completed the intro steps, and they are on the lobby page.

For a better idea of what you can do and customise, see the default version of the Lobby [here](https://github.com/empiricaly/meteor-empirica-core/blob/master/ui/components/GameLobby.jsx).

Once you have created your custom Lobby component, you can import the component to the `client/main.js` with:

```text
import <component name> from <path>;
```

And then set it with:

```text
Empirica.lobby(<component name>);
```

### Can I add a chat to the Lobby?

To add a chat to the Lobby you need to install the **Empirica Chat** with:

```text
meteor npm install --save @empirica/chat
```

Then you can use the default Lobby with chat. Do so by importing the  `LobbyChat` component of this package in your experiment's `client/main.js` file, like this:

```text
import { LobbyChat } from "@empirica/chat";
```

And setting it to the lobby like this:

```text
Empirica.lobby(LobbyChat);
```

Or you can manually add a chat to your customised Lobby.

## Empirica Chat

To add a chat  in Empirica, you can use our simple solution by using the **Empirica Chat**. For detailed information about Empirica Chat, see [here](https://github.com/empiricaly/chat).

### Installing

First, install Empirica Chat with:

```text
meteor npm install --save @empirica/chat
```

### Import and Usage

In the components you want to use the chat, import this:

```text
import { Chat } from "@empirica/chat";
```

Then you can create the chat component with:

```jsx
<Chat player={player} scope={game} />
```

`chat` expects 2 required props:

* `player`: the current player
* `scope`: object that the chat will be attached to, can be game, round, or stage objects.

### Scoping

The scope is important because it determines where the chat data \(the messages\) will be stored. You can access them again afterwards based on the scope. For example, if the scope is set to the Round:

```javascript
const chat = round.get("chat") ?? []
```

### Multiple chat instances within the same scope

You can pass an optional `customKey` string prop to differentiate different chats within the same scope. This changes which get/set key on the given scope the chat will be recorded.

```jsx
<Chat player={player} scope={game} customKey="casual_chat" />
```

### Display names

`Chat` also displays a name for each participant, which you need to set in the experiment independently of the `playerId`: `player.set('name', "myPseudonym")`

### Adding timestamp to chat message

You can pass an optional `timeStamp` date prop to add the timestamp attribute on each message sent. 

Run this command to add mizzao timesync:

```text
meteor add mizzao:timesync
```

You can pass an optional timeStamp date prop to add the timestamp attribute on each message sent:

```jsx
// reactive time value only updates at 1000 ms
const timeStamp = new Date(TimeSync.serverTime(null, 1000));

<Chat player={player} scope={game} timeStamp={timeStamp} />
```

### Other functionalities

There are many other functionalities with Empirica chat that you can see [here](https://github.com/empiricaly/chat).

