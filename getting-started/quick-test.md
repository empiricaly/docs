---
description: How to quickly set up an Empirica experiment locally to test it
---

# How can I quickly test someone else's experiment?

You might want to quickly look at someone's experiment, you might be collaborating with others who might not be involved in the coding of the experiment but still want to test it locally, or you might want to copy/tweak these instructions into a `readme.md` file on your repository for people who come across your experiment.

## General  Setup

If you haven't already:

* Install `Node.js` and `npm` here: [https://nodejs.org/en/](https://nodejs.org/en/)
* Install `Meteor` here: [https://www.meteor.com/install](https://www.meteor.com/install)

## Preparing their app

Clone the repository with their experiment to a local repository on your device.

In the command line run this to install all the Node packages:

```text
meteor npm install
```

If you are re-downloading \(e.g., you pull from the repository\) a version that has had changes in terms of its packages, you need to run this line again.

## Running the app locally

You can now run the app on your local machine with:

```text
meteor
```

This can take a few minutes.

This will start the app that you can access as a participant: [https:/localhost:3000/](https:/localhost:3000/)

You can access the admin panel here: [https:/localhost:3000/admin](https:/localhost:3000/admin)

Log in with the _username_ and _password_ provided in the command line.

## Loading the factors and treatments

To use the app, you usually need to use treatments and factors. Some might be prepared in a `.yaml` file \(e.g., `factors.yaml`\). In the **admin panel**:

* click on the **Configuration** button
* click on **import**
* select the `.yaml` file you want to import the factors and treatments from
* wait a few seconds for the factors and treatments to be imported

## Testing the app

To run a game create a new `batch` with the games of treatments you want to use and click start on the batch.

Open a player tab by going to [https:/localhost:3000/](https:/localhost:3000/) or clicking on **open app**.

The player that you open with [https:/localhost:3000/](https:/localhost:3000/) is cached on your browser. Whenever you start a game with this player, your local app will keep that information. To play again there are multiple things you can do:

* Click on the **Reset current session** button on header of a tab with your player to reset this player, and create a new game for this player to join.
* Click on the **New Player** button on header of a tab with your player to open a new tab with a different player \(you will see tab with an id\).
* Go to the **Players** tab in the admin panel and retire players that have finished or cancelled.

**The app will hot reload as you save changes to the code.**

