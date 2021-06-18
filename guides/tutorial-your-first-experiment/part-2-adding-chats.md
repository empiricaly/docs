# Part 4: Adding Chats

## Installing the Empirica Chat

First, you should stop running your experiment \(e.g., press **cntrl+c** in the command line\) to [install the package for the Empirica chat](https://www.npmjs.com/package/@empirica/chat).

Install it by running this in the command line:

```text
meteor npm install --save @empirica/chat
```

## Adding a chat to the social stage

To add a chat to a component, you have to import it at the top of the `client/game/SocialExposure.jsx` component:

```jsx
import { Chat } from "@empirica/chat";
```

Then, still in the `client/game/SocialExposure.jsx` component, we add a chat component by replacing the return of the main render with this code:

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
        <div>
          <p className="chat-title"><strong>Chat</strong></p>
          <Chat player={player} scope={round} />
        </div>
      </div>
    );
```

Note that we need to assign this `player` as a property of the chat. We also need to define a `scope`, which determines in which element of Empirica the chat is created \(the game, every round, every stage, etc.\). Here, we want one chat per round, so we set the scope to the `round`.

This also means we need to call out the round from the props of this component. Replace this line in the main render:

```jsx
const { game, player } = this.props;
```

With this one:

```jsx
const { game, player, round } = this.props;
```



Overall, your `SocialExposure.jsx` component shoud look like this:

```jsx
import React from "react";
import { Chat } from "@empirica/chat";

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
    const { game, player, round } = this.props;

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
        <div>
          <p className="chat-title"><strong>Chat</strong></p>
          <Chat player={player} scope={round} />
        </div>
      </div>
    );
  }
}

```

## Styling the chat

You can now see the chat in the social stage. However, it does not look very pretty. We could use a `customClassName` on the chat component to style all the elements of the chat. However, it is easier for us to just style a few of the chat's elements.

Add this bit of styling at the end of the `client/main.less` :

```css
/* -----------------
- Styling the chat -
----------------- */

.chat-title {
  text-align: center;

  color: white;
  background-color: #394b59;

  border: 1px solid hsl(0, 0%, 75%);

  margin: 1rem 0 0 0;
}

.empirica-chat-container {
  width: 100%;

  .chat {
    form {
      padding: 0;
    }
  }
}

.empirica-chat-container .chat form {
  padding: 0;
}

.chat-button-send, .chat-input {
  min-height: 50px;
}

.empirica-chat-container .chat form .chat-footer .chat-input {
  border-radius: 0%;
}

.empirica-chat-container .chat form .chat-footer .chat-button-send {
  border-radius: 0%;
}

.messages {
  border: 1px solid hsl(0, 0%, 75%);
  max-height: 300px;
}

.empirica-chat-container .chat .messages .message {
  margin: 2rem 0.5rem;
}
```

## Using factors to create conditions with and without chats:

You might want to create condition where there are chats and a condition without chats. We can do that with new Treatments and Factors. 

Go back to the Admin Panel and create a new `chat` factor as a `boolean` factor so it will be `true` or `false`.  Now  create a treatment with `chat` set to `true` and a treatment with `chat` set to `false`.

Now let's use this information render the chat only if the `chat` factor is set to `true` . Change the return of the main render of the  `SocialExposure.jsx` component with this code:

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
        {game.treatment.chat &&
          <div>
            <p className="chat-title"><strong>Chat</strong></p>
            <Chat player={player} scope={round} />
          </div>
        }
      </div>
    );
```

