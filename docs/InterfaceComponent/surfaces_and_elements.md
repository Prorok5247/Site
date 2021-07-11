# Surfaces & Elements

=== "Surface"
    ## Surface
    A class that loops through and renders `Element` objects, it also defines a top level `Element` that you can add to

    !!! warning
        `Primitives` is largely incomplete

    A surface contains a `Primitives` interface, which defines methods for drawing, and can be overriden

    ```java linenums="1"

    // prim.java
    class prim implements Primitives
    {
        @Override
        public void draw(Element element) {
            System.out.println("Drew a thing.");
        }
    }

    // main.java
    Surface surface = new Surface(new prim());
    ```

    ## Adding elements
    Element addition is done using method chaining, much like with [`Stylesheets`](stylesheets.md)

    !!! note
        `ELEMENT` should be substituted with the `Element` object that you would like to add
    ```java linenums="1"
    Surface surface = new Surface(new prim())
        .addElement(ELEMENT);
    ```

    * * *

    ## Rendering
    This can be simply done with the method `render()`

    ```java linenums="1"
    surface.render()
    ```

    * * *
    ## Getters and setters
    * * *
    ### GetScreen
    #### Return value: `Element` screen value, returns the top level element
    ```java linenums="1"
    public Element getScreen()
    ```

=== "Element"
    ## Element
    An individual element of the UI, it is constructed with a `Stylesheet` and a set of `ids`

    Elements are arranged in a tree like structure

    !!! note
        `STYLESHEET` should be substituted with a `Stylesheet` object

        `ID1, ID2, ...` should be substituted with the `String` ids you want the object to have
    ```java linenums="1"
    new Element(STYLESHEET, ID1, ID2, ...)
    ```

    ## Adding elements
    Element addition is done using method chaining, much like with [`Stylesheets`](stylesheets.md)

    !!! note
        `ELEMENT` should be substituted with the `Element` object that you would like to add
    ```java linenums="1"
    element
        .add(ELEMENT)
        .add(...);
    ```

    * * *

    ## Iterating through an Element

    This can simply be done with a loop

    !!! note
        `ROOT_ELEMENT` should be substituted with the `Element` object that you would like to loop from
    ```java linenums="1"
    for (Element e : ROOT_ELEMENT)
    {
        // ...
    }
    ```

    * * *
    ## Getters and setters

    * * *

    ### GetChildren
    #### Return value: `ArrayList<Element>` the children of the `Element`
    ```java linenums="1"
    public ArrayList<Element> getChildren()
    ```

    * * *

    ### GetParent
    #### Return value: `Element` the `Element`'s parent
    ```java linenums="1"
    public Element getParent()
    ```

    * * *

    ### IsRoot
    #### Return value: `boolean` whether this is the top level Element in the tree
    ```java linenums="1"
    public boolean isRoot()
    ```

    * * *

    ### GetRoot
    #### Return value: `Element` the heirachial root of the `Element` tree
    ```java linenums="1"
    public Element getRoot()
    ```

    * * *

    ### GetSize
    #### Return value: `Box` the size of the `Element`
    ```java linenums="1"
    public Box getSize()
    ```

    * * *

    ### GetPosition
    #### Return value: `Pair<Layout, Layout>` the position of the `Element`
    ```java linenums="1"
    public Pair<Layout, Layout> getPosition()
    ```

    * * *

    ### GetCalculatedPositionX
    #### Return value: `int` a calculated x position based on the size of the parent `Element`
    ```java linenums="1"
    public int getCalculatedPositionX()
    ```

    * * *

    ### GetCalculatedPositionY
    #### Return value: `int` a calculated y position based on the size of the parent `Element`
    ```java linenums="1"
    public int getCalculatedPositionY()
    ```

    * * *

    ### GetStylesheet
    #### Return value: `Stylesheet` the `Element`'s stylesheet
    ```java linenums="1"
    public Stylesheet getStylesheet()
    ```

    * * *

    ### GetIds
    #### Return value: `String[]` the `ids` the `Element` has
    ```java linenums="1"
    public String[] getIds()
    ```

    * * *

    ### SetSize
    #### Return value: `void`
    #### Params: `Box` size to set the `Element`'s size to
    ```java linenums="1"
    public void setSize(Box size)
    ```

    * * *

    ### SetPosition
    #### Return value: `void`
    #### Params: `Pair<Layout, Layout>` position value to set the `Element` object's position to
    ```java linenums="1"
    public void setPosition(Pair<Layout, Layout> position)
    ```