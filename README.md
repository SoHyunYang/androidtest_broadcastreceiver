

```
스터디 진행 : 2016년 7월 2일, 양소현
최초 작성자 : 양소현
최초 작성일 : 2016년 7월 1일
마지막 수정 : 2016년 7월 2일, 양소현
```

# androidtest_broadcastreceiver

**Service**

컴퓨터 프로그램 수행 시 프로세스 내부에 존재하는 수행 경로, 즉 일련의 실행 코드. 프로세스는 단순한 껍데기일 뿐, 실제 작업은 스레드가 담당한다. 프로세스 생성 시 하나의 주 스레드가 생성되어 대부분의 작업을 처리하고 주 스레드가 종료되면 프로세스도 종료된다. 하나의 운영 체계에서 여러 개의 프로세스가 동시에 실행되는 환경이 멀티태스킹이고, 하나의 프로세스 내에서 다수의 스레드가 동시에 수행되는 것이 멀티스레딩이다


**Broadcast receiver**



안드로이드는 UI 스레드라는 것이 존재하는데 UI 와 관련된 작업은 UI스레드만 할 수 있다. 
그래서 안드로이드는 백그라운드 스레드와 UI 스레드의 통신 방법으로 핸들러(Handler)를 제공한다.
핸들러는 메시지를 받거나 보낼수 있고, 메시지에 따라 특정 작업을 실행 할 수 있다. 






##1. broadcast intent 전송하기


![batteryexample1.JPG](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/batteryexample1.JPG?raw=true)
![batteryexample2.JPG](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/batteryexample2.JPG?raw=true)

**Button 생성 & inflation**
```XML
  <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="kr.co.mash_up.wifiexample.MainActivity">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="New Button"
        android:id="@+id/button"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="129dp"
        />
</RelativeLayout>


```
```JAVA
public class MainActivity extends AppCompatActivity {


  
    Button button;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button= (Button)findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();

                intent.setAction("exam.Action");

                sendBroadcast(intent);
            }
        });


    }
```
**CustomReceiver(Broadcast Receiver) 생성**

```JAVA
public class CustomReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context,"CustomReceiver : " +intent.getAction(), Toast.LENGTH_LONG).show();
    }
}

```
**manifests 설정**

```XML
   <receiver
            android:name=".CustomReceiver">
            <intent-filter>
                <action android:name="exam.Action" />
            </intent-filter>
        </receiver>
```


##2. 동적 broadcast receiver

![batteryexample1.JPG](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/batteryexample1.JPG?raw=true)
![batteryexample3.JPG](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/batteryexample3.JPG?raw=true)

**BatteryChangeReceiver(Broadcast Receiver) 생성**



```JAVA
public class BatteryChangeReceiver extends BroadcastReceiver {


        @Override
        public void onReceive(Context context, Intent intent) {
            Toast.makeText(context, "BatteryChangeReceiver : "+intent.getAction(), Toast.LENGTH_LONG).show();


        }

}
```

```JAVA
public class MainActivity extends AppCompatActivity {


    private BatteryChangeReceiver batBR = new BatteryChangeReceiver();

    Button button;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button= (Button)findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();

                intent.setAction("exam.Action");

                sendBroadcast(intent);
            }
        });


    }



        @Override
    protected void onResume() {
        super.onResume();

        IntentFilter filter = new IntentFilter();

        filter.addAction(Intent.ACTION_BATTERY_CHANGED);

        registerReceiver(batBR, filter);




    }

    @Override
    protected void onPause() {
        super.onPause();
        unregisterReceiver(batBR);



    }
}
```



##3. Connectivity manager를 사용하여 네트워크 연결상황  broadcast로 전달
![wifiexample1.JPG](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/wifiexample1.JPG?raw=true)
![wifiexample2.jpg](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/wifiexample2.jpg?raw=true)
![wifiexample3.jpg](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/wifiexample3.jpg?raw=true)
![wifiexample4.jpg](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/wifiexample4.jpg?raw=true)

**NetworkUtil(java file) 생성**

```JAVA
public class NetworkUtil {
    public static int TYPE_WIFI = 1;
    public static int TYPE_MOBILE = 2;
    public static int TYPE_NOT_CONNECTED = 0;


    public static int getConnectivityStatus(Context context) {
        ConnectivityManager cm = (ConnectivityManager) context
                .getSystemService(Context.CONNECTIVITY_SERVICE);

        NetworkInfo activeNetwork = cm.getActiveNetworkInfo();
        if (null != activeNetwork) {
            if(activeNetwork.getType() == ConnectivityManager.TYPE_WIFI)
                return TYPE_WIFI;

            if(activeNetwork.getType() == ConnectivityManager.TYPE_MOBILE)
                return TYPE_MOBILE;
        }
        return TYPE_NOT_CONNECTED;
    }

    public static String getConnectivityStatusString(Context context) {
        int conn = NetworkUtil.getConnectivityStatus(context);
        String status = null;
        if (conn == NetworkUtil.TYPE_WIFI) {
            status = "Wifi enabled";
        } else if (conn == NetworkUtil.TYPE_MOBILE) {
            status = "Mobile data enabled";
        } else if (conn == NetworkUtil.TYPE_NOT_CONNECTED) {
            status = "Not connected to Internet";
        }
        return status;
    }

}
```


**NetworkChangeReceiver(Broadcast Receiver) 생성**



```JAVA
public class NetworkChangeReceiver extends BroadcastReceiver {


    @Override
    public void onReceive(Context context, Intent intent) {
        String status = NetworkUtil.getConnectivityStatusString(context);

        Toast.makeText(context, status, Toast.LENGTH_LONG).show();

    }
}

```

**manifests 설정(permission 추가)**

```XML
 <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="kr.co.mash_up.wifiexample2">

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <receiver
            android:name=".NetworkChangeReceiver">
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                
            </intent-filter>
        </receiver>
    </application>

</manifest>
```

##4. Pending intent
http://micropilot.tistory.com/1994참고
새 스레드는 Thread() 생성자로 만들어서 내부적으로 run()을 구현하던지, Thread(Runnable runnable) 생성자로 만들어서 Runnable 인터페이스를 구현한 객체를 생성하여 전달하던지 둘 중 하나의 방법으로 생성하게 된다. 후자에서 사용하는 것이 Runnable로 스레드의 run() 메서드를 분리한 것이다. 따라서 Runnable 인터페이스는 run() 추상 메서드를 가지고 있으므로 상속받은 클래스는 run()코드를 반드시 구현해야 한다.
앞서 언급한대로 Message가 int나 Object같이 스레드 간 통신할 내용을 담는다면, Runnable은 실행할 run() 메서드와 그 내부에서 실행될 코드를 담는다는 차이점이 있다.

![alarmexample1.JPG](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/alarmexample1.JPG?raw=true)
![alarmexample2.JPG](https://github.com/SoHyunYang/androidtest_broadcastreceiver/blob/master/alarmexample2.JPG?raw=true)

**XML 구성**
```XML
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="kr.co.mash_up.alarmexample.MainActivity" >



    <Chronometer
        android:id="@+id/chronometer"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal" />

    <Button
        android:id="@+id/setnocheck"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Set Alarm 10 sec later - alarmManager.set()" />



</LinearLayout>

```

**MainActivity 구성**
```JAVA
 public class MainActivity extends ActionBarActivity {

    Chronometer chronometer;
    Button btnSetNoCheck;
    final static int RQS_1 = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        chronometer = (Chronometer)findViewById(R.id.chronometer);
        btnSetNoCheck = (Button)findViewById(R.id.setnocheck);
        btnSetNoCheck.setOnClickListener(onClickListener);

    }


    View.OnClickListener onClickListener = new View.OnClickListener(){

        @Override
        public void onClick(View v) {
            chronometer.setBase(SystemClock.elapsedRealtime());
            chronometer.start();

            //10 seconds later
            Calendar cal = Calendar.getInstance();
            cal.add(Calendar.SECOND, 10);

            Intent intent = new Intent(getBaseContext(), AlarmReceiver.class);
            PendingIntent pendingIntent =
                    PendingIntent.getBroadcast(getBaseContext(),
                            RQS_1, intent, PendingIntent.FLAG_ONE_SHOT);
            AlarmManager alarmManager =
                    (AlarmManager)getSystemService(Context.ALARM_SERVICE);

            if(v==btnSetNoCheck){
                alarmManager.set(AlarmManager.RTC_WAKEUP,
                        cal.getTimeInMillis(), pendingIntent);
                Toast.makeText(getBaseContext(),
                        "call alarmManager.set()",
                        Toast.LENGTH_LONG).show();


        }

    };

};
}

```
**AlarmReceiver(Broadcast Receiver) 생성**



```JAVA
public class AlarmReceiver extends BroadcastReceiver {
 
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context,
                "AlarmReceiver.onReceive()",
                Toast.LENGTH_LONG).show();

    }
}

```

**manifests 확인**

```XML
 <receiver
            android:name=".AlarmReceiver"
           ></receiver>

```

##5. goAsync()
http://happyourlife.tistory.com/m/post/121 참고
**정지버튼 생성**
```XML
 
```

**flag를 만들어 정지버튼을 눌렀을 때 Thread 정지시키기**
```JAVA


```


###참고문헌###
Do it! 안드로이드 앱 프로그래밍
실무에 바로 적용하는 안드로이드 프로그래밍

http://koreaparks.tistory.com/128
http://android-er.blogspot.kr/2015/04/example-of-using-alarmmanager-to.html
http://blog.naver.com/ruly2001/70166117812




