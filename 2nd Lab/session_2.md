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



## 2. Row

### Definition:

`Row` arranges its child composables horizontally, 
side by side.


### Features and Properties:
* Alignment: Control vertical alignment using `verticalAlignment`.
* Spacing: Adjust space between children with `horizontalArrangement`.
* Weight: Distribute space between children proportionally.


### Advantages:
* Simplifies horizontal arrangement.
* Can combine with other composables for advanced layouts.


### Use Cases:
* Navigation bars.
* Aligning images with text in a single row.


### Example:


```kotlin

Row(
    horizontalArrangement = Arrangement.spacedBy(8.dp),
    verticalAlignment = Alignment.CenterVertically
) {
    Icon(Icons.Default.Home, contentDescription = "Home")
    Text("Home")
}


```








## 3. Box

### Definition:

`Box` allows composables to be placed on top of each other, offering z-order stacking. It also provides alignment within its bounds.


### Features and Properties:
* Alignment: Position children using `contentAlignment`.
* Modifiers: Supports background, padding, and border.
* Overlap: Useful for overlays and layering components.


### Advantages:
* Flexible for creating layered layouts.
* Simplifies alignment of a single child or overlapping items.


### Use Cases:
* Adding text overlays on images.
* Positioning floating action buttons.


### Example:


```kotlin

Box(
    modifier = Modifier.size(200.dp),
    contentAlignment = Alignment.Center
) {
    Image(painter = painterResource(id = R.drawable.sample), contentDescription = "Image")
    Text("Overlay Text", color = Color.White)
}



```



## Advantages of Basic Composables

1. <b>Declarative Syntax:</b> Simplifies UI creation by defining the desired output instead of the steps to create it.
2. <b>Less Boilerplate:</b> Removes the need for XML layouts, reducing code redundancy.
3. <b>Customizable:</b> Easily modify properties (padding, size, color) using Modifier.
4. <b>Composability:</b> Combine basic composables to create complex, reusable UI components.
5. <b>Integration:</b> Work seamlessly with modern Android libraries and APIs.





## Comparison Table
| Composable | Layout Behavior | Common Attributes | Best Use Cases |
|----------|----------|----------|----------|
|  Column | Vertical arrangement  | `verticalArrangement`, `horizontalAlignment`  | Forms, lists, vertical stacking  |
| Row  | Horizontal arrangement  | `horizontalArrangement`, `verticalAlignment`  |  Navigation bars, horizontal alignment |
| Box  | Overlapping composables (z-order)  | `contentAlignment`, `Modifier`  |  Overlays, floating action buttons |
|





### Example of `Column` with working attributes and properties
___


```kotlin

@Composable
fun ColumnExample() {
    Column(
        modifier = Modifier
            .fillMaxSize() // Fills the available screen size
            .padding(16.dp) // Adds padding around the column
            .background(Color.LightGray) // Sets background color
            .border(2.dp, Color.Black) // Adds a border around the column
            .verticalScroll(rememberScrollState()), // Makes the column scrollable
        verticalArrangement = Arrangement.spacedBy(16.dp), // Space between children
        horizontalAlignment = Alignment.CenterHorizontally // Align children horizontally to the center
    ) {
        // Child 1: Text
        Text(
            text = "Hello, Compose!",
            fontSize = 20.sp,
            fontWeight = FontWeight.Bold,
            color = Color.Black,
            modifier = Modifier.padding(8.dp) // Padding inside the child
        )

        // Child 2: Button
        Button(
            onClick = { /* Handle click */ },
            modifier = Modifier
                .align(Alignment.Start) // Overrides column alignment for this child
        ) {
            Text(text = "Click Me")
        }

        // Child 3: Image
        Image(
            painter = painterResource(id = R.drawable.sample),
            contentDescription = "Sample Image",
            modifier = Modifier
                .size(100.dp) // Fixed size for the image
                .clip(CircleShape) // Circular shape for the image
        )

        // Child 4: Spacer
        Spacer(modifier = Modifier.height(24.dp)) // Adds empty vertical space

        // Child 5: Box inside Column
        Box(
            modifier = Modifier
                .size(100.dp)
                .background(Color.Blue)
                .align(Alignment.End) // Aligns this box to the end of the column
        ) {
            Text(
                text = "Inside Box",
                color = Color.White,
                modifier = Modifier.align(Alignment.Center) // Aligns text inside the box
            )
        }
    }
}



```












### Explanation of Attributes and Properties Used:

1. **Modifier**:
   - `fillMaxSize`: Makes the column occupy the entire screen.
   - `padding`: Adds inner spacing around the column.
   - `background`: Sets the background color of the column.
   - `border`: Adds a border around the column.
   - `verticalScroll`: Enables scrolling when content overflows.

2. **verticalArrangement**:
   - Defines vertical spacing or alignment of children inside the column.
   - Example: `Arrangement.spacedBy(16.dp)` creates 16dp space between children.

3. **horizontalAlignment**:
   - Aligns children horizontally (left, center, right).
   - Example: `Alignment.CenterHorizontally` centers all children horizontally.

4. **Child-Specific Modifiers**:
   - `align`: Overrides the column's alignment for specific children.


5. **Child Types**:
   - `Text`: Displays text with customizable styles.
   - `Button`: Adds interactive buttons.
   - `Image`: Displays images with transformations.
   - `Spacer`: Adds empty space between children.
   - `Box`: Nested layout for overlapping or layered content.
