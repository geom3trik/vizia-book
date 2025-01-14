# Composing Views

Composition of views is achieved through container views. These views take a closure which allows us to build child views within them. Some container views may also arrange their content in a particular way.

For example, the `HStack` container view will arrange its contents into a horizontal row. Let's use this to declare the rest of the views for our counter application:

```rust
use vizia::prelude::*;

fn main() -> Result<(), ApplicationError> {
    Application::new(|cx|{

        HStack::new(cx, |cx|{
            Button::new(cx, |cx| Label::new(cx, "Decrement"));
            Button::new(cx, |cx| Label::new(cx, "Increment"));
            Label::new(cx, "0");
        });
    })
    .title("Counter")
    .inner_size((400, 150))
    .run()
}
```

For now we have hard-coded the label to display the number 0, but later we will hook this up to some model data so that it updates when the data changes. We've also removed the modifiers from the label, as we'll be replacing these with CSS styling later on.

Note that the `Button` view is also a container, and is designed to allow things like a button with both text and a leading or trailing icon.

<p align="center">
<img src="img/composing_views.png" alt="A vizia app showing two buttons and a label"/>
</p>

Composing views together forms a tree, where each view has a single parent and zero or more children. For example, for the code above the view tree can be depicted with the following diagram:

<p align="center">
<img src="img/view_tree.svg" alt="Diagram of the vizia application view tree"/>
</p>