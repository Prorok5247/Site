---
title: SkidKit site
summary: Documentation and articles pertaining to SkidKit
authors:
    - Basilicous
url: https://basil.is-a.dev/
---

# Box model

* * *

## About

!!! note
    My box model implementation is loosely based on [this](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model).

The box model has:

- `Margin`
- `Border`
- `Padding`

The box model is used for sizing and positioning elements

<figure>
  <img src="../../res/BoxModel.png" />
  <figcaption>Image credit: <a href="https://www.w3schools.com/Css/css_boxmodel.asp">W3Schools</a></figcaption>
</figure>


* * *

## Creating a box

!!! note
    `PADDING` should be substituted with an `Integer` value for padding

    `LINETYPE` should be substituted with a member of the `LineType` enum

    `BORDER_WIDTH` should be substituted with an `Integer` value for the border's width

    `R, G, B` should be substituted with `Integer` values for `red`, `green` and `blue`

    `MARGIN `should be substituted with an `Integer` value for margin


```java linenums="1"
new Box(
    PADDING,
    new Border(BORDER_WIDTH, LINETYPE, new Colour(R, G, B)), 
    MARGIN)
```

* * *

## Polling size from box
!!! note
    Size is calculated like so: 

    $padding + borderwidth + margin$

!!! example
    ```java linenums="1"
    Box box = new Box(
        20,
        new Border(30, LINETYPE, new Colour(255, 255, 255)), 
        50);

    System.out.println(box.getFull())

    // Outputs 20 + 30 + 50 = 100
    ```

```java linenums="1"
box.getFull()
```

* * *
## Getters and setters

* * *
### GetBorder
#### Return value: `Box` object
```java linenums="1"
public Border getBorder();
```
* * *
### GetMargin
#### Return value: `Integer` margin value of the `Box` object
```java linenums="1"
public int getMargin();
```

* * *
### GetPadding
#### Return value: `Integer` padding value of the `Box` object
```java linenums="1"
public int getPadding();
```

* * *
### GetFull
#### Return value: `Integer` full size value of the `Box` object
```java linenums="1"
public int getFull();
```

* * *
### SetBorder
#### Return value: `void`
#### Params: `Border` border value to set the `Box` object's border to
```java linenums="1"
public void setBorder(Border border);
```

* * *
### SetBorderType
#### Return value: `void`
#### Params: `LineType` border line type value to set the `Box` object's border's line type to
```java linenums="1"
public void setBorderType(LineType lineType);
```

* * *
### SetBorderColour
#### Return value: `void`
#### Params: `Colour` border colour value to set the `Box` object's border's colour to
```java linenums="1"
public void setBorderColour(Colour colour);
```

* * *
### SetBorderSize
#### Return value: `void`
#### Params: `Integer` border size value to set the `Box` object's border's size to
```java linenums="1"
public void setBorderSize(int size);
```

* * *
### SetMargin
#### Return value: `void`
#### Params: `Integer` margin size value to set the `Box` object's margin size to
```java linenums="1"
public void setMargin(int margin);
```

### SetPadding
#### Return value: `void`
#### Params: `Integer` padding size value to set the `Box` object's padding size to
* * *
```java linenums="1"
public void setPadding(int padding);
```