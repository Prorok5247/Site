# 3 | Making a tweaker
* * *
## What is a Tweaker?
A tweaker allows arguments to be passed into it, and then allow you to inject your mixins into the Minecraft class loader
* * *
## Making the class
So first, go to the mixins folder in your project heirachy
```
project
    -> src
        -> java
            -> group_name
                -> project_name
                    -> mixins <-- Go here
```

In the previous tutorial, in the `build.gradle` file, we set the tweakclass variable to something like so
```java
tweakClass = "tech.lowspeccorgi.mixinclienttutorial.mixins.MixinClientTutorialTweaker"
```

Take the last part of that (the tweaker class name), and in the mixins folder create a class by that name

***

## Writing the Tweaker
First, you need to implement the `ITweaker` interface, just add `implements ITweaker` to your class definition

Next, you should add the launch arguments array, this will store any launch arguments needed for later
```java linenums="1"
/**
    * This stores the Minecraft launch arguments  
    */
private List<String> launchArgs = new ArrayList<>();
```

Now we need to handle the launch arguments, or options, so you can simply just override `acceptOptions`
```java linenums="1"
/**
/**
    * This handles the launch arguments passed towards minecraft
    * @param args The launch arguments
    * @param gameDir The game directory (ie: .minecraft)
    * @param assetsDir The assets directory
    * @param profile The game profile
    */
@Override
public final void acceptOptions(List<String> args, File gameDir, File assetsDir, String profile) {
    ...
}
```

Add the args to our `launchArgs` array, so we can use them later
```java linenums="1"
// Add the launch arguments to our launchArgs array
this.launchArgs.addAll(args);
```

We're going to only deal with 3 options, so we can easily define them as a set of constants
```java linenums="1"
// Constants
final String VERSION = "--version";
final String ASSET_DIR = "--assetDir";
final String GAME_DIR = "--gameDir";
```

Now we need to

1. Check if the launch args contains each constant, if not add it
2. Check if the profile isn't `null` (profile contains metadata about the game, for example, the version)

We can do this by checking with a few `if` statements like so
```java linenums="1"
// Check if version is passed as a launch argument, if not add it
if (!args.contains(VERSION) && profile != null) {
    launchArgs.add(VERSION);
    launchArgs.add(profile);
}

// Check if assetDir is passed as a launch argument, if not add it
if (!args.contains(ASSET_DIR) && profile != null) {
    launchArgs.add(ASSET_DIR);
    launchArgs.add(profile);
}

// Check if gameDir is passed as a launch argument, if not add it
if (!args.contains(GAME_DIR) && profile != null) {
    launchArgs.add(GAME_DIR);
    launchArgs.add(profile);
}
```

You also need to define a function for injection into the class loader
```java linenums="1"
/**
    * Inject into the MC class loader
    * @param classLoader The class loader
    */
@Override
public final void injectIntoClassLoader(LaunchClassLoader classLoader) {
    ...
}
```

For the injection function, you must first initialize `MixinBootsrap`, which is trivially done
```java linenums="1"
MixinBootstrap.init();
```

Next we need to register out mixin configuration file and retrieve the default mixin environment

Just like in the `build.gradle` file, substitute `PROJECT_NAME` with your project's name

```java linenums="1"
// Retrieve the default mixin environment and register the config file
MixinEnvironment environment = MixinEnvironment.getDefaultEnvironment();

environment.addConfiguration("mixins.PROJECT_NAME.json");
```

Now we just need to check if there is no current obfuscation set, then set the obfuscation context to the `notch` obfuscation
```java linenums="1"
// Check if the obfuscation context is null
if (environment.getObfuscationContext() == null) {
    environment.setObfuscationContext("notch");
}
```

Now we need to inform Minecraft that this modification is running on the client side
```java linenums="1"
// This is a client side, client :)
environment.setSide(MixinEnvironment.Side.CLIENT);
```

Lastly, we just add the boiler plate which allows Minecraft to retrieve the launch arguments and the launch target
```java linenums="1"
@Override
public String getLaunchTarget() {
    return MixinBootstrap.getPlatform().getLaunchTarget();
}

@Override
public String[] getLaunchArguments() {
    return launchArgs.toArray(new String[0]);
}
```
* * *
## 🥳 You should now be able to run the client
[View the Tweaker code here](https://gist.github.com/LowSpecCorgi/4e7d2ff1ee7d00324770df8f3bd3a4d5)
* * *

### How can I get help, ask questions, etcetera?
The easiest way is to join my *Discord server*, which you can find here
[https://discord.gg/45RhZT5e9Z](https://discord.gg/45RhZT5e9Z)
