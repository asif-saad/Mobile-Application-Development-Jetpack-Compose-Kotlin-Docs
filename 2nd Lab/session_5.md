# ViewModel in Jetpack Compose



### What is a ViewModel?
---

**Definition**:
A `ViewModel` is a class designed to store and manage UI-related data in a lifecycle-conscious way. It allows data to survive configuration changes like screen rotations. `ViewModel` in Android is used for managing UI-related data in a lifecycle-conscious way. It serves as an abstraction layer between the UI (Activity/Fragment) and the data layer (e.g., repositories, databases, or network sources), ensuring that your app’s data survives configuration changes (like screen rotations) and remains consistent.

**Purpose**:
It helps separate the UI logic from the business logic, making your code cleaner and easier to maintain.

### Why Use ViewModel with Jetpack Compose?
---

**State Management**:
`ViewModel` holds the app's data that the UI observes. When data changes, the UI automatically updates.

**Lifecycle Awareness**:
`ViewModel` is aware of the app's lifecycle, so it won't be destroyed on configuration changes.

**Separation of Concerns**:
Keeps your composables focused on UI rendering, while the `ViewModel` handles data and business logic.





### Real-Life Example: To-Do List App
---

Let's create a simple To-Do List app where you can add tasks, and the list updates automatically. We'll use `ViewModel` to manage the list of tasks.

### Step 1: Add Dependencies

Make sure you have these dependencies in your `build.gradle` file:

```groovy
implementation "androidx.lifecycle:lifecycle-viewmodel-compose:2.6.1"
implementation "androidx.activity:activity-compose:1.7.2"
```

### Step 2: Create the ViewModel

```kotlin
import androidx.lifecycle.ViewModel
import androidx.compose.runtime.mutableStateListOf

class TodoViewModel : ViewModel() {
    // Holds the list of tasks
    var tasks = mutableStateListOf<String>()
        private set

    // Function to add a new task
    fun addTask(task: String) {
        tasks.add(task)
    }
}
```

#### Explanation:

- **`mutableStateListOf<String>()`**:
  A special list that Jetpack Compose observes for changes. When you add or remove items, the UI updates automatically.

- **`private set`**:
  Prevents external classes from modifying the `tasks` list directly.





### Step 3: Create the UI Composable


```kotlin
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.text.KeyboardActions
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.input.ImeAction
import androidx.compose.ui.unit.dp

@Composable
fun TodoApp(viewModel: TodoViewModel) {
    // State for the text field
    var text by remember { mutableStateOf("") }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        // Input field to add new tasks
        OutlinedTextField(
            value = text,
            onValueChange = { text = it },
            label = { Text("Enter a task") },
            modifier = Modifier.fillMaxWidth(),
            keyboardOptions = KeyboardOptions.Default.copy(imeAction = ImeAction.Done),
            keyboardActions = KeyboardActions(
                onDone = {
                    if (text.isNotBlank()) {
                        viewModel.addTask(text)
                        text = "" // Clear the text field
                    }
                }
            )
        )

        Spacer(modifier = Modifier.height(16.dp))

        // Display the list of tasks
        Text("Tasks:", style = MaterialTheme.typography.titleMedium)
        Spacer(modifier = Modifier.height(8.dp))

        for (task in viewModel.tasks) {
            Text("• $task")
        }
    }
}
```


#### Explanation:

- **Text Input**:
  - `OutlinedTextField`: Allows the user to type in a new task.
  - `keyboardActions`: Handles the action when the user presses the "Done" button on the keyboard.

- **Clearing Input**: After adding a task, we reset `text` to an empty string.

- **Displaying Tasks**:
  - We loop through `viewModel.tasks` and display each task with a bullet point.





### Step 4: Set Up the Activity


```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.lifecycle.viewmodel.compose.viewModel

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            // Obtain the ViewModel
            val todoViewModel: TodoViewModel = viewModel()

            // Call the UI composable
            TodoApp(viewModel = todoViewModel)
        }
    }
}
```




#### Explanation:

- `viewModel()`: Retrieves an instance of `TodoViewModel`. Compose provides this function to get the ViewModel in a composable-friendly way.

### Step 5: Run the App

When you run the app:

- You can type a task into the text field.
- Press "Done" on the keyboard to add the task.
- The list of tasks updates immediately.



<br><br><br>

## Key Concepts Explained

1. **ViewModel Holds the Data**:
   - The `TodoViewModel` keeps the list of tasks.
   - Even if the screen rotates, the tasks remain because the ViewModel survives configuration changes.

2. **UI Observes the ViewModel**:
   - The `TodoApp` composable reads data from `viewModel.tasks`.
   - When tasks change, the UI recomposes automatically.

3. **Separation of Concerns**:
   - **ViewModel**: Manages the data and business logic (adding tasks).
   - **Composable Function**: Handles the UI rendering based on the data from the ViewModel.



<br><br><br>

## Advantages of Using ViewModel with Compose

1. **Data Persistence**: Data survives configuration changes, like screen rotations.
2. **Clean Architecture**: Separates UI code from business logic, making the app easier to maintain and test.
3. **Automatic UI Updates**: When data in the ViewModel changes, the UI updates without manual intervention.
4. **Lifecycle Awareness**: ViewModel is tied to the lifecycle of the activity or fragment, preventing memory leaks.




<br><br><br>


### Additional Tips
---

- **Remember to Import Correct Packages**: Ensure you import `androidx.lifecycle.viewmodel.compose.viewModel` for the `viewModel()` function.
- **Testing**: ViewModels are easier to test since they don't depend on the UI framework.
- **Scalability**: As your app grows, ViewModel helps manage complex data and state efficiently.
