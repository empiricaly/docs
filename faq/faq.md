# Managing Players and Games

### When are players playing synchronously and asynchronously? Can I modify this?

Players are **asynchronous during the Intro Steps** \(i.e., they can finish each part at their own pace\). At the **lobby**, players have to wait for each other to finish the Intro Steps. Then players are **synchronous during the Game** \(i.e., they must all finish a Stage before they can move on to the next Stage or Round\). Finally, once all the players have finished the Game, they are **asynchronous again during the Exit Steps**.

It is not recommended to try and modify this aspect of an Empirica app. You should consider what you want players to be doing at their own pace and what you want them to be doing synchronously. Remember that you can use your Treatments to modify how certain aspects of the game are presented throughout the app, and you can set different times for different Stages.

For more information about the life cycle of a game, see [here](../overview/lifecycle/).

### Can players leave and re-join the app? Can players refresh the page?

Yes, the browser cache of the player records they Player id and the database records the status and position of the Player, so it knows whether the Player is in the Intro Steps, the Lobby, a specific Round and Stage, or the Exit Steps. If a player refreshes the page or leaves and comes back, they will be sent back to their position. 

However, remember that in Stages the timer is still ticking and during the Intro Steps other players might be waiting in the lobby; hence, it is not a good idea for players to leave the game.

It can be useful for players to know that they can safely refresh the page in case they have a graphical bug or if the page takes too long to render.

### How can I assign players to different versions of my game based on a condition set outside of the game?

Imagine you have players that have different favourite colors, and you want to create games with one player for each favourite color. This is currently hard to make with Empirica because the players are allocated to a game and then they are asked questions, not the other way round.

One solution could be to create a big game, and ask players their favourite color in the Intro Steps. Then, during the [gameInit\(\)](../overview/lifecycle/) you assign participants to subgames. This might be complicated because you might have the wrong proportions of players per color. 

Another solution is to create the groups outside of Empirica and invite them one group at a time to a game.

### Can I have an idea of what my app would look like in production/deployment on my local machine?

When you launch your meteor app locally, it will look different to what it looks like when players see the deployed version. This is because there are tools that you only to have access to as the designer when the app is running locally on your machine \(e.g., `New Player`, `Reset current session`, and `Open Admin` buttons\).

If you want to see what your app will look like once deployed, but still run it locally, you can run the app with:

```text
meteor --production --settings settings.json
```

