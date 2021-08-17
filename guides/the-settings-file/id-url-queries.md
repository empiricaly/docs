# Setting player ids via URL queries

You can have the player's id set automatically based on a URL query \(e.g., `MID` or `PROLIFIC_PID`\). This will skip the section at the start of the game where players put in their id.

In the `public` section of your settings file, add this sort of code:

```text
"public": { 
    "playerIdParam": "playerIdKey",
    "playerIdParamExclusive": false 
}
```

Where `"playerIdParam"` sets the name of the URL query parameter after which you set the player's id.  For example,  a fake app with these settings, I could set the player id directly in the URL `https://myfakeapp.meteorapp.com/?playerIdKey=3333`.

If `"playerIdParamExclusive"` is set to `true` , then players can only ever participate if their id is set by the `"playerIdParam"` \(i.e., they cannot set their id in the NewPlayer page\), but if it is set to `false`, then players can join both by setting their id at the NewPlayer page \(if there is no query in their URL\) and by having their id in the URL query set by the `"playerIdParam"`.

