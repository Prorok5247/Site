# 2 | Setting up
* * *

## Creating a new project
Click the create new project button on IDEA

![New project](../../res/ST_NEW_PROJ.png)

Go through the dialog setting options as you please, until you can create your project
* * *
## Customizing the template

### Build.gradle
So now we need to convert this from a template to your own project

First open `build.gradle`, and [paste the code from here](https://github.com/SkidKit/MixinTemplate/blob/master/build.gradle) to your `build.gradle`

I want you to decide on the following things

- A project name
    - The name of your client
- A group name
    - This is a top level layout of your package structure, please [read here](http://maven.apache.org/guides/mini/guide-naming-conventions.html) if you do not understand what it is

Now you must do the following things

1. Go to line 25 and change the word `GROUP_NAME` to whatever you have decided your group name to be
2. Go to line 26 and change the word `BASE_NAME` to whatever you have decided for your project name to be
3. Go to line 33 and change the word `GROUP_NAME` to your group name, and the word `PROJECT_NAME` to your project name + the word `Tweaker` (Ie: MixinClientTutorialTweaker)
4. Go to line 67 and change the word `PROJECT_NAME` to your project's name
5. Go to line 91 and change the word `PROJECT_NAME` to your project'sname
7. Lastly, go to line 92 and change the word `GROUP_NAME` to your decided group name, and change `TWEAKER_NAME` to your project's name + the word `Tweaker`

[View the an example of a customised `build.gradle` here](https://gist.github.com/LowSpecCorgi/68dbaa069c95494c4687905e75f3e882)

* * *
## Little gradle fix
Open the file `gradle-wrapper.properties`
```
project
    -> gradle
        -> wrapper
            -> gradle-wrapper.properties <-- Open this file
```

It should look roughly like this
```java linenums="1"
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-6.8-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

Where the line distribution is, change the `distributionUrl` so the version is `4.7`, like so:
```java linenums="1"
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.7-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

This will take a bit of time, so just wait for it to download and install gradle
* * *

## Mixin file
Create the file `mixins.PROJECT_NAME.json`, with the word `PROJECT_NAME` replaced with the project name that you decided on earlier, in the folder src/main/resources
```
project
    -> src
        -> main
            -> resources
                -> mixins.PROJECT_NAME.json <-- Create this file
```

In that file, add the content:
!!! note
    Replace all instances of `GROUP_NAME` with our group name
    And all instances of `PROJECT_NAME` should be replaced with your project name
```json linenums="1"
{
  "required": true,
  "compatibilityLevel": "JAVA_8",
  "verbose": true,
  "package": "GROUP_NAME.PROJECT_NAME.mixins",
  "refmap": "mixins.PROJECT_NAME.refmap.json",
  "mixins": []
}
```

For example, if my group ID was `tech.lowspeccorgi`, and my project name was MixinClientTutorial, I would customise the `*.json` refmap file like so

```json linenums="1"
{
  "required": true,
  "compatibilityLevel": "JAVA_8",
  "verbose": true,
  "package": "tech.lowspeccorgi.mixinclienttutorial.mixins",
  "refmap": "mixins.MixinClientTutorial.refmap.json",
  "mixins": []
}
```

I will explain this file *later*, but for now we don't need to know much about it

* * *
## Finishing off
### Creating the skeleton
Remember the group ID seperated by dots you had?

In the `java` folder outlined below, create a folder structure seperated by dots, with your project name, and a mixins folder
```
project
    -> src
        -> java <-- Go here
```

For example, if my group ID was `tech.lowspeccorgi`, and my project name was `MixinClientTutorial`, I would create a folder structure like so
```
project
    -> src
        -> java
            -> tech
                -> lowspeccorgi
                    -> mixinclienttutorial
                        -> mixins
```
* * *
### Configuring the project

Now you can run the task `:setupDecompWorkspace`

![Run decomp task](../../res/ST_DECOM_TASK.png)

**This will take some time**

Next click the elephant reload button to get gradle to configure your project

![Configure project](../../res/BUILD_GRADLE.png)

* * *
### Generating run configurations
In the same sidepanel where you pressed `setupDecompWorkspace`, you should see a button entitled `genIntelijRuns`, click that to run the `:genIntelijRuns` task

After that is done, click the edit configurations button
![Edit configurations button](../../res/ST_EDIT_CONF.png)

Then change the module to `PROJECT_NAME.main`, and click apply
![Configuration screen](../../res/ST_CONF_SCREEN.png)
* * *
You can now run the project by clicking the green run button
!!! error
    This won't work for now as we don't have a Tweaker, but I will get to that next tutorial
![Run button](../../res/ST_RUN.png)


* * *

### How can I get help, ask questions, etcetera?
The easiest way is to join my *Discord server*, which you can find here
[https://discord.gg/45RhZT5e9Z](https://discord.gg/45RhZT5e9Z)
