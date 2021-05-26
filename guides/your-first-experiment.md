---
id: your-first-experiment
title: Your First Experiment
description: A tutorial for preparing your first Empirica experiment.
---

# Tutorial: Your First Experiment

If you want to compare your work to a finished version of the tutorial, you can find it [on GitHub](https://github.com/empiricaly/your-first-experiment/).

## Getting Started With Empirica

Although it won't do too much, Empirica works straight out of the box. The first step to customizing your Empirica app is to launch a test game so you can see your edits in action.

1. Follow the [setup](../getting-started/setup/) guide and [create your experiment](../getting-started/quick-start.md).
2. Follow the guide on [running your experiment](../getting-started/quick-test.md).
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

   `One` and the value to `1`, then click `Create Factor Value`.

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
2. Follow through the consent, identification \(this would be, e.g., an MTurk

   ID, but you can enter whatever you want right now\), instruction pages, and attention check \(you have to answer correctly\).

3. You're running an experiment! This is the default app, just a slider with 10

   rounds of input. We're going to edit that.

For more information about testing the experiment, see our [guide on running your experiment](../getting-started/quick-test.md).

## Creating the experiment title

The `client/main.html` is the html point of entry for the React code that determines what a participant sees when they are in your experiment. **You should NOT change anything in this file** except for the text in between the `<title></title>` tags. There you can put the title you want to appear at the top of the tab when people are using your experiment \(e.g., "My First Experiment"\).

## Configuring the task page

The root file that determines what is displayed to the participant \(i.e., the user-side\) during a game is located at `/client/game/Round.jsx`.

By default, this is divided into two main components, `Task` and `SocialExposure`. The `Task` itself is composed of `TaskStimulus` which contains the stimulus \(e.g., a survey question — or in this example, an estimation task\) and `TaskResponse` which contains the input for users to response to the stimulus.

### Adding content to the task stimulus

Edit `/client/game/TaskStimulus.jsx` to the following. Although Empirica is built on top of a sophisticated Meteor framework, all we have to interact with directly is standard JavaScript with a ReactJS framework.

React components have a `this.props` which contains elements passed into it. These props can be extracted with destructuring \(the `const {} = this.props` part\). Notice that the props here include `round`, `stage`, and `player`. These are your interface with Empirica, and allow you to both read and set data for the state of the experiment.

All we're doing here is adding an image and question to set up the display. Later, we'll make the image path and question text configurable by reading in values from the `round` prop.

`/client/game/TaskStimulus.jsx`:

```jsx
import React from "react";

export default class TaskStimulus extends React.Component {
  render() {
    const { round, stage, player} = this.props;

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

### Customizing the input field

All we're going to do in this section is change the input type from a slider to a numeric input box.  To do so, go to the `client/game/TaskResponse.jsx` file. You can see that there are some "handle" functions \(they don't need to be named this way, but clear names are helpful\) that will execute on certain events. There are also different "render" functions that will be called further down in the main `render` based on different conditions \(e.g., if the player has already submitted for this stage\).

Change the `renderSlider` function. First let's change it's name to `renderInput` . Then, replace the slider with a numeric input box. Because we're asking people to estimate the number of candies in a jar, we'll set the minimum value to one. We recommend adding additional form verification methods beyond the default to ensure a smooth user experience. Note that if you do choose to use a slider input, we strongly recommend using the Empirica slider, which is made with experimenters in mind — no default values that might create anchoring effects.

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

Notice that this method uses the `player` data to control the form input. This player data is created with `player.round.set()` in the `handleChange` method which is called every time the input is updated, and will save this data to the database on the server, automagically.

The sliders and inputs don't send exactly the same data format, so we have to change the `handleChange` function. 

```jsx
handleChange = event => {
  const value = Number(event.currentTarget.value);
  const { player } = this.props;
  player.round.set("value", value);
};
```

Because we changed the name of the "render" function to `renderInput` we need to change this in the main `render` ; instead of calling `this.renderSlider()` it should call `this.renderInput()`.

```jsx
render() {
    const { player } = this.props;

    // If the player already submitted, don't show the slider or submit button
    if (player.stage.submitted) {
      return this.renderSubmitted();
    }

    return (
      <div className="task-response">
        <form onSubmit={this.handleSubmit}>
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

The `export` word allows us to access this JavaScript object in another `.js` or `.jsx` file.

### Using data from `constants.js`

Next, we need to use this data in the `Empirica.gameInit` callback in `/server/main.js`. This is an important event that sets up the rounds and stages of a game. For more about the life cycle of an Empirica app and its callback, consult [our guide](../overview/lifecycle.md).

You can see that this files has a few imports and then most of the code is within `Empirica.gameInit`. In gameInit, you can see that it is going through each player and assigning them some data such as a score and an avatar. Then it is doing something 10 times \(using `_times()` from [underscore.js](http://underscorejs.org); a library of convenient JavaScript tools\): It is creating 10 rounds, each with one stage.

First, we import the `constants.js` data with `import { taskData } from "./constants";` added to the head of `main.js`.

We're also going to update this method to randomly assign each player a set of "neighbors" to follow. We'll use this information when we update `SocialExposure.jsx`.

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

		// Assign each node as a neighbor with probability 0.5
		const networkNeighbors = nodes.filter(node => Math.random() >= 0.5);
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

### Adding the candy jar image

You can download the candy jar image we use [here](https://raw.githubusercontent.com/empiricaly/your-first-experiment/master/public/experiment/images/candies.jpg).

Then, in the `public` directory, create the following directories: `/experiment/images/` and add the candy jar image there so that it matches the path set in `constants.js`: `"/experiment/images/candies.jpg"` .

The `public` directory allows you to store static assets ,such as images, that can then be accessed on the client side.

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

The first thing to do is create a multi-player game by adding a new factor value to playerCount \(following the same procedure as before\) with more than one player, then creating a new treatment and a new batch. You can then launch the Empirica app multiple times in the same web browser by clicking `New Player` in the header tab. The app will start once the required number of players has entered the lobby.

In this example we're going to modify the default behavior so that \(a\) the social information only shows after subjects have entered their initial response, and \(b\) social information shows only for a subject's network neighbors.

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
  p._id != player._id && player.get("neighbors").includes(p.get("nodeId"))
);
```

_This sets the `otherPlayers` to players that are not the current player and players that are in the current player's neighbors \(their node is in their list of neighbors\)._

We're also going to remove the slider element, since we are not using that in this example, and just show the number:

```jsx
  renderSocialInteraction(otherPlayer) {
    const value = otherPlayer.round.get("value");
    return (
      <div className="alter" key={otherPlayer._id}>
        <img src={otherPlayer.get("avatar")} className="profile-avatar" />
        Guess: {value}
      </div>
    );
  }
```

This simple feature shows just how powerful Empirica can be. Just by virtue of storing user information with `player.set()` and displaying that information in the `SocialExposure` component, the subject interface is automatically updated. This is due to the way ReactJS works with Meteor: any time a piece of information passed as one of the props is updated, then the display re-renders to show the new information.

## Using treatment factors

One of the key features of Empirica is the ability to think like a scientist and control your app with experimental conditions. To show how this works we'll add a factor, allowing us to modify the stage length without changing the code.

First, we add the new factor through the admin panel by navigating to the factors interface and clicking "New Factor". We will create a `stageLength` factor of type `Integer`. 

{% hint style="info" %}
Because we are replacing timing of the stages with this factor, you should make this factor `Required` so that it has to be entered when creating treatment. Otherwise, if you forget to set a `stageLength`, the experiment will break.

You can also archive your previous treatment that does not have a `stageLength`, to avoid being tempted to use it.
{% endhint %}

Then, add a new value \(such as `60`, for 60 seconds\), make a new treatment, and create a new batch. You can archive your earlier treatment\(s\) so they don't clutter up the interface. You'll always be able to unarchive these if necessary.

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

## Calculating player score with callbacks

Empirica provides a set of methods that will run at the start and end of each round and stage. These can be found in `/server/callbacks.js`.

To learn more about callbacks, see [our guide on the life cycle of an Empirica experiment](../overview/lifecycle.md).

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

Go to `Configuration` on the admin panel \(where you create Treatments and Factors\) and click the `Export` button. This will allow you to download a yaml file of your Treatments and Factors. Add it to the root of your Empirica experiment folder. It can be good to rename it to something like `factors.yaml`.

Now, if you need to recreate your Treatments and Factors, you can just go `Configuration` on the admin panel, click `Import` and select your `.yaml` file.

## Next steps and future work

In this tutorial, we have created a simple that prompts subjects to complete numeric estimation tasks and allows them to revise their answers while observing the responses of other subjects.

Right now, this app ONLY works for social conditions ; if you don't have any neighbors, things will look a bit weird because the stage name is called "Social Information" and there are no instructions.

Here are some examples of how we can use Empirica features to expand the functionality of this experiment:

* Add logic to use different stage names for social and non-social conditions
* Add configurable task instructions to the constants.js that say things like

  "Please answer the question, you will have a chance to revise your answer" and

  "Please revise your answer"

* Add a factor "questionSet" that takes a comma-separated list of question

  identifiers, and uses only those questions. This allows different treatments

  to use different conditions, based on the same constants.js file.

* If you want to use more complex network structures, you could store those in a

  json format and import them in main.js

There are also several very basic features from the Intro and Outro we haven't touched yet that you'll need to update:

* The Consent form
* The Instructions pages
* The attention check
* The Exit Survey and Thank You page

