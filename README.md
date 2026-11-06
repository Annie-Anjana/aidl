# Ex.No:2 Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.The client app calls the service and changes the button's colour within the Main activity.

## DEVELOPED BY: ANNIE ANJANA A
## REGISTER NUMBER: 212224040025

## AIM:

To Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.
The client app calls the service and changes the button's colour within the Main activity using AIDL interface in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Griaffe )

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as CSAIDL and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file(client/server).

Step 7: Save and run the application.

## PROGRAM:



## SERVER PROGRAM:


### Mainactivity.java
```
package com.example.ex2;

import android.os.Bundle;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }
}
```
### AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Ex2">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <service
            android:name=".ColorService"
            android:exported="true">
            <intent-filter>
                <action android:name="com.example.ex2.IColorService"/>
            </intent-filter>
        </service>
    </application>

</manifest>
```

## CLIENT PROGRAM:

### Activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <Button
        android:id="@+id/btnChangeColor"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:text="Change Color"
        android:textSize="22sp"/>

</LinearLayout>
```

### MainActivity.java

```
package com.example.client;

import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.RemoteException;
import android.widget.Button;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.example.ex2.IColorService;

public class MainActivity extends AppCompatActivity {

    IColorService colorService;
    boolean isBound = false;

    Button btnChangeColor;

    ServiceConnection connection = new ServiceConnection() {

        @Override
        public void onServiceConnected(
                ComponentName name, IBinder service) {

            colorService =
                    IColorService.Stub.asInterface(service);

            isBound = true;

            Toast.makeText(MainActivity.this,
                    "Connected", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            isBound = false;
            colorService = null;
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnChangeColor = findViewById(R.id.btnChangeColor);

        btnChangeColor.setOnClickListener(v -> {

            if (isBound && colorService != null) {
                try {
                    int color = colorService.getRandomColor();

                    btnChangeColor.setBackgroundColor(color);

                } catch (RemoteException e) {
                    e.printStackTrace();
                }
            } else {
                Toast.makeText(this, "Not Connected",
                        Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();

        Intent intent = new Intent(
                "com.example.ex2.IColorService");

        intent.setPackage("com.example.ex2");

        bindService(intent, connection,
                Context.BIND_AUTO_CREATE);
    }

    @Override
    protected void onStop() {
        super.onStop();

        if (isBound) {
            unbindService(connection);
            isBound = false;
        }
    }
}
```


## OUTPUT


server side:

<img width="1707" height="1057" alt="Screenshot 2026-11-05 222518" src="https://github.com/user-attachments/assets/0d1ca76d-c282-4eef-b8e9-f057c7649d75" />

<img width="1697" height="1060" alt="Screenshot 2026-11-05 222533" src="https://github.com/user-attachments/assets/1ea4ac49-67b7-4216-9d98-21875498761e" />



client side:

<img width="1722" height="1063" alt="Screenshot 2026-11-05 223915" src="https://github.com/user-attachments/assets/02aea069-6a51-415f-92a1-ee4594c8fdfa" />

<img width="1622" height="1040" alt="Screenshot 2026-11-05 223926" src="https://github.com/user-attachments/assets/15d766f7-b238-429f-9e5c-c712dafadce1" />


## RESULT
Thus a Simple Android Application to create a AIDL interface and communicate the process between client and server using AIDL interface in Android Studio is developed and executed successfully.
