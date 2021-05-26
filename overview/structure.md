# Structure

## General Structure

Empirica allows you to build powerful apps for multiplayer and singleplayer research. It is made of these important components:

* Your **code**.
  * The backend \(managing your games and data\) with Meteor
  * The frontend \(what the players see and interact with\) with React.js
* An **Admin Panel** that allows you manage the conditions, games, and players in your experiment.
* A connection to a **database**, such as MongoDB Atlas, to store your data that can be accessed and modified live by your experiment.
* The app is deployed to a **hosting service**, such as Meteor Galaxy, so that your experiment is online and you can send a **link** for players to connect to it.

![](../.gitbook/assets/picture1%20%281%29.png)

## File Structure

This is what a starting Empirica experiment file structure looks like:

```text
my-experiment
├── .meteor
├── README.md
├── node_modules
├── package.json
├── package-lock.json
├── .gitignore
├── public
├── client
│   ├── main.html
│   ├── main.js
│   ├── main.css
│   ├── game
│   │   └── ...
│   ├── intro
│   │   └── ...
│   └── exit
│       └── ...
└── server
    ├── main.js
    ├── callbacks.js
    └── bots.js
```

**It contains:**

* `.meteor` with the meteor files. You do not need to touch those.
* `node_modules` the files for the node modules in your app. You do not need to touch those. This directory should be in the `.gitignore` file.
* `public` where you store static assets \(e.g., images\).
* `client` where you set what the players can see in the experiment.
* `server` where you manage the flow and resources of the experiment.

### Client

All code in the `/client` directory will be ran on the client. The entry point for your app on the client can be found in `/client/main.js`. In there you will find more details about how to customize how a game _Round_ should be rendered, what _Consent_ message and which _Intro Steps_ you want to present the players with, etc.

The HTML root of you app in `/client/main.html` shouldn't generally be changed much, other than to update the app's HTML `<head>`, which contains the app's title, and possibly 3rd party JS and CSS imports.

All styling starts in `/client/main.less`, and is written in [LESS](http://lesscss.org/), a simple superset of CSS. You can also add a plain CSS files in `/client`.

The `/client/game`, `/client/intro`, `/client/exit` directories all contain [React](https://reactjs.org/) components, which compose the UI of your app. If you are new to React, we recommend you try out the official [React Tutorial](https://reactjs.org/tutorial/tutorial.html).

### Server

Server-side code all starts in the `/server/main.js` file. In that file, we set an important Empirica integration point, the `Empirica.gameInit`, which allows to configure each game as they are initiated by Empirica.

From there we import 2 other files. First the `/server/callback.js` file, which contains all the possible callbacks used in the lifecycle of a game. These callbacks, such as `onRoundEnd`, offer powerful ways to add logic to a game in a central point \(the server\), which is often preferable to adding all the logic on the client.

 Finally, the `/server/bots.js` file is where you can add bot definitions to your app.

### Public

The `/public` is here to host any static assets you might need in the game, such as images. For example, if you add an image at `/public/my-logo.jpeg`, it will be available in the app at `http://localhost:3000/my-logo.jpeg`.

