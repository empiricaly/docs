# Part 3: Adding Social Information and New Factors

## Adding social information

Social information is included in the default Empirica template via `SocialExposure.jsx`. However, nothing will show if we don't have any other players!

In this example we're going to modify the default behavior so that \(a\) the social information only shows after participants have entered their initial response, and \(b\) social information shows only for a participant's network neighbors.

### In the admin panel

Return to your `admin panel` and to the `configuration` section.

First, create a multi-player game by adding a new factor value to `playerCount` \(following the same procedure as before\) with more than one player, then creating a new treatment and a new batch. 

You can then launch the Empirica app multiple times in the same web browser by clicking `New Player` in the header tab. The game will start once the required number of players have entered the lobby.

### On the server side

Let's go back to `server/main.js` 

Within each `round` are multiple `stages`. We can add more stages by returning to the `Empirica.gameInit` method and adding the following after the other `roud.addStage` part.

```javascript
round.addStage({
  name: "social",
  displayName: "Social Information",
  durationInSeconds: 120
});
```

So that your `server/main.js` file looks like this:

```javascript
import Empirica from "meteor/empirica:core";

import "./callbacks.js";
import "./bots.js";

import { taskData } from "./constants";

// gameInit is where the structure of a game is defined.
// Just before every game starts, once all the players needed are ready, this
// function is called with the treatment and the list of players.
// You must then add rounds and stages to the game, depending on the treatment
// and the players. You can also get/set initial values on your game, players,
// rounds and stages (with get/set methods), that will be able to use later in
// the game.
Empirica.gameInit(game => {
  // Establish node list
  const nodes = [];
  for (let i = 0; i <= game.players.length; i++) {
    nodes.push(i);
  }

  game.players.forEach((player, i) => {
    player.set("avatar", `/avatars/jdenticon/${player._id}`);
    player.set("score", 0);

    // Give each player a nodeId based on their position
    player.set("nodeId", i);

    // Assign each node as a neighbor
    const networkNeighbors = nodes.filter(node => node !== i);
    player.set("neighbors", networkNeighbors);
  });

  Object.keys(taskData).forEach(taskName => {
    const task = taskData[taskName];
    const round = game.addRound({
      data: {
        taskName: taskName,
        questionText: task.questionText,
        imagePath: task.path,
        correctAnswer: task.correctAnswer
      }
    });

    round.addStage({
      name: "response",
      displayName: "Response",
      durationInSeconds: 120
    });

    round.addStage({
      name: "social",
      displayName: "Social Information",
      durationInSeconds: 120
    });

  })

});

```

### In Round.jsx

We can access the stage information \(including the name\) within the app, so we're going to modify `Round.jsx` to display the `SocialExposure` component only when `stage.name === "social"`. 

Modify the `<SocialExposure {...this.props} />` part with this code:

```jsx
{
  stage.name === "social" && (
    <SocialExposure {...this.props} />
  )
}
```

So that your `Round.jsx` file looks like this:

```jsx
import React from "react";

import PlayerProfile from "./PlayerProfile.jsx";
import SocialExposure from "./SocialExposure.jsx";
import Task from "./Task.jsx";

export default class Round extends React.Component {
  render() {
    const { round, stage, player, game } = this.props;

    return (
      <div className="round">
        <div className="content">
          <PlayerProfile player={player} stage={stage} game={game} />
          <Task game={game} round={round} stage={stage} player={player} />
          {
            stage.name === "social" && (
              <SocialExposure {...this.props} />
            )
          }
        </div>
      </div>
    );
  }
}

```

### In SocialExplosure.jsx

Finally, we'll modify `SocialExposure.jsx` .

We're going to remove the slider element, since we are not using that in this example, and just show the number:

```jsx
  renderSocialInteraction(otherPlayer) {
    // Get the value or return NA if no value was entered
    const value = otherPlayer.round.get("value") ?? "NA";
    return (
      <div className="alter" key={otherPlayer._id}>
        <img src={otherPlayer.get("avatar")} className="profile-avatar" />
        Guess: {value}
      </div>
    );
  }
```

{% hint style="info" %}
This simple feature shows just how powerful Empirica can be. Just by virtue of storing user information with `player.set()` and displaying that information in the `SocialExposure` component, the participant's interface is automatically updated. This is due to the way React.js works with Meteor: any time a piece of information passed as one of the props is updated, then the display re-renders to show the new information.
{% endhint %}

We also only want to show information for players listed in `player.get("neighbors")` . Let's do this by replacing the declaration for `const otherPlayers` with:

```javascript
const otherPlayers = game.players.filter(p =>
  player.get("neighbors").includes(p.get("nodeId"))
);
```

_This sets the `otherPlayers` to players that are not the current player and players that are in the current player's neighbors \(their node is in their list of neighbors\)._

Now, let's add a title to make this component match the other two. And let's make a condition to determine whether the phrase, "There are X other players:", should be plural or not. This paragraph will show different text depending on whether there is only one or more other players.

```jsx
return (
      <div className="social-exposure">
        <h3 className="title">Social Information</h3>
        <p className="title">
          {
            otherPlayers.length > 1
              ? <strong>There are {otherPlayers.length} other players:</strong>
              : <strong>There is one other player:</strong>
          }
        </p>
        {otherPlayers.map(p => this.renderSocialInteraction(p))}
      </div>
    );
```

In the end, your `SocialExposure.jsx` component should look like this:

```jsx
import React from "react";

export default class SocialExposure extends React.Component {
  renderSocialInteraction(otherPlayer) {
    // Get the value or return NA if no value was entered
    const value = otherPlayer.round.get("value") ?? "NA";
    return (
      <div className="alter" key={otherPlayer._id}>
        <img src={otherPlayer.get("avatar")} className="profile-avatar" />
        Guess: {value}
      </div>
    );
  }

  render() {
    const { game, player } = this.props;

    const otherPlayers = game.players.filter(p =>
      player.get("neighbors").includes(p.get("nodeId"))
    );

    if (otherPlayers.length === 0) {
      return null;
    }

    return (
      <div className="social-exposure">
        <h3 className="title">Social Information</h3>
        <p className="title">
          {
            otherPlayers.length > 1
              ? <strong>There are {otherPlayers.length} other players:</strong>
              : <strong>There is one other player:</strong>
          }
        </p>
        {otherPlayers.map(p => this.renderSocialInteraction(p))}
      </div>
    );
  }
}

```

## Adding treatments and factors

One of the key features of Empirica is that you can think like a scientist and control your app with experimental conditions. To show how this works we'll add a factor, allowing us to modify the stage length without changing the code.

First, we add the new factor through the `admin panel` by navigating to the factors interface and clicking "New Factor". 

We will create a `stageLength` factor of type `Integer`.  You need to add a little description, but you do not need to specify a minimum or maximum.

{% hint style="info" %}
Because we are replacing timing of the stages with this factor, you should make this factor `Required` so that it has to be entered when creating treatment. Otherwise, if you forget to set a `stageLength`, the experiment will break.

You can also archive your previous treatments that do not have a `stageLength`, to avoid being tempted to use them and declutter your interface. You'll always be able to unarchive these if necessary.
{% endhint %}

Now, add a new value to the factor you have created \(such as `60`, for 60 seconds\).

**Now create a new treatment where you use the stage length.** You can now use this new treatment when you create new batches. 

We use this treatment information in the `Empirica.gameInit` callback. In  `server/main.js` , go to the part where stages are added to the round, and change the `durationInSeconds: 120`to  `durationInSeconds: game.treatment.stageLength` for both stages.

```javascript
		round.addStage({
			name: "response",
			displayName: "Response",
			durationInSeconds: game.treatment.stageLength
		});

		round.addStage({
			name: "social",
			displayName: "Social Information",
			durationInSeconds: game.treatment.stageLength
		});
```

{% hint style="info" %}
If you are trying to edit elements \(e.g., change the styling\) you might want the stage length to be longer so you have time to work on your changes without creating new games over and over again. Consider creating a `stageLength` factor that is longer and make a 'test' or 'dev' treatment with this factor that's just for you when you want to do some long edits.
{% endhint %}

## Exporting the Treatments and Factors

The Treatments and Factors that you have created are all saved on your local database. This means that you will no longer have access to them if you share this someone through GitHub, if you reset your app, or if you deploy your app.

**You should export these Treatments and Factors so that they can be imported back in if need be.**

Go to `Configuration` on the admin panel \(where you create Treatments and Factors\) and click the `Export` button. This will allow you to download a `.yaml` file of your Treatments and Factors. Add it to the root of your Empirica experiment folder \(or somewhere else where it is handy to access\). It can be good to rename it to something like `factors.yaml`.

Now, if you need to recreate your Treatments and Factors, you can just go `Configuration` on the admin panel, click `Import` and select your `.yaml` file.

