---
title: SkidKit site
summary: Documentation and articles pertaining to SkidKit
authors:
    - Basilicous
url: https://basil.is-a.dev/
---

# Stylesheets

* * *

## About

!!! note
    Stylesheets are **very** loosely based on [this](https://developer.mozilla.org/en-US/docs/Web/CSS).

Stylesheets have a rule based structure, like so:

!!! note
    Stylesheets have a rule based system, so you add a rule with a ID to specify which `Element`s you want to modify, then you can add `Option`s of different types
```json linenums="1"
"ID"
{
    "Position": 20
    ...
}
```

* * *
## Making style sheets
My code based implementation of stylesheets uses [method chaining](https://www.geeksforgeeks.org/method-chaining-in-java-with-examples/) to build a stylesheet.

!!! warning
    This example showcases specific options with specific parameters.
    
    However, you may add as many, or as little options as you want, and change the parameters to the options how you want.

    All options are optional.
```java linenums="1"
new Stylesheet()
    .addRule(
        new Rule("test-1")
            .addOptionInt("width", 200)
            .addOptionString("type", "relative")
            .addOptionBox("box", new Box(20, new Border(20, LineType.SOLID, new Colour(255, 255, 255)), 50))
    )
    .addRule(
        new Rule("test-2")
            .addOptionLayout("top",  new Layout(2, LayoutTypes.RELATIVE))
            .addOptionLayout("left",  new Layout(2, LayoutTypes.RELATIVE))
        );
```

* * *
## Rules
A rule takes one parameter `id`, this specifies the scope that this rule can modify.

!!! note
    ID should be subsituted with a `String` value
```java linenums="1"
new Rule(ID)
```

You may then chain on `Option`s to the Rule object

!!! note
    `ID` should be subsituted with a `String` value

    `TYPE` should be subsituted with the end to a method prefixed with `addOption` in the `Rule` class

    `NAME` should be subsituted with a `String` value

    `VALUE` should be substituted with the value `addOptionTYPE` accepts
```java linenums="1"
    new Rule(ID)
        .addOptionTYPE(NAME, VALUE);
```
* * *
## Polling a stylesheet box
You can get a box from the stylesheet with the method [`parseIntoBox`](https://github.com/SkidKit/InterfaceComponent/blob/c126dcc99123f4e3f9d16c26349d0f2706d5b12d/src/main/java/tech/lowspeccorgi/interfacecomponent/style/Stylesheet.java#L32-L57)

!!! example
    ```java linenums="1"
    new Stylesheet()
        new Rule("test-1")
            .addOptionBox(
                "box", 
                new Box(20, new Border(20, LineType.SOLID, new Colour(255, 255, 255)), 50));
    
    System.out.println(stylesheet.parseIntoBox().getMargin());

    // Outputs the margin of the box (50)
    ```

```java linenums="1"
stylesheet.parseIntoBox()
```
* * *
## Polling a stylesheet layout
You can get a box from the stylesheet with the ugly hunk of a method called [`parseIntoLayout`](https://github.com/SkidKit/InterfaceComponent/blob/c126dcc99123f4e3f9d16c26349d0f2706d5b12d/src/main/java/tech/lowspeccorgi/interfacecomponent/style/Stylesheet.java#L59-L116)

!!! example
    ```java linenums="1"
    new Stylesheet()
        new Rule("test-1")
            .addOptionLayout("top",  new Layout(2, LayoutTypes.RELATIVE))
            .addOptionLayout("left",  new Layout(2, LayoutTypes.RELATIVE))
    
    System.out.println(stylesheet.parseIntoLayout().getPosition());

    // Outputs the position of the layout (N/A)
    ```

```java linenums="1"
stylesheet.parseIntoLayout()
```

* * *
## Getters and setters
* * *

### GetRules
#### Return value: `java.util.ArrayList<Rule>` rules that the stylesheet contains
```java linenums="1"
public ArrayList<Rule> getRules();
```