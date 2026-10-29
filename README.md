# Workshop 01: Addition of Two Numbers

## Aim
To create an Android application that takes two numbers as input and displays their sum in a text box, using Android Studio.

## Equipment Required
- Android Studio (Arctic Fox or higher)
- Android Emulator or a physical Android device

## Algorithm
1. Open Android Studio and click **File -> New -> New Project**.
2. Select **Empty Views Activity**, enter the application name, and choose **Java** as the language.
3. Click **Finish** to create the project.
4. Design the layout in `activity_main.xml` with two EditText fields, a Button, and a result text box.
5. In `MainActivity.java`, initialize the views using `findViewById()`.
6. On button click, read both values and check that they are not empty.
7. Convert the values to numbers, calculate the sum, and display it in the result text box.
8. Save and run the application.

## Project Structure
```
app/
 ├── manifests/
 │    └── AndroidManifest.xml
 ├── java/com.example.additionapp/
 │    └── MainActivity.java
 └── res/
      ├── layout/activity_main.xml
      └── values/strings.xml
```

## Code 
# activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F5F5F5"
    android:padding="24dp"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textViewTitle"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:gravity="center"
        android:text="@string/title_addition"
        android:textSize="24sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/editTextNumber1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:background="@android:drawable/edit_text"
        android:hint="@string/hint_first_number"
        android:importantForAutofill="no"
        android:inputType="numberDecimal|numberSigned"
        android:padding="16dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textViewTitle" />

    <EditText
        android:id="@+id/editTextNumber2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:background="@android:drawable/edit_text"
        android:hint="@string/hint_second_number"
        android:importantForAutofill="no"
        android:inputType="numberDecimal|numberSigned"
        android:padding="16dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/editTextNumber1" />

    <Button
        android:id="@+id/buttonAdd"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:padding="16dp"
        android:text="@string/button_add"
        android:textSize="18sp"
        app:cornerRadius="12dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/editTextNumber2" />

    <EditText
        android:id="@+id/editTextResult"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:background="@android:drawable/edit_text"
        android:cursorVisible="false"
        android:focusable="false"
        android:hint="@string/hint_result"
        android:importantForAutofill="no"
        android:inputType="none"
        android:padding="16dp"
        android:textColor="@android:color/black"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/buttonAdd" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

# MainActivity.java
```
package com.example.addition_application;

import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    private EditText editTextNumber1;
    private EditText editTextNumber2;
    private Button buttonAdd;
    private EditText editTextResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        
        // Handle window insets for edge-to-edge display
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        // Initialize Views
        editTextNumber1 = findViewById(R.id.editTextNumber1);
        editTextNumber2 = findViewById(R.id.editTextNumber2);
        buttonAdd = findViewById(R.id.buttonAdd);
        editTextResult = findViewById(R.id.editTextResult);

        // Set click listener for the Add button
        buttonAdd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                calculateSum();
            }
        });
    }

    private void calculateSum() {
        String num1Str = editTextNumber1.getText().toString().trim();
        String num2Str = editTextNumber2.getText().toString().trim();

        // Validate if either field is empty
        if (TextUtils.isEmpty(num1Str) || TextUtils.isEmpty(num2Str)) {
            Toast.makeText(MainActivity.this, getString(R.string.error_empty_fields), Toast.LENGTH_SHORT).show();
            return;
        }

        try {
            // Convert strings to double
            double num1 = Double.parseDouble(num1Str);
            double num2 = Double.parseDouble(num2Str);

            // Calculate the sum
            double sum = num1 + num2;

            // Format result: if it's a whole number, display without decimal part
            String resultStr;
            if (sum == (long) sum) {
                resultStr = String.valueOf((long) sum);
            } else {
                resultStr = String.valueOf(sum);
            }

            // Display the result
            editTextResult.setText(resultStr);

        } catch (NumberFormatException e) {
            // Handle invalid number format exceptions safely
            Toast.makeText(MainActivity.this, getString(R.string.error_invalid_number), Toast.LENGTH_SHORT).show();
        }
    }
}
```

## Sample Test Case

| Input 1 | Input 2 | Output |
|---|---|---|
| 15 | 25 | 40 |
| 8.5 | 1.5 | 10 |
| -4 | 10 | 6 |



## Output
<img width="1531" height="854" alt="image" src="https://github.com/user-attachments/assets/1931d032-f36d-41cc-bb77-9566f6c86a27" />


<img width="1535" height="863" alt="image" src="https://github.com/user-attachments/assets/1eb14a68-59a6-4175-a533-811dc8a1e9f9" />


## Result
Thus, a simple Android application to add two numbers and display the result in a text box was developed and executed successfully using Android Studio.
