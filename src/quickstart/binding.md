# Lenses and Data Binding

Now that we have some model data we can *bind* the `count` to the `Label` view. 

Data binding is the concept of linking model data to views, so that when the model data is changed, the views observing this data update in response.

In Vizia, data binding is achieved through the use of lenses. A lens is an object which allows you to *select* some part of a model and inspect its value. These lens objects are then used to form a binding between views and these parts of the model, updating when only these specific parts have changed.


## Generating lenses

The `Lens` derive macro can be used to generate a lens for each field of a struct. These lenses can then be used to transform a reference to the struct into a reference to each of its fields. The generated lenses are given the same name as the field and placed in a module with the same name as the struct. For example, deriving `Lens` on the model we defined before:

```rust
#[derive(Lens)]
pub struct AppData {
    count: i32,
}

impl Model for AppData {}
```

A lens to the `count` field of the `AppData` struct is generated as `AppData::count`.

## Binding the label

With the generated `AppData::count` lens, we can bind the `count` data to the `Label` by passing the lens in place of the string:

```rust
use vizia::prelude::*;

fn main() -> Result<(), ApplicationError> {
    Application::new(|cx|{

        AppData { count: 0 }.build(cx);

        HStack::new(cx, |cx|{
            Button::new(cx, |cx| Label::new(cx, "Decrement"))
                .class("dec");
            Button::new(cx, |cx| Label::new(cx, "Increment"))
                .class("inc");
            Label::new(cx, AppData::count) // Bind the label to the count data
                .class("count");
        })
        .class("row");
    })
    .title("Counter")
    .inner_size((400, 100))
    .run()
}
```

This sets up a binding which updates the value of the label whenever the `count` changes. We can depict this with the following diagram, where the green arrow shows the direct link between the data and the label:

<p align="center">
<img src="img/binding.svg" alt="Diagram of model-view-tree with an arrow from 'AppData' to 'Label' representing data binding"/>
</p>

## Modifier bindings
Many modifiers also accept a lens as well as a value. When a lens is supplied to a modifier, a binding is set up which will update the modified property when the bound to model data changes. For example:

```rust

#[derive(Lens)]
pub struct AppData {
    color: Color,
}

...

Label::new(cx, "Hello World")
    .background_color(AppData::color);
```

