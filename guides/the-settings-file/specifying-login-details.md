# Specifying Login Details

If you go to the `local.json` file \(or you can create a json file of your choosing, just make sure it is in the `.gitignore` file\), you can elements to set the username and the password of the admin account for your Empirica Admin Panel.

For example:

```text
{
  "admins": [
    {
      "username": "admin",
      "password": "p5yycpd2yrg"
    }
  ]
}
```

You can set the information you want here. Now you can run your app with:

```text
meteor --settings local.json
```

This will allow you to open the admin panel with your own login details instead of the one automatically generated.

