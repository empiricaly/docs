# Part 5: Adding Bots

## Adding bots in the factors and treatments

To add bots you need to go to the admin panel and create `botsCount` \(the name is important this time!\) factor as an integer and set it up with the number of bots you want in your study. Then create treatments that use this factor.

{% hint style="info" %}
Bots will replace one of the player slots. So if you want two human players and one bot, you need to create a game with a `playerCount` of  3.
{% endhint %}

## Programming a bot

In the `server/bots.js` you can create bots to be used based on the number of set in the `botsCount` factor.

Bots will be added to the `game.players` in the game. They have some special characteristics such as a `bot` name.

### **Submitting the stages**

Let's program `bob` to submit every stage if they haven't already. We do so by modifying the `onStageTick` method:

```jsx
// Called during each stage at tick interval (~1s at the moment)
onStageTick(bot, game, round, stage, secondsRemaining) {
    // Have the bot always submit their stage
    if (!bot.stage.submitted) {
      bot.stage.submit()
    }
}
```

### **Getting a value**

In the social stage, we want the bot to get a value and maybe chat. Let's create a conditional to tell the bot what to do when it is the social stage. If the both does not have a value for this round yet, let's set one for them as the mean of the values of the other players.

```jsx
// Called during each stage at tick interval (~1s at the moment)
onStageTick(bot, game, round, stage, secondsRemaining) {

  // Have the bot always submit their stage
  if (!bot.stage.submitted) {
    bot.stage.submit()
  }

  // if this is a social stage... 
  if (stage.name === "social") {
    // get the mean of the values of the other players and set it to the bot
    if (!bot.round.get("value")) {
      const nonBotPlayers = game.players.filter(p => p.bot === undefined)
      const sumValues = nonBotPlayers
        .map(p => Number(p.round.get("value")))
        .reduce((total, item) => (total += item), 0)

      const mean = Math.round(sumValues / nonBotPlayers.length)

      bot.round.set("value", mean)
    }
  }
}
```

### Bots and chats

If there is a chat, we want the bot to send a random message every 20 seconds.

To select a random message from an array we need to create a function to make a random choice. Let's add this function to a separate `.js` file on the server side: `server/randomFunctions.js` :

```javascript
//Function to randomly choose an element from an array:
export const choice = (array) => {
    var randomIndex = Math.floor(Math.random() * array.length);
    var randomElement = array[randomIndex];
    return randomElement;
}

```

Now, let's import this function at the top of the `server/bots.js` file:

```jsx
import { choice } from "./randomFunctions";
```

Now lets also add a list of messages to randomly chose from at the top of the `server/bots.js` file \(but below the import statements\):

```jsx
const botLanguage = [
  "Hi",
  "That's a good question!",
  "What do you think the correct answer is?",
  "This experiment is fun",
  "I also do human things"
]
```

_Feel free to add your own messages!_

Now, still in the conditional that it is the social round, add a conditional that will send a message if there is a chat in this game and if the remaining seconds are divisible by 20 \(to send a message every 20s\):

```jsx
// send a random message every 20s
if (game.treatment.chat && secondsRemaining % 20 === 0) {
  const chat = round.get("chat") ?? []

  chat.push({
    text: choice(botLanguage),
    player: {
      _id: bot._id,
      avatar: bot.get("avatar"),
      name: bot.bot
    }
  })

  round.set("chat", chat)
}
```

Notice that we get the `"chat"` from the `round`, this is because we set the `scope` of the chat to the `round`. 

If the chat is empty, we prepare an empty array, otherwise we take the messages in the chat as they are, and we push a new message from this bot with a random choice of text. We set these new messages to the round.

## Next steps and future work

In this tutorial, we have created a simple experiment that prompts participants to complete numeric estimation tasks and allows them to revise their answers while observing the responses of other participants. We have also added chats and bots.

Right now, this app ONLY works for social conditions ; if you don't have any neighbors, things will look a bit weird because the stage name is called "Social Information". 

Here are some examples of how we can use Empirica features to expand the functionality of this experiment:

* Add instructions for the different stages of the game.
* Add logic to use different stage names for social and non-social conditions
* Add configurable task instructions to the constants.js that say things like

  "Please answer the question, you will have a chance to revise your answer" and

  "Please revise your answer"

* Add a factor "questionSet" that takes a comma-separated list of question

  identifiers, and uses only those questions. This allows different treatments

  to use different conditions, based on the same constants.js file.

* If you want to use more complex network structures, you could store those in a

  json/js format and import them in main.js

There are also several very basic features from the Intro and Outro we haven't touched yet that you'll need to update:

* The Consent form
* The Instructions pages
* The attention check
* The Exit Survey and Thank You page

