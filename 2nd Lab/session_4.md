# `remember` and `mutableStateOf` in Jetpack Compose




## Real-Life Example: A Simple Counter App


This will demonstrate how `remember` and `mutableStateOf` work together.

```kotlin

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.* // Import for remember and mutableStateOf
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Set the content of the activity
        setContent {
            // Call our composable function
            CounterApp()
        }
    }
}

@Composable
fun CounterApp() {
    // Remember the counter state
    var count by remember { mutableStateOf(0) }

    // UI Layout
    Column(
        modifier = Modifier
            .fillMaxSize() // Fill the available screen space
            .padding(16.dp), // Add padding around the content
        verticalArrangement = Arrangement.Center, // Center vertically
        horizontalAlignment = Alignment.CenterHorizontally // Center horizontally
    ) {
        // Display the counter value
        Text(
            text = "You have clicked $count times",
            style = MaterialTheme.typography.headlineMedium
        )

        Spacer(modifier = Modifier.height(16.dp)) // Add space between items

        // Button to increase the counter
        Button(
            onClick = {
                count += 1 // Increase the counter by 1
            }
        ) {
            Text(text = "Click Me")
        }
    }
}
```





### Explanation of the Code

1. **Imports:**
   - We import `androidx.compose.runtime.*` to use `remember` and `mutableStateOf`.

2. **MainActivity:**
   - The `onCreate` method sets the content to our `CounterApp` composable.

3. **CounterApp Composable:**

   - **State Declaration:**

   ```kotlin
   var count by remember { mutableStateOf(0) }
   ```
   
    `var count` We declare a variable `count` that can change. `by` Kotlin property delegate that allows us to write `count` instead of `count.value`. `remember` Tells Compose to remember the value of `count` across recompositions. `mutableStateOf(0)` Initializes `count` to `0` and creates a state object.

    - **Layout:** `Column`: Arranges items vertically.

    - **Modifiers:** `fillMaxSize()` Makes the column fill the screen. `padding(16.dp)` Adds space around the content. `verticalArrangement` and `horizontalAlignment` Centers the content.

- **Displaying the Counter:**

```kotlin
Text(
  text = "You have clicked $count times",
  style = MaterialTheme.typography.headlineMedium
)
```

Shows the current value of `count`.



- **Displaying the Counter:**

```kotlin
Button(
    onClick = {
        count += 1
    }
) {
    Text(text = "Click Me")
}
```

When clicked, increases `count` by 1.
Because `count` is a mutable state, the UI automatically updates to show the new value.
