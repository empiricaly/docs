# Managing the Data

## Recording the data

There are multiple places where you can record data in an Empirica experiment.

### To the player

One way of recording the data of players' responses is to set them to the `player` prop itself. You can do so with this command:

```text
player.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer for the player with:

```text
player.get("name of property")
```

This will be accessible in the `player` collection of your database.

### To the player.stage

One way of recording the data of players' responses is to set them to the `player.stage` prop to identify a particular data/response of a particular player to a particular stage. You can do so with this command:

```text
player.stage.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer with:

```text
player.stage.get("name of property")
```

This will be accessible in the `player-stages` collection of your database.

### To the player.round

One way of recording the data of players' responses is to set them to the `player.round` prop to identify a particular data/response of a particular player to a particular round. You can do so with this command:

```text
player.round.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer with:

```text
player.round.get("name of property")
```

This will be accessible in the `player-rounds` collection of your database.

### To the round

If you want to save general data \(not specific to one player\), you can save it to the `round` with:

```text
round.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer with:

```text
round.get("name of property")
```

This will be accessible in the `round` collection of your database.

### To the game

If you want to save general data \(not specific to one player\), you can save it to the `game` with:

```text
game.set("name of property", value)
```

You can retrieve what you have set as a specific property/answer with:

```text
game.get("name of property")
```

This will be accessible in the `game` collection of your database.

## Data structure

The data is stored in your database across multiple different collections \(e.g., batches, games, players, etc.\).

The collections you are the most interested in are:

* batches
* games
* player-inputs
* players

Because that is where most of the data is. You can then wrangle these together according to game, player, batch, and other `_ids`.

## Manually downloading data

In the **Admin Panel**, in the **configuration tab**, you can click on **Export** to download a zip file with CSVs or JSONs for each of these collections. The advantage of downloading from the panel is that all the data objects will have been unnested to some extent. For example, every attribute of a Player will be a column in players.csv and every Player will be a row.

## Collect or avoid collecting the players' id, IP address, and user agent

Empirica allows you to collect the user agents and the IP addresses of players, but this has to be activated to avoid running into privacy issues unknowingly.

To do so, in the `"public"` part of the settings file you need to add:

```text
"collectPII": true
```

When you download files from the control panel, you need to tick a specific box if you want to download the `player id` \(the id they enter themselves or that is set as a URL parameter\), the user agents, and the IP addresses of players \(if you have decided to collect them\): `Add Personal Identity Information (includes the player ID, URL parameters, and IP addresses)`

This allows you to download a version of the data without any identifying information.

## How long players took to submit in a stage

Once you have the data, in the `player_stages` part of the data, you can see each `playerId` and `stageId` combination, when it is was started \(`createdAt`\) and when the player submitted \(`submittedAt`\). Compare the two and that will give you the time it took the player to submit that stage.

## How long the game lasted

Once you have the data, in the `game` part of the data, you can see for each game, when it is was created  \(`createdAt`\) and when it finished \(`finishedAt`\). Compare the two and that will give you how long that game lasted.

**Note: this does not count the intro and exit steps, only the game part**.

## Wrangling the data with R

Instead of manually downloading the data, you can directly access the data in your database with the R programing language \(similar things can be done with Python and other languages\).

### Preparation

You will need the `mongolite` library and the `tidyverse` \(or at least `dplyr`\) library.

You will need [this great function to unnest the data](https://raw.githubusercontent.com/joshua-a-becker/RTools/master/df_unnest.R). The top of your script should look like this:

```r
# preparations
library(mongolite)
library(tidyverse)
source("https://raw.githubusercontent.com/joshua-a-becker/RTools/master/df_unnest.R")
```

Then you need to save your [MongoDB URI](deploying-my-experiment/database.md) to a variable \(without the `?retryWrites=true&w=majority`\):

```r
# The URI to the mongoDB database
uri <- "<your uri>"
```

### Getting collections

To get data from collection that is then unnest, use the `mongo` and `find` functions from the `mongolite` packaged, the pipe from `dplyr` and the special unnesting function:

```r
players <- mongo("players", url = uri)$find(
  fields='{}'
) %>% 
  df_unnest()
```

The first argument of the `mongo` function is the string of the collection you want to download. You can do this for players, player-stage, player-rounds, rounds, stages, games, batches, factors, treatments.

The fields argument of the `find` function ca be tricky. If you set it to `'{}'` it means it will grab every field in the database. However, you might do that and find that there is an issue with your dataframes because of a mismatch between the names of dataframe and the actual names available. This is usually because Empirica creates potential a `data` field - the field where all the data that you `.set()` goes to \(see above on recording the data\). So if you haven't set any data to this part of the experiment, and you are getting errors when trying to view the dataframes, set it to `'{"data":false}'` instead.

### Tips for Renaming and Joining

Once you have accessed all the collections you need, you can use merger/join functions to bring everything together into one dataframe.

The games, the players, the rounds, etc. will have certain variables with the same names that you might want to rename: `_id` \(the unique id from the database\), `index`, `createdAt`. 

The id variable of from dataframe might be referred to in another, so you can use this to join dataframes together. For example, `players` has  `_id` which corresponds to `playerId` in `player_stages`.

