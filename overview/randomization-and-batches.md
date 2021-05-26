# Randomization & Batches

## Creating batches

In the `Admin panel` you can create a batch of games. Select the games of the different treatments you want and create the batch. 

To start the games in the batch click the `► Start`  button. 

Now players can join the games in that batch. 

If all the games from that batch are full, they will be allocated to games in other open batches that have the same treatments. If there are no such games available, the players will receive a `gameFull` and told that there are no games available.

## What do the assignment methods 'simple' and 'complete' do?

These are different methods for randomly allocating players to different Games.

When you create a Batch, you can create multiple games within it, and these games can have different treatments. With experiments, one usually wants to randomly allocate players across the different games \(Treatments\).

**Simple** will randomly allocate players to either of the games. Imagine rolling a die for each player to determine which game they are sent to. This might lead to situations where more than the maximum number of players of a game are allocated to a game. In that case, the first players to reach the lobby will be those who get the play the game and the others will be unable to join and their status will be set to `gameFull`.

**Complete** will randomly allocate players to either of the games except those who already have the maximum number of players. This avoids the potential gameFull issue of the simple allocation method.

## What's the difference between N batches of 1 game each or 1 batch of N games?

The assignment of players to games is done in batches sequentially and within each batch players will be randomly assigned to one game. Therefore, when you have 1 batch with multiple games, players will be assigned randomly to each of the X games according to the assignment method. This means it's possible that none of the games fill up even though enough players join for at least one game to proceed past the lobby as the players are distributed across various games. This would not happen if each batch has only one game: the first game will fill up with the first players who move past the lobby and the remaining players will transition to the game in the second batch.

If you want to ensure the maximum possible number of players get assigned to a game, a good strategy would be to start batches each with 1 game per each treatment condition. For example, if you have two treatment conditions of 8 players each, your batches should contain 1 game of each treatment. This way you can be sure if 16 players join, all 16 will be randomized between only 2 games and you don't lose any of your players in games that never fill up. **This approach however has a drawback as it does not randomize between players with different arrival time or completion time of instructions.**

