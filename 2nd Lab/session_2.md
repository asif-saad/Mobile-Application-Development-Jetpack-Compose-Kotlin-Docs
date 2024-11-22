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




<br><br>
## Comparison Table
| Composable | Layout Behavior | Common Attributes | Best Use Cases |
|----------|----------|----------|----------|
|  Column | Vertical arrangement  | `verticalArrangement`, `horizontalAlignment`  | Forms, lists, vertical stacking  |
| Row  | Horizontal arrangement  | `horizontalArrangement`, `verticalAlignment`  |  Navigation bars, horizontal alignment |
| Box  | Overlapping composables (z-order)  | `contentAlignment`, `Modifier`  |  Overlays, floating action buttons |
<br><br><br><br><br>



### Example of `Column` with working attributes and properties
---


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




<br><br><br><br><br>





### Example of `Row` with working attributes and properties
---


```kotlin
@Composable
fun RowExample() {
    Row(
        modifier = Modifier
            .fillMaxWidth() // Fills the available width of the screen
            .padding(16.dp) // Adds padding around the row
            .background(Color.LightGray) // Sets background color
            .border(2.dp, Color.Black) // Adds a border around the row
            .horizontalScroll(rememberScrollState()), // Makes the row scrollable if content overflows
        horizontalArrangement = Arrangement.spacedBy(12.dp), // Adds space between Text children
        verticalAlignment = Alignment.CenterVertically // Aligns children vertically to the center
    ) {
        // Child 1: Text
        Text(
            text = "Hello",
            fontSize = 18.sp,
            fontWeight = FontWeight.Bold,
            color = Color.Blue,
            modifier = Modifier.padding(8.dp) // Adds inner padding to the Text
        )

        // Child 2: Text
        Text(
            text = "Jetpack",
            fontSize = 20.sp,
            fontStyle = FontStyle.Italic,
            color = Color.Green,
            modifier = Modifier
                .align(Alignment.Top) // Overrides row's vertical alignment for this Text
        )

        // Child 3: Text
        Text(
            text = "Compose",
            fontSize = 22.sp,
            fontWeight = FontWeight.ExtraBold,
            color = Color.Red,
            modifier = Modifier
                .padding(4.dp) // Adds padding inside the Text
                .align(Alignment.Bottom) // Aligns this Text at the bottom of the row
        )
    }
}
```













### Explanation of Attributes and Properties Used

1. **Modifier**:
   - `fillMaxWidth`: Ensures the row spans the entire width of the screen.
   - `padding`: Adds spacing around the row.
   - `background`: Sets a background color.
   - `border`: Adds a border to the row for visual clarity.
   - `horizontalScroll`: Makes the row scrollable horizontally if the content overflows.

2. **horizontalArrangement**:
   - Defines how children are spaced horizontally within the row.
   - Example: `Arrangement.spacedBy(12.dp)` creates 12dp spacing between the Texts.

3. **verticalAlignment**:
   - Aligns children vertically relative to the row.
   - Example: `Alignment.CenterVertically` centers all children vertically within the row.

4. **Child-Specific Modifiers**:
   - `align`: Overrides the row's vertical alignment for a specific child (e.g., `Alignment.Top`, `Alignment.Bottom`).
5. **Text Customizations**:
   - `fontSize`: Sets the size of the text.
   - `fontWeight`: Adjusts the thickness of the text.
   - `fontStyle`: Applies styles like italic.
   - `color`: Changes the text color.














<br><br><br><br><br>





### Example of `Box` with working attributes and properties (utilising Overlapping feature)
---


```kotlin
@Composable
fun BoxExample() {
    Box(
        modifier = Modifier
            .fillMaxSize() // Fills the entire available screen space
            .padding(16.dp) // Adds padding around the Box
            .background(Color.LightGray) // Sets the background color of the Box
            .border(2.dp, Color.Black), // Adds a border to the Box
        contentAlignment = Alignment.Center // Aligns all children to the center by default
    ) {
        // Child 1: Text (Background-like Text)
        Text(
            text = "Background Text",
            fontSize = 24.sp,
            fontWeight = FontWeight.Bold,
            color = Color.Gray,
            modifier = Modifier.align(Alignment.CenterStart) // Aligns this Text to the start of the center
        )

        // Child 2: Text (Center Text)
        Text(
            text = "Center Text",
            fontSize = 28.sp,
            fontWeight = FontWeight.ExtraBold,
            color = Color.Blue
        ) // This Text aligns with the `contentAlignment` (center by default)

        // Child 3: Text (Overlay Text at Bottom)
        Text(
            text = "Bottom Right Text",
            fontSize = 18.sp,
            fontWeight = FontWeight.Medium,
            color = Color.Red,
            modifier = Modifier.align(Alignment.BottomEnd) // Places the Text at the bottom-right corner
        )
    }
}
```













### Explanation of Attributes and Properties Used

1. **Modifier**:
   - `fillMaxSize`: Makes the `Box` fill the entire screen space.
   - `padding`: Adds space around the `Box`.
   - `background`: Applies a background color.
   - `border`: Draws a border around the `Box`.

2. **contentAlignment**:
   - Specifies the default alignment for all children in the `Box`.
   - Example: `Alignment.Center` aligns all children to the center unless overridden.

3. **Child-Specific `Modifier.align`**:
   - Overrides the default `contentAlignment` for a specific child.
   - Examples:
     - `Alignment.CenterStart`: Aligns the child to the left of the center.
     - `Alignment.BottomEnd`: Aligns the child to the bottom-right corner.

4. **Text Customizations**:
   - `fontSize`: Adjusts the size of the text.
   - `fontWeight`: Specifies text thickness.
   - `color`: Changes the text color.





<br><br><br><br><br>










### Example of `Box` with working attributes and properties (Centering and Aligning a Single Element)
---

**Purpose**: Use `Box` to position a child element precisely within its parent, such as centering an item or aligning it to a specific corner.

```kotlin
@Composable
fun BoxAlignmentExample() {
    Box(
        modifier = Modifier
            .size(200.dp) // Sets a fixed size for the Box
            .background(Color.LightGray) // Applies a background color
            .border(2.dp, Color.Black) // Adds a border around the Box
    ) {
        Text(
            text = "Centered Text",
            fontSize = 18.sp,
            color = Color.Blue,
            modifier = Modifier.align(Alignment.Center) // Centers the Text within the Box
        )
    }
}

```




### Explanation:

**Box Modifier Properties**:
- `size(200.dp)`: The `Box` has a fixed width and height of 200 density-independent pixels.
- `background(Color.LightGray)`: Sets a light gray background color.
- `border(2.dp, Color.Black)`: Adds a 2dp black border.

**Child `Text` Modifier**:
- `align(Alignment.Center)`: Positions the `Text` in the center of the `Box`.

**Possible Alignments with `Modifier.align`**:
- `Alignment.TopStart`
- `Alignment.TopCenter`
- `Alignment.TopEnd`
- `Alignment.CenterStart`
- `Alignment.Center` (used in the example)
- `Alignment.CenterEnd`
- `Alignment.BottomStart`
- `Alignment.BottomCenter`
- `Alignment.BottomEnd`



<br><br><br><br><br>





### Example of `Box` with working attributes and properties (Positioning with Offsets)
---

**Purpose**: Position an element at an exact location inside the `Box` using offsets.

```kotlin
@Composable
fun BoxOffsetExample() {
    Box(
        modifier = Modifier
            .size(250.dp)
            .background(Color.Yellow)
            .border(2.dp, Color.Black)
    ) {
        Text(
            text = "Offset Text",
            fontSize = 16.sp,
            color = Color.Red,
            modifier = Modifier
                .offset(x = 50.dp, y = 100.dp) // Moves the Text 50dp right and 100dp down
        )
    }
}
```



### Explanation:

- `offset(x, y)`: Moves the child from its default position by the specified x and y distances.





<br><br><br><br><br>





### Example of `Box` with working attributes and properties (Backgrounds and Clickable Areas)
---

**Purpose**: Apply backgrounds, borders, or make an area clickable without affecting the child composable.

```kotlin
@Composable
fun BoxBackgroundExample() {
    Box(
        modifier = Modifier
            .padding(16.dp)
            .background(Color.LightGreen, shape = RoundedCornerShape(8.dp))
            .clickable { /* Handle click */ }
            .padding(16.dp) // Inner padding inside the Box
    ) {
        Text(
            text = "Text with Background",
            fontSize = 20.sp,
            color = Color.Black
        )
    }
}

```



### Explanation:

- **`Box` Modifier Properties**:
  - `padding(16.dp)`: Outer padding around the `Box`.
  - `background(Color.LightGreen, shape = RoundedCornerShape(8.dp))`: Sets a light green background with rounded corners.
  - `clickable`: Makes the entire `Box` respond to click events.
  - Second `padding(16.dp)`: Inner padding inside the `Box` around the `Text`.

- **Benefit**: The `Box` acts as a styled container for the `Text`, providing background color, padding, and click handling without modifying the `Text` itself.

---

<br><br><br>


### Advantages of Using `Box` for Positioning and Styling

1. **Precise Alignment**: Easily align a single child to any part of the container using `Modifier.align`.
2. **Background and Borders**: Apply backgrounds, borders, and shapes to the `Box` to style the container.
3. **Padding Management**: Control inner and outer padding separately for better layout design.
4. **Gesture Handling**: Use modifiers like `clickable`, `pointerInput`, or `draggable` on the `Box` to handle user interactions.
5. **Isolation**: Encapsulate styling and positioning logic within the `Box` without affecting the child composable.
