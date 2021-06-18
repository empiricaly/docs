---
id: your-first-experiment
title: Your First Experiment
---

# Part 1: Getting Started

## Getting Started With Empirica

Although it won't do too much, Empirica works straight out of the box. The first step to customizing your Empirica app is to launch a test game so you can see your edits in action.

1. Follow the [setup](../../getting-started/setup/) guide and [create your experiment](../../getting-started/quick-start.md)
2. Start your app by running the command `meteor`
3. Then, visit your admin panel [http://localhost:3000/admin](http://localhost:3000/admin)
4. Log in using the password generated in the command line by step 2

### Configuring your app

1. Click `configuration` in the upper right corner to access app settings. This is where you'll define all your experimental conditions.
2. Click `factors` in the menu bar at the top. This is where you set the most

   basic parameters for your experiment. The only default parameter is

   playerCount. Add a new factor by clicking the `+` button. Set the name to

   `one` and the value to `1`, then click `Create Factor Value`.

3. Click `treatments`. This is where you combine factors to create an

   experimental condition. Create a new treatment using the factor you just

   created and name whatever you want.

4. Click `Lobby Configurations` and create a new lobby configuration using the default settings.

### Creating an experiment session

1. Click `monitoring` in the upper right corner to toggle your admin panel from configuration to monitoring. This is where you'll interact with all the

   Empirica features that have to do with actually running experiments.

2. Click `New Batch` in the batches panel, select the treatment you just

   created, and click `Create Batch`.

3. Press the `► Start`  to start the batch.

### Testing the experiment as a participant

1. Navigate to [http://localhost:3000/](http://localhost:3000/) \(or click the `► Open App` button on the top right\) .
2. Follow through the consent, identification \(this would be, e.g., an MTurk/Prolific ID, but you can enter whatever you want right now\), instruction pages, and attention check \(you have to answer correctly\).
3. You're running an experiment! This is the default app, just a slider with 10

   rounds of input. We're going to edit that.

{% hint style="info" %}
For more information about testing the experiment locally, see our [guide on running your experiment](../../getting-started/quick-test.md).
{% endhint %}

## Creating the experiment title

The `client/main.html` is the html point of entry for the React code that determines what a participant sees when they are in your experiment. **You should NOT change anything in this file** **except** for the text in between the `<title></title>` tags. There you can put the title you want to appear at the top of the tab when people are using your experiment \(e.g., "My First Experiment"\).

## Cleaning up the styling

The styling of an Empirica experiment is set in `.client/main.less`. **Less** is very similar to CSS. 

The default styling of the experiment is a bit complicated to understand, and to make our experiment look good we want to tweak a few things. Let's start by replacing the code in `main.less` with this code:

```css
/* ----------------
- default styling -
---------------- */

.title {
  text-align: center;
}

input[type="text"],
input[type="number"] {
  padding: 0.3rem 0.5rem;
  border-radius: 0.3rem;
  border: 1px solid lightgray;
}

input[type="radio"] {
  margin-right: 0.5rem;
}

.content {
  display: flex;
  flex-wrap: wrap;
}

.content > * {
  padding: 0.5rem;
  box-sizing: border-box;
}

.player-profile {
  display: flex;
  flex-direction: column;
  width: 20%;
  align-items: center;

  .profile-avatar {
    width: 8rem;
    height: 8rem;
    margin: 3rem 0;
  }

  .profile-score {
    text-align: center;
    span {
      font-size: 3rem;
      font-weight: bold;
    }
  }

  .timer {
    margin-top: 2rem;
    text-align: center;

    .seconds {
      font-size: 3rem;
      // Helvetica has tabular numbers (fixed width), which looks better
      // for a timer, doesn't jump all over the place.
      font-family: Helvetica, Arial, sans-serif;
      color: green;
    }
    &.lessThan10 {
      .seconds {
        color: orange;
      }
    }
    &.lessThan5 {
      .seconds {
        color: red;
      }
    }
  }
}

.task {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 50%;

  .task-image {
    text-align: center;
    margin-bottom: 1rem;
  }

  .task-question {
    width: 100%;
    text-align: center;
  }

  .task-response {
    width: 100%;

    form {
      display: flex;
      justify-content: center;
      margin-top: 1.5rem;
    }

    .response-submitted {
      margin-top: 1.5rem;
      background-color: #e8e8e8;
      padding: 2rem;
      border-radius: 0.4rem;

      h5 {
        margin: 0 0 0.5rem;
        font-size: inherit;
      }
    }
  }
}

.social-exposure {
  width: 30%;

  .alter {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 1rem;

    img {
      width: 6rem;
      height: 6rem;
      margin-right: 3rem;
    }

    .range {
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}

button {
  padding: 0.8rem 1.2rem;
  border-radius: 0.3rem;
  border: 1px solid lightgray;
  background-color: #eee;
  cursor: pointer;

  &:hover {
    background-color: #ddd;
  }
}

.instructions,
.quiz,
.exit-survey {
  button {
    margin: 2rem 1rem 0 0;
  }

  label {
    display: block;
    margin: 2rem 0 1rem;
  }

  textarea {
    padding: 0.3rem 0.5rem;
    border-radius: 0.3rem;
    border: 1px solid lightgray;
  }
}

.exit-survey {
  .form-line {
    display: flex;

    & > div {
      margin-right: 30px;

      textarea {
        min-height: 90px;
        width: 100%;
      }
    }

    &.thirds {
      & > div {
        flex-grow: 0;
        flex-shrink: 0;
        flex-basis: 30%;

        label {
          min-height: 36px;
        }
      }
    }
  }
}

.finished {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

## Understanding an Empirica Experiment

As you go about your first experiment, it might help to try and get a global understanding of what an Empirica Experiment is.

### The Empirica Framework

The Empirica framework provides powerful tools to create singleplayer and multiplayer games. 

You have the frontend in the `/client` folder. This is where you determine what the users will see when they come to your experiment: which components you build your game with and what they look like. 

The backend in the `/server` folder. This is where you determine the structure of your game.

You have an `admin panel` in the browser that allows you to create factors and treatments as well as monitor the games you are running. For now, everything is running locally for your to test out, but the admin panel will look similar once deployed to recruit participants online.

{% hint style="info" %}
The best ways to learn about the structure of an Empirica experiment and testing it locally is to give the **Readme.md** file a read or consult our other sections in this documentation.
{% endhint %}

### The Technologies used by Empirica

Empirica is a framework that builds on top of a Meteor.js and React.js app. Meteor does the backend and generally building of the app whilst React is used for the front end. 

You can find some tutorial online for both technologies. However, you do not need any knowledge of these technologies for this tutorial.

{% hint style="info" %}
Our [brief explanation on what React components are](../../faq/the-processes-and-elements-of-an-empirica-experiment.md#what-is-a-react-js-component) might help.
{% endhint %}

