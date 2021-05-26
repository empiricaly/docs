# Experiment Set Up

### I downloaded an Empirica app from GitHub, how do I make sure it has everything I need to use it?

If you download an app from a GitHub, such as the **community demos**, it is likely that there will be some missing parts:

* Not all of the NPM, Meteor, and React.js [packages will be installed](faq.md#how-can-i-make-sure-all-the-packages-in-my-app-are-installed).
* There will be no [settings file](../guides/the-settings-file.md), you might need to create your own and [connect it to your own database](faq.md#how-do-i-connect-my-app-to-mongodb-atlas).
* You might need to [import the treatments and factors](faq.md#can-i-import-and-export-my-treatments-factors-and-lobby-settings).



### How can I make sure all the packages in my app are installed?

When an Empirica app is sent to and downloaded from a remote repository like GitHub, it will not have all the packages installed. Instead, the `package.json` and `package-lock.json` files keep track of all the packages the app needs.

You will get error messages if you try to run the app without all the packages it needs installed.

To have it install all the packages it needs, use:

```text
meteor npm install
```

### 

### Can I import and export my treatments, factors, and lobby settings?

In the **configuration** tab of the Admin Panel of your app there are **Import** and **Export** buttons. 

The **Export** button will produce a `.yaml` file of your treatments and factors.

The **Import** button will allow you to select a `.yaml` file to set the treatments and factors.

If you want to share your app with someone, it is important to provide this `.yaml` so that they can use the same treatments and factors as you.

