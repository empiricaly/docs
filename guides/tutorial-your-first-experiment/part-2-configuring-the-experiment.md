# Part 2: Configuring the Experiment

## Configuring the task page

The main file that determines what is displayed to the participant \(i.e., the user-side\) during a game is located at `/client/game/Round.jsx`.

By default, this is divided into two main components, `Task` and `SocialExposure`. The `Task` itself is composed of `TaskStimulus` which contains the stimulus \(e.g., a survey question — or in this example, an estimation task\) and `TaskResponse` which contains the input for users to response to the stimulus.

Let's start by adding a title to the `/client/game/Task.jx`  so that the component looks like this:

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

{% hint style="info" %}
React components have a `this.props`  object which contains elements passed into it. These props can be extracted with destructuring \(the `const {} = this.props` part\). Notice that the props here include `round`, `stage`, and `player`. These are your interface with Empirica, and allow you to both read and set data for the state of the experiment.
{% endhint %}

All we're doing here is adding an image and question to the `client/game/TaskStimulus.jsx` so that it looks like this:

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

{% hint style="info" %}
The `public` directory allows you to store static assets, such as images, that can then be accessed on the client side.
{% endhint %}

### Customizing the input field

In this section, we are going to change the input type from a slider to a numeric input box.  To do so, go to the `client/game/TaskResponse.jsx` file. 

You can see that there are some "handle" functions \(they don't need to be named this way, but clear names are helpful\) that will execute on certain events. 

There are also different "render" functions that will be called further down in the main `render` based on different conditions \(e.g., if the player has already submitted for this stage\).

Change the `renderSlider` function. First let's change it's name to `renderInput` . Then, replace the slider with a numeric input box. Because we're asking people to estimate the number of candies in a jar, we'll set the minimum value to one. 

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

{% hint style="info" %}
We recommend adding additional form verification methods beyond the default to ensure a smooth user experience. Note that if you do choose to use a slider input, we strongly recommend using the Empirica slider, which is made with experimenters in mind — no default values that might create anchoring effects.
{% endhint %}

Notice that this method uses the `player` data to control the form input. This player data is created with `player.round.set()` in the `handleChange` method which is called every time the input is updated. This method will save this data to the database on the server, automagically.

The sliders and inputs don't send exactly the same data format, so we have to change the `handleChange` function to look like this:

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

{% hint style="info" %}
**Careful with the brackets!**

You might notice that a react component, like `TaskResponse.jsx` has a few import lines at the top of the file and then starts with this type of line and opening of curly brackets {}

```jsx
export default class TaskResponse extends React.Component { 
```

This curly bracket will close on the last line of the file.

You will also notice that the main render of a component also opens curly brackets {} which close before those of the overall component.

So when you are changing the render of a component, **careful not to delete the final bracket that closes the component**, otherwise you will get an error such as `'}' expected`.
{% endhint %}

Overall, `TaskResponse.jsx` should look like this:

```jsx
import React from "react";

export default class TaskResponse extends React.Component {
  handleChange = event => {
    const value = Number(event.currentTarget.value);
    const { player } = this.props;
    player.round.set("value", value);
  };

  handleSubmit = event => {
    event.preventDefault();
    this.props.player.stage.submit();
  };

  renderSubmitted() {
    return (
      <div className="task-response">
        <div className="response-submitted">
          <h5>Waiting on other players...</h5>
          Please wait until all players are ready
        </div>
      </div>
    );
  }

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
}

```

## Configuring stimulus content

The next step is to allow the experimenter to modify the stimulus without having to update the experiment code directly. We'll accomplish this by adding a JavaScript object to a `constants.js` file \(the name of the file in itself is not important, as long as you are consistent\), that will contain all the stimulus information, and then use that in the experiment via the `Empirica.gameInit` method.

This all happens server-side, so you need to create this file in your `server/` directory instead of your `client/` directory. Create a file `server/constants.js`, and add the following code to it.

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

First, we import the `constants.js` data with `import { taskData } from "./constants";` added to at the top of `server/main.js`.

We are going to create a network of players. We prepared a node for each player based on their position, and then we assign each player a set of "neighbors": Every possible node except a player's own node. This means this is a full network. Of course, you could create a different type of network if you wanted to. The neighbours will see each other's responses in a social stage we are going to create.

Replace the code in your `server/main.js` with the following code:

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

### Incorporating dynamic round data in TaskStimulus.jsx

Finally, we're ready to incorporate our new configurable task data into `TaskStimulus.jsx`

All we do is change the value of the constants to pull dynamically with `round.get()` instead of setting them manually. Replace the lines that declare the imagePath and the questionText with these:

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

Overall, it should look like this:

```jsx
import React from "react";

export default class TaskStimulus extends React.Component {
  render() {
    const { round, stage, player } = this.props;

    const imagePath = round.get("imagePath");
    const questionText = round.get("questionText");
    return (
      <div className="task-stimulus">
        <div className="task-image">
          {imagePath && <img src={imagePath} height={"300px"} />}
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

## Calculating player score with callbacks

Empirica provides a set of methods that will run at the start and end of each round and stage. These can be found in `server/callbacks.js`.

{% hint style="info" %}
To learn more about callbacks, see [our guide on the life cycle of an Empirica experiment](../../overview/lifecycle.md).
{% endhint %}

By default, the score is equal to the total sum of responses, but this is not very informative. We'll modify this to show percentage of error subtracted from 1. Replace the `Empirica.onRoundEnd` code in `server/callbacks.js`  with this:

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

