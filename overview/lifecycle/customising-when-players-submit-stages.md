# Customising when players submit stages

## When players submit \(end\) a stage

Instead of having players wait until the end of the stage, you can have them submit their answer. This will set the Stage as submitted for them. If all the other players have also submitted the Stage, then the players move on to the next Stage or Round.

To set the stage to submitted you need to run this method from the `player` prop:

```text
player.stage.submit()
```

This will change this property, that you can use to see if the stage has been submitted \(it is a Boolean, true or false\):

```text
player.stage.submitted
```

For example, you could create a button, that has a handle method for `onClick` that will do the necessary with the player's answer and call `player.stage.submit()`.

## What players see when they have submitted a stage

Once a player submits for the stage, you might want to show something different on the screen. For example, instead of showing the stage's question/task \(and avoiding players submitting multiple responses\), you could show a message thanking the player for submitting the stage and telling them that they will have to wait for all players to submit the stage before they can move on to the next one.

You can do this by setting a **conditional** that renders different elements depending on what `player.stage.submitted` returns.

For example:

```jsx
{
player.stage.submitted
    ? <div> Thank you for your answer. The next stage will start when all the other 
    players have submitted their answer. </div>
    : <div><Question player={player} /></div>
}
```

