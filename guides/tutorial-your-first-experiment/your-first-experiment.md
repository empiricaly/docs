---
id: your-first-experiment
title: Your First Experiment
---

# Part 1: Creating an Estimation Task

## Getting Started With Empirica

Although it won't do too much, Empirica works straight out of the box. The first step to customizing your Empirica app is to launch a test game so you can see your edits in action.

1. Follow the [setup](../../getting-started/setup/) guide and [create your experiment](../../getting-started/quick-start.md).
2. Follow the guide on [running your experiment](../../getting-started/quick-test.md).
3. Then, visit your admin panel [http://localhost:3000/admin](http://localhost:3000/admin).
4. Log in using the password generated in the command line by step 2.

{% hint style="info" %}
### Using your own login details

If you go to the `local.json` file, you can elements to set the username and the password of the admin account for your Empirica Admin Panel.

For example:

```javascript
{
  "admins": [
    {
      "username": "admin",
      "password": "p5yycpd2yrg"
    }
  ]
}
```

You can set the information you want here. Now you can run your app with:

```text
meteor --settings local.json
```

This will allow you to open the admin panel with your own login details instead of the one automatically generated.
{% endhint %}

### Configuring your app

1. Click "configuration" in the upper right corner to access app settings. This

   is where you'll define all your experimental conditions.

2. Click `factors` in the menu bar at the top. This is where you set the most

   basic parameters for your experiment. The only default parameter is

   playerCount. Add a new factor by clicking the `+` button. Set the name to

   `one` and the value to `1`, then click `Create Factor Value`.

3. Click `treatments`. This is where you combine factors to create an

   experimental condition. Create a new treatment using the factor you just

   created and name whatever you want.

4. Click `Lobby Configurations` and create a new lobby configuration using the

   default settings.

### Creating an experiment session

1. Click `monitoring` in the upper right corner to toggle your admin panel from

   configuration to monitoring. This is where you'll interact with all the

   Empirica features that have to do with actually running experiments.

2. Click `New Batch` in the batches panel, select the treatment you just

   created, and click `Create Batch`.

3. Press the `► Start`  to start the batch.

### Testing the experiment as a participant

1. Navigate to [http://localhost:3000/](http://localhost:3000/) \(or click the `► Open App` button on the top right\) .
2. Follow through the consent, identification \(this would be, e.g., an MTurk/Prolific

   ID, but you can enter whatever you want right now\), instruction pages, and attention check \(you have to answer correctly\).

3. You're running an experiment! This is the default app, just a slider with 10

   rounds of input. We're going to edit that.

For more information about testing the experiment locally, see our [guide on running your experiment](../../getting-started/quick-test.md).

## Creating the experiment title

The `client/main.html` is the html point of entry for the React code that determines what a participant sees when they are in your experiment. **You should NOT change anything in this file** except for the text in between the `<title></title>` tags. There you can put the title you want to appear at the top of the tab when people are using your experiment \(e.g., "My First Experiment"\).

## Cleaning up the styling

The styling of an Empirica experiment is set in `.client/main.less`. **Less** is very similar to CSS. 

The default styling of the experiment is a bit complicated to understand, and to make our experiment look good we want to tweak a few things. Let's start by replacing the code in `main.less` with this code:

```css
/* ----------------
- default styling -
---------------- */

.title {
  text-align: center;
}

input[type="text"],
input[type="number"] {
  padding: 0.3rem 0.5rem;
  border-radius: 0.3rem;
  border: 1px solid lightgray;
}

input[type="radio"] {
  margin-right: 0.5rem;
}

.content {
  display: flex;
  flex-wrap: wrap;
}

.content > * {
  padding: 0.5rem;
  box-sizing: border-box;
}

.player-profile {
  display: flex;
  flex-direction: column;
  width: 20%;
  align-items: center;

  .profile-avatar {
    width: 8rem;
    height: 8rem;
    margin: 3rem 0;
  }

  .profile-score {
    text-align: center;
    span {
      font-size: 3rem;
      font-weight: bold;
    }
  }

  .timer {
    margin-top: 2rem;
    text-align: center;

    .seconds {
      font-size: 3rem;
      // Helvetica has tabular numbers (fixed width), which looks better
      // for a timer, doesn't jump all over the place.
      font-family: Helvetica, Arial, sans-serif;
      color: green;
    }
    &.lessThan10 {
      .seconds {
        color: orange;
      }
    }
    &.lessThan5 {
      .seconds {
        color: red;
      }
    }
  }
}

.task {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 50%;

  .task-image {
    text-align: center;
    margin-bottom: 1rem;
  }

  .task-question {
    width: 100%;
    text-align: center;
  }

  .task-response {
    width: 100%;

    form {
      display: flex;
      justify-content: center;
      margin-top: 1.5rem;
    }

    .response-submitted {
      margin-top: 1.5rem;
      background-color: #e8e8e8;
      padding: 2rem;
      border-radius: 0.4rem;

      h5 {
        margin: 0 0 0.5rem;
        font-size: inherit;
      }
    }
  }
}

.social-exposure {
  width: 30%;

  .alter {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 1rem;

    img {
      width: 6rem;
      height: 6rem;
      margin-right: 3rem;
    }

    .range {
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}

button {
  padding: 0.8rem 1.2rem;
  border-radius: 0.3rem;
  border: 1px solid lightgray;
  background-color: #eee;
  cursor: pointer;

  &:hover {
    background-color: #ddd;
  }
}

.instructions,
.quiz,
.exit-survey {
  button {
    margin: 2rem 1rem 0 0;
  }

  label {
    display: block;
    margin: 2rem 0 1rem;
  }

  textarea {
    padding: 0.3rem 0.5rem;
    border-radius: 0.3rem;
    border: 1px solid lightgray;
  }
}

.exit-survey {
  .form-line {
    display: flex;

    & > div {
      margin-right: 30px;

      textarea {
        min-height: 90px;
        width: 100%;
      }
    }

    &.thirds {
      & > div {
        flex-grow: 0;
        flex-shrink: 0;
        flex-basis: 30%;

        label {
          min-height: 36px;
        }
      }
    }
  }
}

.finished {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

## Configuring the task page

The root file that determines what is displayed to the participant \(i.e., the user-side\) during a game is located at `/client/game/Round.jsx`.

By default, this is divided into two main components, `Task` and `SocialExposure`. The `Task` itself is composed of `TaskStimulus` which contains the stimulus \(e.g., a survey question — or in this example, an estimation task\) and `TaskResponse` which contains the input for users to response to the stimulus.

Let's start by adding a title to the `/client/game/Task.jx`  :

```jsx
import React from "react";

import TaskResponse from "./TaskResponse";
import TaskStimulus from "./TaskStimulus";

export default class Task extends React.Component {
  render() {
    return (
      <div className="task">
        <h3 className="title">Task</h3>
        <TaskStimulus {...this.props} />
        <TaskResponse {...this.props} />
      </div>
    );
  }
}

```

### Adding content to the task stimulus

Now, let's edit `/client/game/TaskStimulus.jsx` . 

{% hint style="info" %}
Although Empirica is built on top of a sophisticated Meteor framework, all we have to interact with directly is standard JavaScript within the React.js framework. React components have a `this.props` which contains elements passed into it. These props can be extracted with destructuring \(the `const {} = this.props` part\). Notice that the props here include `round`, `stage`, and `player`. These are your interface with Empirica, and allow you to both read and set data for the state of the experiment.
{% endhint %}

All we're doing here is adding an image and question to set up the display. Later, we'll make the image path and question text configurable by reading in values from the `round` prop.

`/client/game/TaskStimulus.jsx`:

```jsx
import React from "react";

export default class TaskStimulus extends React.Component {
  render() {
    const { round, stage, player } = this.props;

    const imagePath = "/experiment/images/candies.jpg";
    const questionText = "How many candies are in the jar?";
    return (
      <div className="task-stimulus">
        <div className="task-image">
          <img src={imagePath} height={"300px"} />
        </div>
        <div className="task-question">
          <b>Please answer the following question:</b>
          <br />
          {questionText}
        </div>
      </div>
    );
  }
}
```

### Adding the candy jar image

You can download the candy jar image we use [here](https://raw.githubusercontent.com/empiricaly/your-first-experiment/master/public/experiment/images/candies.jpg).

Then, in the `public` directory, create the following directories: `/experiment/images/` and add the candy jar image there so that it matches the path we just set: `"/experiment/images/candies.jpg"` .

The `public` directory allows you to store static assets, such as images, that can then be accessed on the client side.

### Customizing the input field

In this section, we are going to change the input type from a slider to a numeric input box.  To do so, go to the `client/game/TaskResponse.jsx` file. You can see that there are some "handle" functions \(they don't need to be named this way, but clear names are helpful\) that will execute on certain events. There are also different "render" functions that will be called further down in the main `render` based on different conditions \(e.g., if the player has already submitted for this stage\).

Change the `renderSlider` function. First let's change it's name to `renderInput` . Then, replace the slider with a numeric input box. Because we're asking people to estimate the number of candies in a jar, we'll set the minimum value to one. 

{% hint style="info" %}
We recommend adding additional form verification methods beyond the default to ensure a smooth user experience. Note that if you do choose to use a slider input, we strongly recommend using the Empirica slider, which is made with experimenters in mind — no default values that might create anchoring effects.
{% endhint %}

```jsx
renderInput() {
  const { player } = this.props;
  const value = player.round.get("value");
  return (
    <input
      type={"number"}
      min={1}
      onChange={this.handleChange}
      value={value}
      required
    />
  );
}
```

Notice that this method uses the `player` data to control the form input. This player data is created with `player.round.set()` in the `handleChange` method which is called every time the input is updated. This method will save this data to the database on the server, automagically.

The sliders and inputs don't send exactly the same data format, so we have to change the `handleChange` function. 

```jsx
handleChange = event => {
  const value = Number(event.currentTarget.value);
  const { player } = this.props;
  player.round.set("value", value);
};
```

Because we changed the name of the "render" function to `renderInput` we need to change this in the main `render` ; instead of calling `this.renderSlider()` it should be called `this.renderInput()`.

```jsx
  render() {
    const { player } = this.props;

    // If the player already submitted, don't show the slider or submit button
    if (player.stage.submitted) {
      return this.renderSubmitted();
    }

    return (
      <div className="task-response">
        <form className="task-response-form" onSubmit={this.handleSubmit}>
          {this.renderInput()}

          <button type="submit">Submit</button>
        </form>
      </div>
    );
  }
```

## Configuring stimulus content

The next step is to allow the experimenter to modify the stimulus without having to update the experiment code directly. We'll accomplish this by adding a JavaScript object to a `constants.js` file \(the name of the file in itself is not important, as long as you are consistent\), that will contain all the stimulus information, and then use that in the experiment via the `Empirica.gameInit` method.

This all happens server-side, so you need to create this file in your `/server/` directory instead of your `/client/` directory. Create a file `/server/constants.js`, and add the following code to it.

```javascript
export const taskData = {
  candies: {
    path: "/experiment/images/candies.jpg",
    questionText:
      "The jar in this image contains nothing but standard M&M's.  How many M&M's are in the jar?",
    correctAnswer: 797
  },
  survey: {
    questionText:
      "A 2014 survey asked Americans whether science and technology make our lives better (easier, healthier, more comfortable).  What percentage of respondents agreed that science and technology are making our lives better?",
    correctAnswer: 80.5
  }
};
```

{% hint style="info" %}
The **export** keyword allows us to access this JavaScript object in another `.js` or `.jsx` file.
{% endhint %}

### Using data from `constants.js`

Next, we need to use this data in the `Empirica.gameInit` callback in `/server/main.js`. This is an important event that sets up the rounds and stages of a game. 

{% hint style="info" %}
For more about the life cycle of an Empirica app and its callback, consult [our guide](../../overview/lifecycle.md).
{% endhint %}

You can see that this file has a few imports and then most of the code is within `Empirica.gameInit`. In gameInit, you can see that it is going through each player and assigns them some data such as a score and an avatar. Then it is doing something 10 times \(using `_times()` from [underscore.js](http://underscorejs.org); a library of convenient JavaScript tools\): It is creating 10 rounds, each with one stage.

First, we import the `constants.js` data with `import { taskData } from "./constants";` added to the head of `main.js`.

We are going to create a network of players. We prepared a node for each player based on their position, and then we assign each player a set of "neighbors": Every possible node except a player's own node. This means this is a full network. Of course, you could create a different type of network if you wanted to. The neighbours will see each other's responses in a social stage we are going to create.

`/server/main.js`

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
  })

});

```

An important part of what is being done is here is adding a `data` object to the rounds. Any value in this object can then be access with `round.get()` .

### Incorporating dynamic round data in Task.jsx

Finally, we're ready to incorporate our new configurable task data into `Task.jsx`. We'll do this in `TaskStimulus.jsx`

It's easy! All we do is change the value of the constants to pull dynamically with `round.get()` instead of setting them manually:

```javascript
const imagePath = round.get("imagePath");
const questionText = round.get("questionText");
```

We also add logic so that we only display an image if a path is given:

```jsx
<div className="task-image">
  {imagePath && <img src={imagePath} height={"300px"} />}
</div>
```

## Adding social information

Social information is included in the default Empirica template via `SocialExposure.jsx`. However, nothing will show if we don't have any other players!

First, create a multi-player game by adding a new factor value to playerCount \(following the same procedure as before\) with more than one player, then creating a new treatment and a new batch. You can then launch the Empirica app multiple times in the same web browser by clicking `New Player` in the header tab. The game will start once the required number of players have entered the lobby.

In this example we're going to modify the default behavior so that \(a\) the social information only shows after participants have entered their initial response, and \(b\) social information shows only for a participant's network neighbors.

Within each `round` are multiple `stages`. We can add more stages by returning to the `Empirica.gameInit` method and adding the following:

```javascript
round.addStage({
  name: "social",
  displayName: "Social Information",
  durationInSeconds: 120
});
```

We can access the stage information \(including the name\) within the app, so we're going to modify `Round.jsx` to display the `SocialExposure` component only when `stage.name === "social"`:

```jsx
{
  stage.name === "social" && (
    <SocialExposure stage={stage} player={player} game={game} />
  )
}
```

Finally, we'll modify `SocialExposure.jsx` to only show information for players listed in `player.get("neighbors")` by replacing the declaration for `const otherPlayers` with:

```javascript
const otherPlayers = game.players.filter(p =>
  player.get("neighbors").includes(p.get("nodeId"))
);
```

_This sets the `otherPlayers` to players that are not the current player and players that are in the current player's neighbors \(their node is in their list of neighbors\)._

We're also going to remove the slider element, since we are not using that in this example, and just show the number:

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

This simple feature shows just how powerful Empirica can be. Just by virtue of storing user information with `player.set()` and displaying that information in the `SocialExposure` component, the participant's interface is automatically updated. This is due to the way React.js works with Meteor: any time a piece of information passed as one of the props is updated, then the display re-renders to show the new information.

Now, let's add a title to make this component match the other two. And let's make a condition to determine whether the phrase, "There are X other players:", should be plural or not.

```jsx
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
```

## Adding treatments and factors

One of the key features of Empirica is that you can think like a scientist and control your app with experimental conditions. To show how this works we'll add a factor, allowing us to modify the stage length without changing the code.

First, we add the new factor through the admin panel by navigating to the factors interface and clicking "New Factor". We will create a `stageLength` factor of type `Integer`. 

Then, add a new value \(such as `60`, for 60 seconds\), make a new treatment, and create a new batch. 

{% hint style="info" %}
Because we are replacing timing of the stages with this factor, you should make this factor `Required` so that it has to be entered when creating treatment. Otherwise, if you forget to set a `stageLength`, the experiment will break.

You can also archive your previous treatments that do not have a `stageLength`, to avoid being tempted to use them and declutter your interface. You'll always be able to unarchive these if necessary.
{% endhint %}

We use this treatment information in the `Empirica.gameInit` callback by simply changing the definition in `/server/main.js` to be `durationInSeconds: game.treatment.stageLength` for both stages.

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
If you are trying to edit elements \(e.g., change the styling\) you might want the stage length to be longer so you have time to work on your changes without creating new games over and over again. Consider creating a stageLength factor that is longer and make a 'test' or 'dev' treatment with this factor.
{% endhint %}

## Calculating player score with callbacks

Empirica provides a set of methods that will run at the start and end of each round and stage. These can be found in `/server/callbacks.js`.

{% hint style="info" %}
To learn more about callbacks, see [our guide on the life cycle of an Empirica experiment](../../overview/lifecycle.md).
{% endhint %}

By default, the score is equal to the total sum of responses, but this is not very informative. We'll modify this to show percentage of error subtracted from 1:

```javascript
// onRoundEnd is triggered after each round.
// It receives the same options as onGameEnd, and the round that just ended.
Empirica.onRoundEnd((game, round) => {
  game.players.forEach(player => {
    let value = player.round.get("value") || 0;
    const prevScore = player.get("score") || 0;
    let newScore = 1 - value / round.get("correctAnswer");
    if (newScore < 0) newScore = 0;
    player.set("score", prevScore + newScore);
  });
});
```

These scores are compared to the `correctAnswer` from our `constants.js` file.

## Exporting the Treatments and Factors

The Treatments and Factors that you have created are all save on your local database for your meteor app. This means that you will no longer have access to them if you share this someone through GitHub, if you reset your app, or if you deploy your app.

**You should export these Treatments and Factors so that they can be imported back in if need be.**

Go to `Configuration` on the admin panel \(where you create Treatments and Factors\) and click the `Export` button. This will allow you to download a `.yaml` file of your Treatments and Factors. Add it to the root of your Empirica experiment folder. It can be good to rename it to something like `factors.yaml`.

Now, if you need to recreate your Treatments and Factors, you can just go `Configuration` on the admin panel, click `Import` and select your `.yaml` file.

## Next Step: Chats

Exchanging numeric information is great, but it could be interesting to have participants chat about their responses. Go to part 2 of this tutorial to learn how to use Empirica chats!

