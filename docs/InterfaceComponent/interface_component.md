---
title: SkidKit site
summary: Documentation and articles pertaining to SkidKit
authors:
    - Basilicous
url: https://basil.is-a.dev/
---

# Interface component
* * *

!!! warning
    This library is largely untested and incomplete, please heed warning before using it.

#### Where can I get it?
[Right here (github link)](https://github.com/SkidKit/InterfaceComponent)

#### What?
A bare-bones set of tools to make your own GUI framework, completely generic, and Java 8 compliant.

#### Why?
I noticed a severe lack of GUI frameworks with the ability to be able to be used anywhere, so I created this to expand options

#### Design goals
- An extensible DOM based UI framework
- Some sort of stylesheet system
  - Options:
    - Code based (due to requirement of ease of use and the fastest option)
- Box model
- Extensibility via multiple backend support, that can easily be implemented by user
- No dependencies, this is non negotiable

* * *
## Usage example
!!! example
    ```java linenums="1"
    public class TestInterfaceComponent
    {
        public static Stylesheet stylesheet = new Stylesheet()
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


        public static void main(String[] args)
        {
            System.out.println("------------------------------\n\nRUNNING TESTS:\n\n------------------------------\n");
            testStylesheet();
            testElement();
            System.out.println("\n------------------------------\n\nEND TESTS\n\n------------------------------");
        }

        public static void testStylesheet()
        {
            System.out.println("Stylesheet test: " + stylesheet.getRules()
                    .get(0)
                    .getOptions()
                    .get(1)
                    .getOption()
                    .getRight());
        }

        public static void testElement()
        {
            Surface surface = new Surface(new Primitives() {
                @Override
                public void draw(Element element) {
                    System.out.println("Drew a thing.");
                }
            }).addElement(
                    new Element(
                            stylesheet,
                            "sl1-element", "test-1", "test-2")
            ).addElement(
                    new Element(
                            stylesheet,
                            "sl2-element", "test-1", "test-2")
            );

            System.out.println("Element test 1 (1): " +
                    surface.getScreen().getChildren().get(0).getCalculatedPositionX()
            );

            System.out.println("Element test 1 (2): " +
                    surface.getScreen().getChildren().get(0).getCalculatedPositionY()
            );

            System.out.println("\nElement test 2: " +
                    surface.getScreen().getChildren().get(0).getSize().getBorder().getWidth()
            );

            System.out.println("\nElement test 3: ");
            surface.render();

            System.out.println("\nElement test 4: ");
            System.out.println(surface.getScreen().getChildren().get(0).getRoot().getIds()[0]);
        }
    }
    ```
* * *
