# Layouts

* * *

## About

!!! note
    Layouts are loosely based on [this](https://developer.mozilla.org/en-US/docs/Web/CSS/position).

Currently there are *2* types of layouts:

- `ABSOLUTE`
- `RELATIVE`

Which are stored in the enum `LayoutTypes`.

* * *
## Creating a layout

!!! note
    `POSITION` should be subsituted with an Integer value

    `LAYOUT_TYPE` should be substituted with a member of the LayoutTypes enum
```java linenums="1"
new Layout(POSITION, LayoutTypes.LAYOUT_TYPE)
```

* * *
## Polling position from layout
!!! note
    `CONSTRAINT_X` should be subsituted with an Integer value

    What each layout type will return:

    - `ABSOLUTE` = $position$

    - `RELATIVE` = $constraint / position$

!!! example
    ```java linenums="1"
    /*
     * LayoutType.RELATIVE example
     * 5 = position (Integer)
    */
    Layout layout = new Layout(5, LayoutType.RELATIVE);

    System.out.println(layout.calculate position(2));

    // Outputs 5 / 2 = 2.5

    ...

    /*
     * LayoutType.ABSOLUTE example
     * 10 = position (Integer)
    */
    Layout layout = new Layout(5, LayoutType.ABSOLUTE);

    System.out.println(layout.calculate position(2));

    // Outputs 10

    ```

```java linenums="1"
layout.calculatePosition(CONSTRAINT_X)
```

* * *

## Setters and getters

* * *

### GetType
#### Return value: `LayoutTypes` type value of the `Layout` object
```java linenums="1"
public LayoutTypes getType();
```

* * *

### GetPosition
#### Return value: `Integer` position value of the `Layout` object
```java linenums="1"
public int getPosition();
```

* * *

### SetPosition
#### Return value: `void`
#### Params: `int` position value to set the `Layout` object's position to
```java linenums="1"
public void setPosition(int position)
```

* * *

### SetType
#### Return value: `void`
#### Params: `LayoutTypes` type value to set the `Layout` object's layout type to
```java linenums="1"
public void setType(LayoutTypes type)
```
