# The Processes and Elements of an Empirica Experiment

### What is a React.js component?

A React.js component is the main building block of the frontend of your Empirica App. There are many tutorials online to help with your understanding of React.js.

In Empirica, we assign certain components for the Intro Steps, the Round, the Exit Steps, and a few other elements of the Game.

A component is composed of:

* **states** that affect what is rendered for the user, but that can also be changed by the user interacting with the rendered elements of the app.
* **props** provided from other components that can be used to determine what is rendered for the user.
* **other components** that it imports and builds into what it renders for the user.
* a **render function** that determines what is shown to the user with a mix of HTML \(with `<> tags`\) and JavaScript \(with `{}`\).

Each component is generally made into one `.jsx` file. Components can be imported into each other to build more complex components. Usually, the type of components used in Empirica are **class-based**.

A major perk of React.js components is that whenever one of their states change \(e.g., because of the action of user\), it will **refresh every component** **affected** and update what is rendered depending on the new states. This makes for web apps that live update what they look like and do. This is particularly useful for Empirica because you want to update what you show to players depending on states such as which responses they have given, which stage of the Game they are at, what other players are doing, etc.

Hence, there are some elements of a component you might want to render differently depending on certain props and states. You can use syntax such as `{ condition ? true : false }`or `{ condition && true}` where _condition_ is a condition that is tested, _true_ is what is rendered if this condition tests true, and _false_ is what is rendered if this condition tests false.

A component might look like this:

```jsx
// Importing elements and other components
import React, { Component } from 'react'
import GivingResponse from './GivingResponse'

export default class Questionnaire extends Component {
    // The state of the questionnaire
    state = {
        showHint: false
    }

    // Handling if the player clicks to show hint (toggles the hint on and off)
    handleShowHint = () => {
        this.setState({ showHint: !this.state.showHint })
    }

    render() {

        // Getting the props
        const { player } = this.props;

        return (
            <div>
                <p> What is the name of the first person to set foot on the moon?</p>

                {/* A button that affects the conditional that
                 determines whether to show the hint or not */}
                <button onClick={this.handleShowHint()}>Show hint</button>
                {this.state.showHint && <p className="hintcolour">He was American.</p>}

                {/* Importing another component for the player to give their answer.
                 We pass down the prop of the player */}
                <GivingResponse player={player} />
            </div>
        )
    }
}
```

### How can I get rid of the "Waiting on the other players. Please wait until all players are ready"?

This is not something directly specific to Empirica, it's just how the basic template is set up.

 As shown [here](../overview/lifecycle/customising-when-players-submit-stages.md), it is possible to use the information that a Player has submitted their Stage to render something different than before they submit their Stage. In template, this happens in the `TaskResponse.jsx`.  You can see that two **render functions** have been created: `renderSubmitted()` and `renderInput()`. In the `render()` part of the component, you can see that there is an **if conditional** that determines whether the render function used will be `renderSubmitted()`, which will prevent the `renderSubmitted()` from being called \(and thereby hiding the slider and method of response from the player\):

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
          {" "}
          <button type="submit">Submit</button>
        </form>
      </div>
    );
  }
```

You can get rid of this message by taking out the **if conditional**, or changing what `renderSubmitted()` does.

### How can I redirect a player if I detect they are using a certain browser or a mobile device?

You might not want players to join your game from a mobile or tablet, nor from certain browsers. To do so you can use [react-device-detect](https://www.npmjs.com/package/react-device-detect) and modify the first page of your experiment \(e.g., the consent page, the NewPlayer page, or the first page of your Intro Steps\) to prevent them from continuing the experiment if you detect the device or browser that you do not want.

To install react-device-detect use:

```text
meteor npm install react-device-detect
```

react-device-detect has different Booleans that you can import and use in one of your components to detect whether the player is using a certain browser.

* isMobile for whether they are using a mobile device
* isChrome for whether they are using Chrome
* isFirefox for whether they are using Firefox
* isSafari for whether they are using Safari
* ...

And others than you can find out about [here](https://www.npmjs.com/package/react-device-detect).

Import them into the component with:

```text
import { isMobile, isFirefox, isSafari, isChrome } from 'react-device-detect';
```

For example, if you want to render a different consent form if the player is using a mobile device or is not using Chrome:

```jsx
return !isMobile && isChrome ?
(
    <div>This is the consent form...</div>
) :
(
    <div>Please use a computer and Chrome.</div>
)
```

### How can I show a different Exit Step to players depending on whether they have finished the game or if the game was cancelled/had a problem?

In the `client/main.js` you set which components form the **Exit Steps** with `Empirica.exitSteps()`. You can use the `player.exitStatus` to separate out whether they have finished the game or if they were sent to the exit steps because the game was cancelled/had an issue and send them to different Exit Steps.

For example:

```jsx
Empirica.exitSteps((game, player) => {
    return player.exitStatus === "finished"
        ? [PostSurvey, Thanks]
        : [Sorry];
});
```

### Can players navigate back and forth between the Exit Steps?

For now, players cannot navigate back and forth between the Exit Steps. 

Each Exit Step has a name set by `static stepName = "";` and players can only move from one Exit Step to the next if the component has an element \(e.g., a button\) that will call the `onSubmit` prop. For example:

```jsx
<button type="button" onClick={onSubmit}>
    Finish this experiment
</button>
```

If you wanted multiple pages within the Exit Steps that players can navigate through, you could create a component within one Exit Step that has different components to form "pages" and with a state that knows which page it is at and navigating to and from them.

### Can a manually sent a player to an exit step?

You can have a piece of code to manually send a player to an exit stage if they do something \(e.g., you want to give them a quit button\) by using:

```text
player.exit("name of exit step")
```

where the string is the name of the exit stage you want to send them to.

### How can I use bots?

In the `server/bots.js` you can create bots for them to participate in your games.

See [this part](../guides/tutorial-your-first-experiment/part-3-adding-bots.md) of the tutorial for more details.

```jsx
Empirica.bot("bob", {
  // // NOT SUPPORTED Called at the beginning of each stage (after onRoundStart/onStageStart)
  // onStageStart(bot, game, round, stage, players) {},

  // Called during each stage at tick interval (~1s at the moment)
  onStageTick(bot, game, round, stage, secondsRemaining) {}

  // // NOT SUPPORTED A player has changed a value
  // // This might happen a lot!
  // onStagePlayerChange(bot, game, round, stage, players, player) {}

  // // NOT SUPPORTED Called at the end of the stage (after it finished, before onStageEnd/onRoundEnd is called)
  // onStageEnd(bot, game, round, stage, players) {}
});
```

