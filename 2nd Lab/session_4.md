# `remember` and `mutableStateOf` in Jetpack Compose




## Real-Life Example: A Simple Counter App

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