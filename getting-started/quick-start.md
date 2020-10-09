---
id: quick-start
title: Quick Start
sidebar_label: Quick Start
---

# Quick Start

## Install Dependencies

Empirica is build on industry-standard open-source web technologies. We will first need to install Empirica dependencies. Please select your Operating System below for instructions.

{% tabs %}
{% tab title="MacOS / Linux" %}
1. Install Node.js via [https://nodejs.org/en/](https://nodejs.org/en/)  \(download and install the LTS bundle on the left\)
2. Install Meteor with instructions at [https://www.meteor.com/install](https://www.meteor.com/install)
{% endtab %}

{% tab title="Windows" %}
If you are a windows user, you must first complete this guide in order to install the necessary developer tools for Windows.  If you are a MacOS/Linux user, you can proceed to 'Quick Start.'

## Overview

### This guide will proceed through the following steps.

1. **Install and configure the latest version of Windows Linux Subsystem \(WSL2\).** \*\*Step 1 is the most involved component\*\*
2. **Within WLS, install NVM.**

## 1.  Install WSL 2 and Ubuntu 20.04

In order to install WSL2 and Ubuntu 20.04, follow the steps in this guide:  [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

{% hint style="info" %}
If you prefer video instructions, watch this great 5 minute tutorial: [https://www.youtube.com/watch?v=D7Em1wjMiak](https://www.youtube.com/watch?v=D7Em1wjMiak) \(commands in the description\)
{% endhint %}

#### Here is a summary with some helpful screenshots:

1. Enable the Windows Subsystem for Linux
2. Ensure you meet the requirements for WSL2
3. Enable the Virtual Machine feature in Windows \(see screenshot below\)
4. Download the WSL2 update package
5. Set WSL2 as your default Linux system
6. Open Microsoft store, search for Ubuntu 20.04, and install it 
7. Launch the terminal and follow the prompt to create a new username/password \(see screenshot below\).

#### Enabling Virtual Machine feature in Windows: 

![](../.gitbook/assets/1.png)



#### Running Ubuntu 20.04 in Windows for the first time: 

![](../.gitbook/assets/3.png)

## 2.  Install NVM

Our instructions follow this guide: [https://tecadmin.net/install-nodejs-with-nvm/](https://tecadmin.net/install-nodejs-with-nvm/)

_Note: Use the right-click to paste in the command line._

Enter the following commands into your Ubuntu terminal:

```text
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash  # Download and install
source ~/.profile  # Reload system environment
nvm install lts/erbium
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you are not familiar with using the command line, you can find some helpful Linux commands here:

{% page-ref page="../tips-and-tricks/helpful-linux-commands.md" %}
{% endhint %}

## Create your first Experiment

Once your system is configured as described above...

### 1. Run the following 3 command in your terminal. 

Replace `my-experiment` with the name of your experiment \(no spaces\).

```bash
npx create-empirica-app my-experiment
cd my-experiment
meteor
```

### 2. Open [http://localhost:3000/](http://localhost:3000/) to see your experiment.

Edit files in this folder to see instant updates in the browser.

Have fun!

{% hint style="info" %}
If you don't have a code editor of choice, you can find our advice at:

{% page-ref page="../tips-and-tricks/code-editors.md" %}
{% endhint %}

