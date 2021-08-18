# Connecting Locally to MongoDB

You can run your Empirica experiment locally, whilst still connecting it to a cloud MongoDB database.

To do so, you need to [create the database and obtain the URI](../deploying-my-experiment/database.md).

## Via the settings file

As explained in the [guide for deploying to MongoDB](../deploying-my-experiment/database.md#how-do-i-connect-my-app-to-mongodb-atlas), you will need to add these elements to your settings file:

```text
    "galaxy.meteor.com": {
        "env": {
            "MONGO_URL": "mongodb+srv://<read&write username>:<read&write password>@<connection>/<database name>?retryWrites=true&w=majority",
            "MONGO_OPLOG_URL": "mongodb+srv://<oplog username>:<oplog password>@<connection>/local"
        }
    },
```

Fill in the &lt;&gt; parts with the appropriate elements.

Now you can run your app with:

```text
meteor --settings <name of settings file>
```

{% hint style="info" %}
This might not always work for connecting locally to your MongoDB. The method below is more reliable.
{% endhint %}

## Via the command line

According to your OS, follow the instructions below to connect to your MongoDB via the command line.

{% tabs %}
{% tab title="Mac, Linux, Windows with WSL" %}
Run this command with the URI you have obtained but without the `?retryWrites=true&w=majority`

```text
MONGO_URL=<MongoDB URI> meteor
```
{% endtab %}

{% tab title="Windows without WSL" %}
Create a `.bat` file \(e.g., `start.bat`\). **These files allow you to run multiple commands on Windows. Do not run commands you are not comfortable with or not convinced they are secure.**

Make sure that you have `*.bat` in your `.gitignore` because this file will contain sensitive information \(your access to the database\) and you don't want it to be sent to your repository.  
  
Add the following commands to the file with the URI you have obtained but without the `?retryWrites=true&w=majority`:

```text
SET MONGO_URL=<MongoDB URI>
meteor
```

Run the file in the command line \(e.g., `.\start.bat` \).
{% endtab %}
{% endtabs %}

