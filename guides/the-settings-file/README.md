# The Settings File

Emprica is configured with **settings** stored as `json` data.

You will need to create a **settings file** to set up your app. This will be a `.json` file, which is often named`settings.json` \(or `local.json` or `deploy.json`\).

**The settings file contains information that you do not want to share with the public**, such as your admin password and database security credentials. Hence you should keep it private. If you put your code on an online version-control repository \(e.g., GitHub\), we suggest you _**add the name of your settings file to the `.gitignore` file of your project.**_

The settings file contains a JSON object `{}` in which there are other objects separated by `,`

```javascript
{
    "galaxy.meteor.com": {
        
    },
    "admins": [
        {
            "username": "myusername",
            "password": "mypassword"
        }
    ],
    "public": {  
    }
}
```

To run your app locally with your settings as set in the settings file, use:

```text
meteor --settings settings.json
```

## Uses

The settings file is useful for:

{% page-ref page="specifying-login-details.md" %}

{% page-ref page="id-url-queries.md" %}

{% page-ref page="connecting-locally-to-mongodb.md" %}

