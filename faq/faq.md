# Managing Players and Games

### Can I set a personalised admin login?

Yes, and you should, especially because, once deployed, reading the logs of your app to find the randomly generated admin password won't be as easy as reading it from your local console.

In your [settings file](faq.md#what-is-the-settings-file-settings-json), you should add a section to set admin user identifiers. If you deploy the app or launch the app locally with these settings, it will allow you to log into the Admin Panel with the identifiers you have created.

```text
"admins": [
        {
            "username": "myUsername",
            "password": "myPassword"
        }
    ],
```

### When are players playing synchronously and asynchronously? Can I modify this?

Players are **asynchronous during the Intro Steps** \(i.e., they can finish each part at their own pace\). At the **lobby**, players have to wait for each other to finish the Intro Steps. Then players are **synchronous during the Game** \(i.e., they must all finish a Stage before they can move on to the next Stage or Round\). Finally, once all the players have finished the Game, they are **asynchronous again during the Exit Steps**.

It is not recommended to try and modify this aspect of an Empirica app. You should consider what you want players to be doing at their own pace and what you want them to be doing synchronously. Remember that you can use your Treatments to modify how certain aspects of the game are presented throughout the app, and you can set different times for different Stages.

For more information about the life cycle of a game, see [here](../overview/lifecycle.md).

### Can players leave and re-join the app? Can players refresh the page?

Yes, the browser cache of the player records they Player id and the database records the status and position of the Player, so it knows whether the Player is in the Intro Steps, the Lobby, a specific Round and Stage, or the Exit Steps. If a player refreshes the page or leaves and comes back, they will be sent back to their position. 

However, remember that in Stages the timer is still ticking and during the Intro Steps other players might be waiting in the lobby; hence, it is not a good idea for players to leave the game.

It can be useful for players to know that they can safely refresh the page in case they have a graphical bug or if the page takes too long to render.

### How can I assign players to different versions of my game based on a condition set outside of the game?

Imagine you have players that have different favourite colors, and you want to create games with one player for each favourite color. This is currently hard to make with Empirica because the players are allocated to a game and then they are asked questions, not the other way round.

One solution could be to create a big game, and ask players their favourite color in the Intro Steps. Then, during the [gameInit\(\)](../overview/lifecycle.md) you assign participants to subgames. This might be complicated because you might have the wrong proportions of players per color. 

Another solution is to create the groups outside of Empirica and invite them one group at a time to a game.

Upcoming updates to Empirica will propose novel solutions to this problem.

### Retiring Players: How players can play again, even if they got gameFull, LobbyTimedOut, etc.

Players can only play once. If there was a problem \(e.g., the lobby timed out or the game was full\), or if you want players to play again, you can _retire_ them which allows them to play again.

In the **Admin Panel**,  in the **monitoring tab**, in the **players tab**, you can see the status of every Player. At the bottom of the page, you can select a type of **exit status** and retire every player with that status.

Once _retired_, players can refresh their page/return to the link of your game and they will be randomly allocated to a game with the same treatment as before. They are allocated to a game with the same treatment so that they don't discover a game with different conditions or instructions.

### I tried to retire a player but got this error: Failed to return players: Error: ID cannot exceed 256 characters in players update \[400\], what should I do?

This means that the player id of the player was too long. If you go to your data base you can find that player and modify their player id so that it is not as long.

### Where can I record player data?

#### To the player

One way of recording the data of players' responses is to set them to the `player` prop itself. You can do so with this command:

```text
player.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer for the player with:

```text
player.get("name of property")
```

This will be accessible in the `player` collection of your database \(see how to [download your data](faq.md#where-can-i-download-the-data)\).

#### To the player.stage

One way of recording the data of players' responses is to set them to the `player.stage` prop to identify a particular data/response of a particular player to a particular stage. You can do so with this command:

```text
player.stage.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer with:

```text
player.stage.get("name of property")
```

This will be accessible in the `player-stages` collection of your database \(see how to [download your data](faq.md#where-can-i-download-the-data)\).

#### To the player.round

One way of recording the data of players' responses is to set them to the `player.round` prop to identify a particular data/response of a particular player to a particular round. You can do so with this command:

```text
player.round.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer with:

```text
player.round.get("name of property")
```

This will be accessible in the `player-rounds` collection of your database \(see how to [download your data](faq.md#where-can-i-download-the-data)\).

### Where can I download the data?

The data is stored in your database across multiple different collections \(e.g., batches, games, players, etc.\).

The collections you are the most interested in are:

* batches
* games
* player-inputs
* players

Because that is where most of the data is. You can then wrangle these together according to game, player, batch, and other `_ids`.

In the **Admin Panel**, in the **configuration tab**, you can click on **Export** to download a zip file with CSVs or JSONs for each of these collections. The advantage of downloading from the panel is that all the data objects will have been unnested to some extent. For example, every attribute of a Player will be a column in players.csv and every Player will be a row.

You can also directly access your MongoDB Atlas database in R \(with `mongolite`\) or in Python \(with `pymongo`\). You will have to unnest the data objects yourself.

### How can I collect or avoid collecting the players' id, IP address, and user agent?

Empirica allows you to collect the user agents and the IP addresses of players, but this has to be activated to avoid privacy issues.

To do so, in the `"public"` part of the settings.json you need to add:

```text
"collectPII": true
```

When you download files from the control panel, you need to tick a specific box if you want to download the `player id` \(the id they enter themselves or that is set as a URL parameter\), the user agents, and the IP addresses of players \(if you have decided to collect them\): `Add Personal Identity Information (includes the player ID, URL parameters, and IP addresses)`

This allows you to download a version of the data without any identifying information.

### Can I track the status of player? In my data, can I see whether a player dropped out?

In the **Admin Panel**,  in the **monitoring tab**, in the **players tab**, you can see the status of every Player:

* Whether they are offline, idle, or active
* Their Exit Status

In the data you can see the Players'

* exitStepsDone
* exitStatus
* information about whether and when they were retired
* when they last logged in

### How can I know how long players took to submit in a stage?

Once you have [downloaded the data,](faq.md#where-can-i-download-the-data) in the `player-stage` part of the data, you can see each `playerId` and `stageId` combination, when it is was created  \(`createdAt`\) and when the player submitted \(`submittedAt`\). Compare the two and that will give you the time it took the player to submit that stage.

### What happens when the Lobby times-out?

When players start a Game, they go through Intro Steps, and when they have finished the Intro Steps they arrive at a Lobby where they are set as **ready** and they wait for other players to join until there are as many ready players as the number set in the `playerCount` factor.

However, it is possible that there won't be enough players and you don't want players to wait indefinitely in the Lobby for others to join. Hence, you can set a time after which the Lobby times-out.

This is set in the **Admin Panel**, in the **Configuration tab**, in **Lobby Configurations**. What happens when the Lobby times-out depends on these settings.

If `Timeout Type` is set to _Lobby_ then the countdown to the timeout starts as soon as the first player reaches the Lobby. Here the `Timeout Stragety` determines what happens when the Lobby times-out:

* If set to _Fail_, the Game will fail \(it will be terminated\) and the current players waiting in the Lobby [will be sent to the Exit Steps set for them](faq.md#how-can-i-show-a-different-exit-step-to-players-depending-on-whether-they-have-finished-the-game-or-if-the-game-was-cancelled-had-a-problem) with their Exit Status set to _gameLobbyTimedOut_.
* If set to _Ignore_, it will start the Game anyway, even if there aren't enough players.

If `Timeout Type` is set to _Individual_ then a different countdown starts for each player when they reach the Lobby. If the Lobby times-out, the player[ will be sent to the Exit Steps set for them](faq.md#how-can-i-show-a-different-exit-step-to-players-depending-on-whether-they-have-finished-the-game-or-if-the-game-was-cancelled-had-a-problem) with their Exit Status set to _playerLobbyTimedOut_, but the Game will not fail \(i.e., players can still join the game\). With `Extend Count` you can set the number of times the player needs to timeout before they are sent to the Exit Steps. If `Extend Count` is set to 0, then after one timeout the player is sent to the Exit Steps, if it is set to 1, then after two timeouts the player is sent to the Exit Steps.

### How can I customise the Lobby?

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

Then you can either user the default Lobby with chat by importing the  `LobbyChat` component of this package in your experiment's `client/main.js` file, like this:

```text
import { LobbyChat } from "@empirica/chat";
```

And setting it to the lobby like this:

```text
Empirica.lobby(LobbyChat);
```

If you want more control on what the Lobby and the chat look like, you should refer to [how to customise your Lobby](faq.md#how-can-i-customise-the-lobby) and add the chat manually to that following the instructions on [how to add a chat between players in Empirica](faq.md#how-can-i-add-a-chat-between-players).

### How can I get rid of Game and Player data but keep Treatments, Factors, and Lobby Configurations?

In the **Admin Panel**, in the **Monitoring tab**, you can click on **Reset** and then `Reset Games` to clean the data from previous instances/tests. It will remove the current _batches_, _games_, _players_ but it will keep the _treatments_, _factors_, and _lobby configurations_.

### How can I get rid of all the data in my app?

In the **Admin Panel**, in the **Monitoring tab**, you can click on **Reset** and then `Reset Entire App` to clean the whole database. It will remove all of the data in the database.

### Can I have an idea of what my app would look like in production/deployment on my local machine?

When you launch your meteor app locally, it will look different to what it looks like when players see the deployed version. This is because there are tools that you only to have access to as the designer when the app is running locally on your machine \(e.g., `New Player`, `Reset current session`, and `Open Admin` buttons\).

If you want to see what your app will look like once deployed, but still run it locally, you can run the app with:

```text
meteor --production --settings settings.json
```

