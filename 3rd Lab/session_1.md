## ViewModel in Kotlin with Jetpack Compose
### Definition
ViewModel is a part of Android Jetpack. It acts as a holder for business logic and screen-level state. (State means the current condition or data of the app's screen.) It shows UI state to the user interface while keeping business logic separate. This follows the MVVM pattern, where MVVM stands for Model-View-ViewModel, a way to organize app code.
### Functions
ViewModel keeps UI state safe during changes like screen rotations. It stores data so the app does not need to reload it each time. It gives access to business logic in the UI layer, manages events, and sends complex tasks to other parts of the app. It uses SavedStateHandle to save state if the app process restarts. (SavedStateHandle is a tool to store small amounts of data safely.) It works with Kotlin coroutines through viewModelScope for handling tasks that take time, like network calls. (Coroutines are a way to run code without blocking the main thread.)
### Usage
In Kotlin with Jetpack Compose, ViewModel shares screen UI state with composables. (Composables are building blocks for UI in Compose.) It connects to lifecycle owners like Activities or Fragments, not directly to composables. You create it by extending the ViewModel class and using MutableStateFlow to handle state. (MutableStateFlow is a container that holds changeable data and notifies when it updates.) Composables get the ViewModel with the ```viewModel()``` function and watch state using ```collectAsStateWithLifecycle()```




### Applications
ViewModel is useful when UI data must stay the same, like in shopping apps to keep cart items during screen turns. Or in social apps to remember scroll positions in feeds. It helps in apps with many screens, combining data from places like databases and the internet.
### Significance
ViewModel makes code easier to maintain by separating UI from data work. This allows better testing and fewer errors from lifecycle changes. In Jetpack Compose, it gives a clear way to manage state, making code reusable and stable. But you must handle its scope carefully for screen parts.



---


## LiveData in Kotlin with Jetpack Compose
### Definition
LiveData is a lifecycle-aware data holder from Android Jetpack. (Lifecycle-aware means it knows when the app's screen is active or not.) It is observable, so it tells watchers when data changes, but only if the screen is ready. This stops memory leaks and keeps the UI consistent. (Memory leaks happen when unused data stays in memory.)
### Functions
LiveData uses the observer pattern to update UI automatically when data changes. (Observer pattern is a design where objects watch for changes in others.) It removes watchers when screens close, gives the latest data when screens restart, and handles rotations well. It can change data with tools like ```Transformations.map()``` and ```switchMap()```. (Transformations are ways to modify LiveData values.) It combines sources using ```MediatorLiveData```. Updates use ```setValue()``` on the main thread or ```postValue()``` from background threads. (Threads are paths for code to run at the same time.)
Usage
In Kotlin with Jetpack Compose, LiveData is stored in a ViewModel and watched in composables with observeAsState(). But StateFlow is often better now for its fit with coroutines and Compose.


### Applications
LiveData fits well for real-time UI changes, like chat apps updating messages or stock apps showing live prices. It is good for mixing data from databases and networks, and sharing resources like system services.
### Significance
LiveData makes reactive coding simpler by handling lifecycles automatically and cutting extra code. (Reactive coding means the app responds to data changes.) This leads to stronger apps without leaks. In Jetpack Compose, it is still useful, but StateFlow is more common now for better mixing and testing with modern tools.

---

### build.gradle.kts
```kotlin
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    alias(libs.plugins.kotlin.compose)
}

android {
    namespace = "com.example.myapplication"
    compileSdk = 36

    defaultConfig {
        applicationId = "com.example.myapplication"
        minSdk = 28
        //noinspection OldTargetApi
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }
    kotlinOptions {
        jvmTarget = "11"
    }
    buildFeatures {
        compose = true
    }
}

dependencies {

    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.9.2")
    implementation("androidx.compose.runtime:runtime-livedata")
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.8.4")
    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.lifecycle.runtime.ktx)
    implementation(libs.androidx.activity.compose)
    implementation(platform(libs.androidx.compose.bom))
    implementation(libs.androidx.ui)
    implementation(libs.androidx.ui.graphics)
    implementation(libs.androidx.ui.tooling.preview)
    implementation(libs.androidx.material3)
    implementation(libs.androidx.runtime.livedata)
    testImplementation(libs.junit)
    androidTestImplementation(libs.androidx.junit)
    androidTestImplementation(libs.androidx.espresso.core)
    androidTestImplementation(platform(libs.androidx.compose.bom))
    androidTestImplementation(libs.androidx.ui.test.junit4)
    debugImplementation(libs.androidx.ui.tooling)
    debugImplementation(libs.androidx.ui.test.manifest)
}
```











### MainActivity.kt
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Button
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.livedata.observeAsState
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.myapplication.ui.theme.MyApplicationTheme
import androidx.lifecycle.viewmodel.compose.viewModel

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            CounterScreen()
        }
    }
}


@Composable
fun CounterScreen(viewModel: CounterViewModel = viewModel()) {
    val count by viewModel.count.observeAsState(0)

    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(text = "Count: $count", style = MaterialTheme.typography.headlineMedium)
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = { viewModel.increment() }) {
            Text("Increment")
        }
    }
}
```




### CounterViewModel.kt

```kotlin
package com.example.myapplication

import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class CounterViewModel : ViewModel() {
    private val _count = MutableLiveData(0)
    val count: LiveData<Int> get() = _count

    fun increment() {
        _count.value = (_count.value ?: 0) + 1
    }
}
```

