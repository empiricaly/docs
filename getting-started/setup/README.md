---
id: quick-start
title: Quick Start
sidebar_label: Quick Start
---

# Setup

1. Install Node.js via [https://nodejs.org/en/](https://nodejs.org/en/) \(download and install the LTS bundle on the left\) 
2. Install Meteor with instructions at [https://www.meteor.com/install](https://www.meteor.com/install)

That's it, you're done!  Proceed to creating your experiment:

{% page-ref page="../quick-start.md" %}

### **A note about Windows 10 Setup**

Empirica is build on industry-standard open-source web technologies which run best in Unix-like operating systems such as Linux and MacOS.   ****

Running Meteor apps on Windows can be unreliable, as apps struggle to install and launch. Whilst stopping and trying again multiple times \(as well as making sure there are no processes that slow down the installation of the Meteor files\) might work, it is not recommended.

In order to run Empirica reliably in Windows, you will need to enable developer settings within advanced Windows settings and download/install the new version of Windows Subsystem for Linux \(WLS2\). 

_If you are a Windows user and get stuck at any point in the development process, please contact joshua.becker@ucl.ac.uk or join the Slack channel for community-based technical support on a wide range of topics._

{% page-ref page="windows-instructions.md" %}

### **A note about macOS Monterey (Apple M1 Chip) Setup**

When using apple's new M1 chip, the minimum Meteor version that you can install is version 2.5.1. This causes an issue with Empirica which uses a specific version of Meteor (1.10.2) as shown below.

```
This project uses Meteor 1.10.2, which isn't available on this platform. To work with this app on all supported platforms, use meteor update --release METEOR@2.7.3 to pin this app to the newest compatible release.
```

**Do not run the recommended command to update meteor as that would also cause issues.**

To resolve this issue, you need to switch from macOS Monterey to macOS Rosetta by running the following command:

```
arch -x86_64 zsh
```

Once entered, run the following command to change the meteor version to 1.10.2.

```
meteor update --release 1.10.2
```
