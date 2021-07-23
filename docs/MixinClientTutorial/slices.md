# 6 | Slices
* * *

## What are slices?

Consider the following block of code
```java linenums="1"
private void aMethod(RandomClass test) {
    System.out.println("No. 4561!");
    System.out.println("HElooo 25!");
    System.out.println("idk 356!");
    System.out.println("interesting 46!");
    if (test.getWhetherToRunThisLol()) {
        System.out.println("Hi! 5!");
        System.out.println("Hello 1!");
        System.out.println("no? 456!");
    }
    System.out.println("Bye 48!");
    System.out.println("ALLRIGHt 33!");
    System.out.println("lol 10!");
}
```

This outputs many statements, with no observable pattern to the text, and a function that returns `boolean` by the name of `getWhetherToRunThisLol()Z` decides whether to run the three `println` statements contained within the if statement

We want to inject just after the `System.out.println("Hi! 5!")` in the `if (test)` block

How could we do this?

For starters `@Inject` can be used, but we also need to inject just after the declaration of message

Why not *purely* use ordinal?

- It's hard to read
    - You have to individually count each method to find the right one, which doesn't help easily identify where you are injecting into

- It's hard to do
    - Especially with large function bodies consisting with many calls to the function you are looking at, counting all of that is just unnecessary effort

- It doesn't scale well
    - Say the code block you are injecting into is updated, and more calls to that method are added, you now have to update the ordinal value accordingly

So... what can I use instead?
Allow me to introduce you to `@Slice`, this subverts most of the problems with ordinal, so I shall jump into an example straight away

!!! note
    There is also `to`, this will specify the end of the scope for the slice

```java linenums="1"
@Inject(
    slice = @Slice(
        from = @At(value = "INVOKE", target="getWhetherToRunThisLol()Z")
    )
    at = @At(value = "INVOKE", target = "Ljava/io/PrintStream;println(Ljava/lang/String;I)V")
)
private void aMethodInjector(RandomClass test) {
    System.out.println("HELLO!");
}
```

- `from` (`@At`)
    - `from` accepts an `At` value, which specifies where you want to start the 'scope' from
    - Scope? That is the scope of code in which `Mixin` will look for the top level `at` statement

So, with this scope, mixin will only look at this code

```java linenums="1"
if (test.getWhetherToRunThisLol()) {
    System.out.println("Hi! 5!");
    System.out.println("Hello 1!");
    System.out.println("no? 456!");
}

System.out.println("Bye 48!");
System.out.println("ALLRIGHt 33!");
System.out.println("lol 10!");
```

Subsequently, it will look for the first `println` statement, which, in this case happens to be `System.out.println("Hi! 5!")`, and inject a print statement just after that

## A practical example
So, In the `Minecraft` class, there is a method called `drawSplashScreen`

It has a hunk of OpenGL code

```java linenums="1"
GlStateManager.matrixMode(5889);
GlStateManager.loadIdentity();
GlStateManager.ortho(0.0D, (double)scaledresolution.getScaledWidth(), (double)scaledresolution.getScaledHeight(), 0.0D, 1000.0D, 3000.0D);
GlStateManager.matrixMode(5888);
GlStateManager.loadIdentity(); // <-- HERE
GlStateManager.translate(0.0F, 0.0F, -2000.0F);
GlStateManager.disableLighting();
GlStateManager.disableFog();
GlStateManager.disableDepth();
GlStateManager.enableTexture2D();
```

And we want to inject right after the `loadIdentity` method call, denoted by `// <-- HERE`

We can do this by making a `@Slice` which targets the `ortho` method creating a scope from the ortho method, all the way down, then we can use a top level `@At` annotation which will select the first load identity in that scope, which looks like so
```java linenums="1"
GlStateManager.ortho(0.0D, (double)scaledresolution.getScaledWidth(), (double)scaledresolution.getScaledHeight(), 0.0D, 1000.0D, 3000.0D); // Scope start
GlStateManager.matrixMode(5888);
GlStateManager.loadIdentity(); // <-- HERE
// Injecting right here
GlStateManager.translate(0.0F, 0.0F, -2000.0F);
GlStateManager.disableLighting();
GlStateManager.disableFog();
GlStateManager.disableDepth();
GlStateManager.enableTexture2D();
```

All of this put together looks like so

```java linenums="1"
@Inject(method = "drawSplashScreen",
        slice = @Slice(
                from = @At(value = "INVOKE", target="Lnet/minecraft/client/renderer/GlStateManager;ortho(DDDDDD)V")
        ),
        at = @At(value = "INVOKE", target = "Lnet/minecraft/client/renderer/GlStateManager;loadIdentity()V", ordinal = 0))
private void splashScreen(TextureManager textureManagerInstance, CallbackInfo ci) throws LWJGLException {
    System.out.println("Slice example!");
}
```

## 🥳 You know have learnt how slices work

![Slice output](../../res/SL_SLICE_OUTPUT.png)

[View slices code here](https://github.com/SkidKit/MixinClientTutorial)
* * *

### How can I get help, ask questions, etcetera?
The easiest way is to join my *Discord server*, which you can find here
[https://discord.gg/45RhZT5e9Z](https://discord.gg/45RhZT5e9Z)
