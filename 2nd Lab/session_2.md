# Basic Jetpack Compose Composables

Jetpack Compose introduces basic composables like `Column`, `Row`, `Box`, and others, which act as building blocks for creating layouts. These composables provide a declarative way to design user interfaces.

## 1. Column

### Definition:

`Column` arranges its child composables vertically, one below the other.


### Features and Properties:
* Alignment: Control how children align horizontally using `horizontalAlignment`.
* Spacing: Adjust space between children with `verticalArrangement`.
* Scrollable: Can be made scrollable using `Modifier.verticalScroll`.


### Advantages:
* Easy to arrange multiple components in a vertical order.
* Highly flexible for vertically scrolling content.


### Use Cases:
* Displaying lists or forms.
* Stacking text fields, buttons, or images vertically.


### Example:

```kotlin

Column(
    verticalArrangement = Arrangement.spacedBy(8.dp),
    horizontalAlignment = Alignment.CenterHorizontally
) {
    Text("Item 1")
    Text("Item 2")
    Button(onClick = {}) { Text("Click Me") }
}

```



