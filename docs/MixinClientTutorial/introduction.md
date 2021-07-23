# 1 | Introduction
* * *
#### What are mixins?
Mixins are a Java framework that allows you to modify classes at runtime

#### Ok? But why use it?
So, we are going to be using it to mod minecraft, with a combination of ForgeGradle and mixin, you need to modify Minecraft code to do certain things, and that usually involves heaving hundreds of thousands of lines of code, just to make a simple client.

Mixin allows us to avoid that by allowing us to modify small amounts of code at runtime, while not having to deal with the entirety of Mojang's biggest codebase.

#### How can I get help?
The easiest way is to join my *Discord server*, which you can find here
[https://discord.gg/45RhZT5e9Z](https://discord.gg/45RhZT5e9Z)


* * *
## Prerequisites
### Software

- If you haven't already [install JDK 8](https://adoptopenjdk.net/?variant=openjdk8&jvmVariant=hotspot)
- Install an IDE, I would suggest installing [Intellij IDEA](https://www.jetbrains.com/idea/download/), but there are other IDE's you can install
- [MinecraftDev plugin](https://minecraftdev.org/), this is extremely helpful, can do cool things like autocompleting method bytecode signatures and letting you breakpoint on mixins

### Java
I do expect at least a basic understanding of Java and different design patterns.

!!! note
    The ⭐'s show how hard it is to use each resource
However, if you lack this, I can point you in the direction of a few guides pertaining to Java:
#### Textual:

⭐⭐⭐

[Oracle's own tutorial](https://docs.oracle.com/javase/tutorial/)

⭐⭐

[MOOC.fi tutorial](https://moocfi.github.io/courses/2013/programming-part-1/material.html)

#### Videos:

⭐

[The new boston tutorial](https://youtu.be/Hl-zzrqQoSE)
* * *
### Design patterns
While it's not particularly needed, it is good to have a basic idea of the staple design patterns, I will go over the ones that I do use, though.

[Here you may find some design pattern stuff](https://refactoring.guru/)