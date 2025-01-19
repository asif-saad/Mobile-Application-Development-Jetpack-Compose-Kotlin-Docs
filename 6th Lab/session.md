### MainActivity.kt

```kotlin
package com.example.read_write

import android.content.Context
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxHeight
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Button
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.material3.TextField
import androidx.compose.runtime.Composable
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.TextFieldValue
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import com.example.read_write.ui.theme.Read_writeTheme
import com.google.firebase.database.DataSnapshot
import com.google.firebase.database.DatabaseError
import com.google.firebase.database.DatabaseReference
import com.google.firebase.database.FirebaseDatabase
import com.google.firebase.database.ValueEventListener

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            Read_writeTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                ){

                    val firebaseDatabase = FirebaseDatabase.getInstance()
                    val databaseReference = firebaseDatabase.getReference("StudentInfo")

                    firebaseUI(LocalContext.current, databaseReference)
                }
            }
        }
    }
}

@Composable
fun firebaseUI(context: Context, databaseReference: DatabaseReference){
    var name = remember {
        mutableStateOf(TextFieldValue())
    }
    
    var rollNo = remember {
        mutableStateOf(TextFieldValue())
    }

    
    Column(
        modifier = Modifier.fillMaxHeight()
            .fillMaxWidth()
            .background(Color.White),
        verticalArrangement = Arrangement.Top,
        horizontalAlignment = Alignment.CenterHorizontally,
    ){


        Text(
            text="Firebase Database",
            modifier = Modifier.padding(15.dp),
            style = TextStyle(color = Color.Black, fontSize = 25.sp),
            fontWeight = FontWeight.Bold
        )

        TextField(value = name.value, onValueChange = {name.value = it},
            placeholder = {Text(text= "Enter your name")},
            modifier = Modifier.padding(15.dp)
                .fillMaxWidth(),

            textStyle = TextStyle(color = Color.Black, fontSize = 15.sp),
            singleLine = true
        )

        Spacer(modifier = Modifier.height(10.dp))


        TextField(value = rollNo.value, onValueChange = {rollNo.value = it},
            placeholder = {Text(text= "Enter your roll number")},
            modifier = Modifier.padding(15.dp)
                .fillMaxWidth(),

            textStyle = TextStyle(color = Color.Black, fontSize = 15.sp),
            singleLine = true
        )

        Spacer(modifier = Modifier.height(10.dp))


        Button(onClick = {

            val studobj = StudentObj(name.value.text, rollNo.value.text)

            databaseReference.addValueEventListener(object: ValueEventListener{
                override fun onDataChange(snapshot: DataSnapshot) {
                    databaseReference.setValue(studobj)
                    val dummyName = name.value.text
                    val dummyRoll = rollNo.value.text
                    Toast.makeText(context, "$dummyName $dummyRoll", Toast.LENGTH_LONG).show()
                }

                override fun onCancelled(error: DatabaseError) {
                    Toast.makeText(context, "Fail to add data to Firebase", Toast.LENGTH_LONG).show()
                }
            })
        },
            modifier = Modifier
                .padding(18.dp)
                .fillMaxWidth()
            ){
            Text(text = "Add data to Firebase", modifier = Modifier.padding(7.dp))
        }

        Spacer(modifier = Modifier.height(10.dp))


        Button(onClick = {


            databaseReference.addValueEventListener(object: ValueEventListener{
                override fun onDataChange(snapshot: DataSnapshot) {
                    val name = snapshot.child("studentName").getValue(String::class.java)
                    val rollNo = snapshot.child("studentRollNumber").getValue(String::class.java)
                    Toast.makeText(context, "Your data: $name $rollNo", Toast.LENGTH_LONG).show()
                }

                override fun onCancelled(error: DatabaseError) {
                    Toast.makeText(context, "Fail to read data to Firebase", Toast.LENGTH_LONG).show()
                }
            })
        },
            modifier = Modifier
                .padding(18.dp)
                .fillMaxWidth()
        ){
            Text(text = "Read data from Firebase", modifier = Modifier.padding(7.dp))
        }
    }
    
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    Read_writeTheme {
    }
}
```





### StudentObj.kt

```kotlin
package com.example.read_write

data class StudentObj (


    var studentName: String,
    var studentRollNumber: String
)
```