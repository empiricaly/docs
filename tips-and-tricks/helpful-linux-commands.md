---
description: >-
  Here is a list of helpful commands that should enhance your experience with
  the command line in Linux and WSL.
---

# Helpful Linux Commands

## Commands

To find in which directory you currently are, use:

```text
pwd
```

To list all the files and folders in your directory, use:

```text
ls
```

To navigate between directories use _cd_ followed by the path or name of the directory you want to access. If you start typing a location that is present in your current directory, you can then use the TAB key to autocomplete the name of a directory.

```text
cd <directory>
```

To go up one directory, use:

```text
cd ..
```

To go back to the root \(_/home/&lt;user&gt;_\), use:

```text
cd ~
```

To delete a file, use:

```text
rm <filename>
```

To delete a directory, use:

```text
rm -r <directoryname>
```

Now that you have run multiple commands, and that you will be launching Empirica apps, there are a few useful tricks you should know:

* Use **ctrl+l** to clear your command line.
* Use **ctrl+c** to cancel \(whilst writing it\) or interrupt \(when it is running\) a command.

## Tips for Windows WSL users

### Viewing, accessing, and modifying your files

If you correctly installed WSL 2, you can view, access, and modify the files and folders in your WSL directly from your explorer. To open up the WSL in your explorer use:

```text
explorer.exe .
```

Nevertheless, you will also need to get accustomed to using the command line to navigate in, and use, the the files and folders in your WSL directory. Here are some useful commands.

{% hint style="info" %}
Head over to our Code Editors advice page to find out how to use our recommended code editor with Windows WSL:

{% page-ref page="code-editors.md" %}
{% endhint %}

