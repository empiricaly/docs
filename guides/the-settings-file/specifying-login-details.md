# Specifying Login Details

If you go to the settings file, you can elements to set the username and the password of the admin account for your Empirica Admin Panel.

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
meteor --settings <name of settings file>
```

This will allow you to open the admin panel with your own login details instead of the one automatically generated.

