---
description: >-
  Once you have created an experiment using either create-empirica-app or a git
  clone.
---

# Running your experiment

## Installing the node modules

Whenever you _first_ pull or clone an app from a repository, you need to run this command to install all the Node packages:

```text
meteor npm install
```

## Running the app locally

Run the app on your local machine with one command:

```text
meteor
```

This can take a few minutes.

This will start the app that you can access as a participant: [https:/localhost:3000/](https:/localhost:3000/)

You can access the `admin panel` here: [https:/localhost:3000/admin](https:/localhost:3000/admin)

Log in with the _username_ and _password_ provided in the command line.

## Loading the factors and treatments

Treatments and factors can also be entered manually via the admin panel.

Empirica also enables "experiment as code" by which experimental treatments and factors can stored in a `.yaml` file \(e.g., `factors.yaml`\). 

To load factors and treatments from a `.yaml` file , go in the **admin panel** and:

* click on the **Configuration** button
* click on **import**
* select the `.yaml` file you want to import the factors and treatments from
* wait a few seconds for the factors and treatments to be imported

## Testing the app

To run a game, create a new `batch` with the games of treatments you want to use and click start on the batch.

Open a player tab by going to [https:/localhost:3000/](https:/localhost:3000/) or clicking on **open app**.

The player that you open with [https:/localhost:3000/](https:/localhost:3000/) is cached on your browser. Whenever you start a game with this player, your local app will keep that information. To play again there are multiple things you can do:

* Click on the **Reset current session** button in the top right to reset this player, and create a new game for this player to join.
* Click on the **New Player** button in the top right to open a new tab with a different player \(you will see tab with an id\).
* Go to the **Players** tab in the admin panel and retire players that have finished or cancelled.

**The app will automatically \("hot"\) reload when save changes to the code.**

