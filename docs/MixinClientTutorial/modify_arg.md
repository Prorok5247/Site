# 5 | Modifying arguments
* * *

## What is ModifyArg?
Consider the following method
```java linenums="1"
private void aMethod() {
    System.out.println("No. 4561!");
    System.out.println("HElooo 25!");
    System.out.println("idk 356!");
    System.out.println("interesting 46!");
    System.out.println("Hi! 5!");
    System.out.println("Hello 1!");
    System.out.println("no? 456!");
    System.out.println("Bye 48!");
    System.out.println("ALLRIGHt 33!");
    System.out.println("lol 10!");
}
```

As you can clearly observe, it consists of a set print statements, which outputs 10 strings with no noticeable pattern

So, what if I wanted to change the text `"Hi! 5!"` to `"Hello world!"`?

With our current knowledge all we could do is `@Overwrite` it, but this becomes very painful with larger method bodies with functions/variables that need `@Shadow`'ing (I will talk about shadowing later)

So what do we do?

May I introduce you to `@ModifyArg`, which is a neat function, that allows us to modify the arguments of a function in a function

In the example provided, we can use `@ModifyArg` like so

```java linenums="1"
@ModifyArg(method = "aMethod", at = @At(
            value = "INVOKE",
            target = "Ljava/io/PrintStream;println(Ljava/lang/String;)V",
            ordinal = 4),
           index = 0)
private String modifyOutput(String x) {
    return "Hello World!";
}
```

This (probably) looks exasperatingly complex, so I shall explain what the hell is going on in the aforementioned example

The first parameter is `method`, this is the method that we are going to modify

Now for the hunk of an `@At` statement

- `VALUE = "INVOKE"`
    - This signifies that we are going to look for the invocation of a method
- `target = "Ljava/io/PrintStream;println(Ljava/lang/String;)V"` 
    - This tells `Mixin` where and what method to modify

Lets break down the syntax of `"Ljava/io/PrintStream;println(Ljava/lang/String;)V"` using the table below

The `L` represents a class name, so `Ljava/io/PrintStream;` is the path to the class `PrintStream, which is where `println` is located 

The `println` specifies the method name we are looking for, which is `println`

Inside the brackets, we see another class `(Ljava/lang/String;)`, this is the path to the `String` class, as `String` is actually a class, just one in the standard library, which doesn't need to be imported, everything inside these brackets are the parameters for `println`

Just after the brackets there is a `V`, which simply means `void` specifying that the `println` function doesn't return any value

|*FieldType* term|	Type|	Interpretation|
|--|--|--|
|B|	byte    |	signed byte|
|C|	char    |	Unicode character code point in the Basic Multilingual Plane, encoded with UTF-16|
|D|	double	|double-precision floating-point value|
|F|	float	|single-precision floating-point value|
|I|	int	|integer|
|J|	long	|long integer|
|L| ClassName; |	reference	an instance of class ClassName|
|S|	short	|signed short|
|Z|	boolean|	true or false|
|[|	reference|	one array dimension|

[Read more](https://docs.oracle.com/javase/specs/jvms/se14/html/jvms-4.html#jvms-4.3.2)

There are only 2 more parameters to go over: `ordinal` and `index`

- `ordinal = 4` (optional) 
    - This allows you to set an offset of which `println` statement you actually want to modify, indexed by zero, in this case, it was set to 4, meaning that the 4th zero indexed println statement's arguments will be modified

- `index = 0`
    - This  is the last parameter that we have set, which isn't inside the `@At` statement, this allows us to select which argument in the function we want to modify (indexed by zero)

Lastly, the return statement, `return "Hello World!"`, just returns what we want that argument to be

* * *

## More context
What happens if the argument we want to modify depends on all of the other arguments being passed in?

Say, instead of `println` accepting 1 argument (`String`), rather, it accepted 2 arguments, your message, and the number of times you want it to be printed

```java linenums="1"
private void aMethod() {
    System.out.println("No. 4561!", 5);
    System.out.println("HElooo 25!", 3);
    System.out.println("idk 356!", 45);
    System.out.println("interesting 46!", 34);
    System.out.println("Hi! 5!", 34);
    System.out.println("Hello 1!", 42);
    System.out.println("no? 456!", 4);
    System.out.println("Bye 48!", 1);
    System.out.println("ALLRIGHt 33!", 99);
    System.out.println("lol 10!", 27);
}
```

In this example, once again, we want to change the text of `"Hi! 5!"` to `"Hello world!"`, but this time we also want to add the number of times it will repeat to it (ie: `"Hello World! (34)"`)

It's as simple as adding the extra arguments to the function, like shown below

```java linenums="1"
@ModifyArg(method = "aMethod", at = @At(
            value = "INVOKE",
            target = "Ljava/io/PrintStream;println(Ljava/lang/String;I)V",
            ordinal = 4),
           index = 0)
private String modifyOutput(String x, int nTimesToRepeat) {
    return "Hello World! (" + nTimesToRepeat + ")";
}
```

## What is ModifyArgs?
!!! warning
    My explanation for this may be lacking, as there isn't much official documentation on `@ModifyArgs`, so I am basing this on the [FabricMC examples](https://fabricmc.net/wiki/tutorial:mixin_examples)

Is this a repeat of the previous section?

- Nope! `@ModifyArgs` is different to `@ModifyArg`

Okay, so I know how to modify one argument, what about multiple?

Well using the former example where `println` has 2 arguments, a message and a number of times to print that message, we can modify multiple arguments like so

```java linenums="1"
@ModifyArg(method = "aMethod", at = @At(
            value = "INVOKE",
            target = "Ljava/io/PrintStream;println(Ljava/lang/String;I)V",
            ordinal = 4),
           index = 0)
private void modifyOutput(Args args) {
    String x = args.get(0);
    int nTimesToRepeat = args.get(1);

    args.set(0, "Hello world! (" + nTimesToRepeat + ")");
    args.set(1, nTimesToRepeat + 20);
}
```

Args? What's that?

`Args` is a class provided by `Mixin` that provides 2 major methods

- `args.get(int index)`
    - This allows you to retrieve one of the function's arguments be index

- `args.set(int index, T value)`
    - This allows you to set an arguments value by index

So, now that we know a bit more about class, we can understand what this does
```java linenums="1"
String x = args.get(0); // Get the first argument, the message
int nTimesToRepeat = args.get(1); // Get the second argument, the number of time to repeat the message

args.set(0, "Hello world! (" + nTimesToRepeat + ")"); // Set the first argument to the string "Hello world! (n)", with n being the number of times to repeat
args.set(1, nTimesToRepeat + 20); // This takes the number of times to repeat, and adds the value 20 to it
```
* * *

## A practical example
!!! note
    The [MinecraftDev plugin](https://minecraftdev.org/) can autocomplete the method descriptors for you

In the method `startGame`, there is a line where the version of the LWJGL library is outputted to the console, this is exactly what we want to modify
```java  linenums="1"
/**
    * Starts the game: initializes the canvas, the title, the settings, etcetera.
    */
private void startGame() throws LWJGLException, IOException {
    this.gameSettings = new GameSettings(this, this.mcDataDir);
    this.defaultResourcePacks.add(this.mcDefaultResourcePack);
    this.startTimerHackThread();

    if (this.gameSettings.overrideHeight > 0 && this.gameSettings.overrideWidth > 0) {
        this.displayWidth = this.gameSettings.overrideWidth;
        this.displayHeight = this.gameSettings.overrideHeight;
    }

    logger.info("LWJGL Version: " + Sys.getVersion()); // THIS is what we want to modify
    ...
}
```

First, we need to get the descriptor of the `info` function, so to do this we

- Open the info function file, usually done by pressing ++ctrl+lbutton++, with your cursor above the usage, in most IDEs
- Look at the package name, in this case it's `org.apache.logging.log4j`, so we convert that into the descriptor format by
- Making it separated by forward slashes = `org/apache/logging/log4j`
- Adding the class name = `org/apache/logging/log4j/Logger`
- Adding an `L` to signify class = `Lorg/apache/logging/log4j/Logger`
- Adding a semi-colon = `Lorg/apache/logging/log4j/Logger;`

Now we can add the method name

- `Lorg/apache/logging/log4j/Logger;info`

Then the only parameter, which is a string

- `Lorg/apache/logging/log4j/Logger;info(Ljava/lang/String;)`

Lastly adding the return value, which is void 

- `Lorg/apache/logging/log4j/Logger;info(Ljava/lang/String;)V`

We're modifying the arguments of the invocation of a function, so value can be set to `INVOKE`

The target should be set to the descriptor, just like we specified

The ordinal should be set to 0 as we are looking for the first `Logger.info` statement

The index should be 0 as the `Logger.info` function body only has one argument, which is the argument we want to modify

The return value should be what we want to set the argument to

```java linenums="1"
@ModifyArg(method = "startGame", at = @At(
        value = "INVOKE", target = "Lorg/apache/logging/log4j/Logger;info(Ljava/lang/String;)V", ordinal = 0), index = 0)
private String info(String message) {
    return "LWJGL stuff has been overridden lol";
}
```
* * *
## Result
![Result of modify args](../../res/CBK_LWJGL_OVERRIDDEN.png)
* * *

### How can I get help, ask questions, etcetera?
The easiest way is to join my *Discord server*, which you can find here
[https://discord.gg/45RhZT5e9Z](https://discord.gg/45RhZT5e9Z)
