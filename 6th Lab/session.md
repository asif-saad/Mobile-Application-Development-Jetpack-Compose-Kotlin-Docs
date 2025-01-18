### MainActivity.kt

```kotlin
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
```





### StudentObj.kt

```kotlin
package com.example.read_write

data class StudentObj (


    var studentName: String,
    var studentRollNumber: String
)
```