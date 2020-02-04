---
id: guess-the-correlation
title: Guess the Correlation
---

# Guess The Correlation

You and a group of friends can play with this experiment as we ran it by following these instructions \(assuming you have [Meteor installed](https://www.meteor.com/install)\):

1. [Download](https://github.com/amaatouq/guess-the-correlation) the repository \(and unzip\). Alternatively, from terminal just run:

```text
git clone https://github.com/amaatouq/guess-the-correlation.git
```

1. Go into the folder with `cd guess-the-correlation`
2. Install the required dependencies by running `meteor npm install`
3. Edit the `admin` password in the settings file `local.json` to something you like.
4. Run the local instance with `meteor --settings local.json`
5. Go to [http://localhost:3000/admin](http://localhost:3000/admin) \(or whatever port you are running Meteor on\).
6. login with the credentials username: `admin` and the password you have in `local.json`
7. Start a new batch with whatever configuration you want \(see the example configuration\).

## Example Config:

First, you have to enter the Configuration mode instead of the Monitoring model in the admin UI.

![](https://github.com/amaatouq/guess-the-correlation/raw/master/readme_screenshots/configuration_mode.png)

This will allow you to configure the experiment: Factors, Lobby, and Treatments:

![](https://github.com/amaatouq/guess-the-correlation/raw/master/readme_screenshots/configuration_mode_inside.png)

Now, you have the option to create your own configuration \(see below\) or load an example configuration by clicking on `import` and then choosing the file `./example-config.yaml`. Loading the example configurations will choose some example values for the factors \(i.e., independent variables\), lobby configuration, and few treatments.

The example factors will look like this: ![](https://github.com/amaatouq/guess-the-correlation/raw/master/readme_screenshots/factors_example.png)

And the example treatments will look like this: ![](https://github.com/amaatouq/guess-the-correlation/raw/master/readme_screenshots/treatments_example.png)

Finally, you can go back to the Monitoring mode:

![](https://github.com/amaatouq/guess-the-correlation/raw/master/readme_screenshots/monitoring_mode.png)

Now the _**Batchs**_ tab make sure you add a new batch, add the treatments you want, choose your lobby configurations, and then _**start**_ the batch.

![](https://github.com/amaatouq/guess-the-correlation/raw/master/readme_screenshots/new_batch.png)

Go to [http://localhost:3000/](http://localhost:3000/) and enjoy! If you don't have 3 friends to play with you, you always can use the `new player` button in development \(for more details see this\), which can add an arbitrary number players to the experiment while staying in the same browser \(i.e., no need to open different browsers\).

![](https://github.com/amaatouq/guess-the-correlation/raw/master/readme_screenshots/game.png)

