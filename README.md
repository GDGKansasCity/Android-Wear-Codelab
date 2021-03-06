Android-Wear-Codelab
====================

This codelab was originally written by Marcus Gabilheri for the GDG OSU Stillwater I/O Extended 2014 event and has been updated for newer versions of the used software by Jay Whitsitt for use by GDG Kansas City.

As said in the original repo, feel free to use or modify this codelab for your own purpose.
If you find a bug leave an issue and I will fix as soon as possible. The code should work but in the tutorial I might have left something.

###### Necessary Software
1. JDK - Java Development Kit
2. [Latest Android Studio installed](https://developer.android.com/sdk/index.html)
3. [Device with the Android Wear app installed](https://play.google.com/store/apps/details?id=com.google.android.wearable.app)
4. Android 4.4W API 20
5. Android Wear Emulator
6. [The Android Wear SDK Docs](https://developer.android.com/training/wearables/apps/layouts.html#UiLibrary) 

###### Installing Necessary SDK Packages
![SDK Manager](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/WearSDK20.png)

###### Creating a new Android Wear Emulator
Android Studio Beta 0.8.2 Already come with some pre-built in emulator definitions that we will use as a base for our own emulators. 
![Device Defaults](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab-Preview/master/screenshots/device_defaults.png)

![Creating the emulator](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab-Preview/master/screenshots/new_device_screen.png)

![Visualizing all emulators](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab-Preview/master/screenshots/all_emulators.png)


###### Starting the Emulator

1. Start the Emulator with the AVD Manager
2. Start the Android Wear App on your device and click in connect.
3. Navigate to android-studio/sdk/platform-tools folder and use the following command: 
```adb -d forward tcp:5601 tcp:5601``` (This command should be used every time you connect your phone to the computer. Or every time you restart the emulator). If for some reason you still can not connect both devices try using ```adb devices``` and ensure that both devices are connected. 

![android-wear-app](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab-Preview/master/screenshots/phone_wear_app.fw.png)

<font style="color:'red';"> Not Connected :( </font>

![Not connected](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab-Preview/master/screenshots/wear.png)

<font color='red'> Connected :) </font>

![Connected](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab-Preview/master/screenshots/wear_connected.png)

### Download the Android project (since this is a Wear codelab, not Android)
[ZIP of base Android project](https://github.com/GDG-KansasCity/Android-Wear-Codelab/archive/start.zip)

### Adding a Android Wear Module

![create a new module](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/creating_wear_module/Screenshot%202014-08-01%2012.38.23.png)

1. Select Android Wear Module
2. Use SDK 20 for all the options. Give it the same name as Your app and name the module wear.
![new module options](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/creating_wear_module/Screenshot%202014-08-01%2012.41.17.png)

3. Use the Same Icon as our app...
![icon](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/creating_wear_module/Screenshot%202014-08-01%2012.31.39.png)

4. Create a new Blank Wear Activity and name it WearActivity (just to keep our sanity when we have multiple windows open)
![wear activity](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/creating_wear_module/Screenshot%202014-08-01%2012.31.48.png)

#### Creating the Notification ICON:

###### 1. Right-Click on the res folder and select new->image_asset
![add-asset](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/add-asset.png)
 
###### 2. Select "Notification Icons" from the dropdown menu and give it a resource name
![add-asset2](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/add-asset2.png)

###### 3. Check that your name and the destination is right and click in Finish. Android studio will generate all the necessary sizes for the notificiation icon as well the necessary folders.
![add-asset3](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/add-asset3.png)

#### Changing our App Icon:

The android wear notifications by default have an icon embed to it. So let's change the icon so our notifications are even more unique! 

* Grab the app icon from from [here](https://github.com/fnk0/Android-Wear-Codelab/blob/master/assets/wear-codelab-icon.png) our use any other image you want. 
* Repeat the same process of adding the notification Icon but this time select Launcher Icons. Ps: Do not change the name this time. If we leave ic_launcher as the name it will override our standard icon.
* Click in Finish and now we are all set!

#### Getting our hands into the coding (fun) part of the project.

Android wear let's us extend the experience of our applications by sending a notification to the wear device.
The Notifications API also let's us add Actions to the notification.
Example: In a TODO app we could send a notification to the user that is time for the task to be done and add actions such as "View Map" , "Complete Task", "Snooze", etc.. We can add a "unlimited" (try to limit your actions to a reasonable number of 3 or 4 maximum, you don't want your user to scroll forever to get to an action...) number of actions. 

######So enough has been said let's get our hands dirty!

In our XML buttons we defined the ```xml android:onClick="sendNotification" ``` method. 
So in our **MainActivity** let's define the sendNotificationMethod:

```java
// Define the method to send the notifications with the same name from the Android onClick from the XML Layout
public void sendNotification(View view) {
    //Now let's add a switch to catch the button that has been clicked
    // We also add a case for each of the buttons.
    switch(view.getId()) {
        case R.id.simpleNotification:
            break;
            
        case R.id.bigNotification:
            break;
            
        case R.id.bigNotificationWithAction:
            break;
            
        case R.id.sendCustomNotification:
            break;
    }
}
```

###### Adding the RadioGroups and Other UI Elements:
Add the following instance variables to your class.
```java
public class MainActivity extends Activity {
    private EditText mCustomTitle, mCustomMessage; // Edit text boxes for the custom notification
    private RadioGroup mCustomIconGroup, showHideIconGroup; // Radiogroups with the Icon and settings for the custom notification
    private int mCustomIcon; // The variable that will hold the ID of the custom icon to show
    private boolean showIcon = false; // boolean that will tell if wear should show the app icon or not
    private String LOG_TAG = "WEAR"; // Our LogCat tag for debugging purposes
    ...
```
###### Instantiating the instance Variables and Retrieving RadioGroup input:
Inside onCreate we instantiate our UI elements and add retrieve information about which element is checked.
```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // Custom Title and Message for custom Notification
        mCustomTitle = (EditText) findViewById(R.id.notificationTitle);
        mCustomMessage = (EditText) findViewById(R.id.notificationMessage);
        
        // RadioGroup for the customIcon
        mCustomIconGroup = (RadioGroup) findViewById(R.id.iconGroup);
        mCustomIconGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            // The name of the ICONS will change based on how you named it....
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                switch (group.getCheckedRadioButtonId()) {
                    case R.id.icon1:
                        mCustomIcon = R.drawable.ic_wear_notification;
                        break;
                    case R.id.icon2:
                        mCustomIcon = R.drawable.ic_notification_2;
                        break;
                    case R.id.icon3:
                        mCustomIcon = R.drawable.ic_notification3;
                        break;
                }
            }
        });
        
        // RadioGroup to determine if App Icons should be shown or not.
        showHideIconGroup = (RadioGroup) findViewById(R.id.hideIconGroup);
        showHideIconGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                switch (group.getCheckedRadioButtonId()) {
                    case R.id.showIcon:
                        showIcon = true;
                        break;
                    case R.id.hideIcon:
                        showIcon = false;
                        break;
                }
            }
        });
    }
```
###### Let's send some notifications to our users!!
Let's visit our ```java sendNotification()``` method again and some common code for our notifications. 
We will add this before the switch so all the variables can be shared between the notifications.
```java
// Define the method to send the notifications with the same name from the Android onClick from the XML Layout
public void sendNotification(View view) {

    // Common elements for all our notifications
    int notificationId = 001; // id- An identifier for this notification unique within your application.
    String eventTitle = "Sample Notification"; // Title for the notificaiton
    String eventText = "Text for the notification."; // Text for the notificaiton
    String intentExtra = "This is an extra String!"; // Extra String to be passed to a intent
    // A large String to be used by the BigStyle
    String eventDescription = "This is supposed to  be a content that will not fit the normal content screen"
            + " usually a bigger text, by example a long text message or email."; 

    // Build intent for notification content - This will take us to our MainActivity
    Intent viewIntent = new Intent(this, MainActivity.class);
    PendingIntent viewPendingIntent = PendingIntent.getActivity(this, 0, viewIntent, 0);

    // Specify the 'big view' content to display the long
    // event description that may not fit the normal content text.
    NotificationCompat.BigTextStyle bigStyle = new NotificationCompat.BigTextStyle();
    
    // We instantiate as null because they will be changing based on which button is pressed
    NotificationCompat.Builder mBuilder = null; 
    Notification mNotification = null;

    // Get an instance of the NotificationManager service
    NotificationManagerCompat notificationManager = NotificationManagerCompat.from(this);

    switch(view.getId()) {
        case R.id.simpleNotification:
            break;
            
        case R.id.bigNotification:
            break;
            
        case R.id.bigNotificationWithAction:
            break;
            
        case R.id.sendCustomNotification:
            break;
    }
}

```

#### Building the Notifications: 
Now it's time to build the notification we want to display based on the user input.
You can use anything you want to build and send notifications. Can be game scores, alarm clocks, etc.. 
Let's keep it simple for this codelab and get input directly from the user.

Let's modify our switch cases for each one of our base cases.

###### 1. Simple Notification
This is the simplest form of a notification with 4 elements.
1. A drawable for the notification. We use one of the drawables created by the asset manager.
2. A title for the notification
3. A message to be displayed
4. The intent that should be opened by this notification.
```java
case R.id.simpleNotification:
    mBuilder = new NotificationCompat.Builder(this) // Instantiate the builder with our context.
            .setSmallIcon(R.drawable.ic_wear_notification) // set the icon
            .setContentTitle(eventTitle) // set the title
            .setContentText(eventText) // set the text
            .setAutoCancel(true)  // This flag makes the notification disappear when the user clicks on it!
            .setContentIntent(viewPendingIntent); // and finally the intent to be used
    break;
```

Now after the switch we add the following code to display or notification:
```java
// This code goes after the switch because is common to all our notifications.
// Build the notification and issues it with notification manager.
notificationManager.notify(notificationId, mBuilder.build());
Log.d(LOG_TAG, "Normal Notification");
```

* Now let's try it out!! Run the App and click on the first button.
The notification should be displayed on your phone like this:

![firstNotification](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/sample-notification.png)

* The notifications should also appear on the device like this:

![wearNotification](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/simple_not.png)

* The intent will be displayed with a Open Button:

![wearNotification2](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/simple_not2.png)

###### 2. Big View Notification
Most of the contents of the big notification are the same. 
There are a few different things to pay attention here.

1. The largeIcon is displayed behind the notification as a background. The difference to the small icon is that we will be using BitmapFactory.decodeResource() decode a png file.
2. The setContentTitle and setContentText will be overriden by the bigStyle.bigText and bigStyle.setBigContentTitle.
```java
case R.id.bigNotification:
    bigStyle.bigText(eventDescription); // bigText will override setContentText
    bigStyle.setBigContentTitle("Override Title"); // bigContentTitle Override the contentTitle
    mBuilder = new NotificationCompat.Builder(this)
        .setSmallIcon(R.drawable.ic_wear_notification)
        .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.drawable.face))
        .setContentTitle(eventTitle) // This is unnecessary for the big notification if you use bigText
        .setContentText(eventText) // Unnecessary if setBigContentTitle is Overriden
        .setContentIntent(viewPendingIntent)
        .setAutoCancel(true)
        .setStyle(bigStyle);
    break;
```

[Download face.png here](https://github.com/GDG-KansasCity/Android-Wear-Codelab/blob/master/app/src/main/res/drawable-xxhdpi/face.png?raw=true)

The notification in the wear device should look somewhat like this:

![bigNot1](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/big-not.png)

Since the text is set as a big text it can now expand itself to allow the user to scroll and read the text inside.

![bigNot2](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/big-not3.png)

Note: The developer documentation recommends landscape images at least 600px wide because Wear will automatically add a paralaxing effect when scrolling notification actions. 
Another note: You may have noticed that "Override Title" is not the title of our Wear notification. It is however the title of the notification on your device when its expanded. I'm unsure why this is happening. My guess right now is that something changed since this codelab was originally written.

###### 3. BigView notification with an Action button:
For this Action we will create another activity. Our goal is to start another activity from the intent and set a message + show the picture that is set as the largeIcon.

1. Create the new Activity:
  * Right click any folder inside your app package -> New -> Activity -> Blank Activity
  * Give it a name : SecondActivity
  * Set up the XML layout elements:
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:background="#34495e"
    android:orientation="vertical"
    tools:context="myawesomepackagename.codelabandroidwear.SecondActivity">

    <TextView
        android:id="@+id/extraMessage"
        android:text="@string/hello_world"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <ImageView
        android:id="@+id/extraPhoto"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        />
</LinearLayout>
```
2. Instantiate the XML elements in your activity.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_second);
    TextView mTextView = (TextView) findViewById(R.id.extraMessage); // TextView to retrieve the message
    ImageView mImageView = (ImageView) findViewById(R.id.extraPhoto); // ImageView to retrieve the picture
}
```
3. Get the intent information and set the data for the elements
```java
// Get the intent information
Intent extraIntent = getIntent();

// Get the intent information based on the names passed by your notification "message" and 
mTextView.setText(extraIntent.getStringExtra("message")); // Retrieve the text and set it to our TextView
mImageView.setImageResource(extraIntent.getIntExtra("photo", 0)); // The zero is a default value in case the intent extra is empty.
```
4. Now that our Second Activity is set and ready to receive some data we proceed to build our notification.
```java
case R.id.bigNotificationWithAction:
    // Create the new intent that is gonna receive the information from our action.
    Intent photoIntent = new Intent(this, SecondActivity.class); // Intent pointing to our second activity
    photoIntent.putExtra("message", intentExtra); // Set the extra message that will open in the next activity
    photoIntent.putExtra("photo", R.drawable.face); // Send the photo to the next activity
    
    PendingIntent photoPending = PendingIntent.getActivity(this, 0, photoIntent, 0); // set a new pending intent
    String title = "Mr. Important";
    bigStyle.setBigContentTitle(title); // title for the Big Text
    bigStyle.bigText("Check out this picture!! :D"); // Message in the Big Text
    mBuilder = new NotificationCompat.Builder(this)
            .setContentTitle(title)
            .setSmallIcon(R.drawable.ic_wear_notification) // Small icon for our notification
            .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.drawable.face)) // The PNG picture
            .setContentIntent(viewPendingIntent) // This will be the default OPEN button.
            .addAction(R.drawable.ic_photo, "See Photo", photoPending) // This is our extra action. With an Extra Icon and pointing to the other PendingIntent
            .setAutoCancel(true)
            .setStyle(bigStyle); // Add the bigStyle
    break;
```
[Download ic_photo.png here](https://github.com/GDG-KansasCity/Android-Wear-Codelab/blob/master/app/src/main/res/drawable-xxhdpi/ic_photo.png?raw=true)

The Action button with our Custom Icon to view the photo:

![bigNotAction](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/big-not5.png)

And this is how the same notification looks in the cellphone

![mr-flowers](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/mr-flowers.png)

###### Creating our custom notification.
Our custom notification will let the user set a title, a message, select the Icon to display in the notification and will give an option if the user wants to show or not the App Icon.
```java
case R.id.sendCustomNotification:
    mBuilder = new NotificationCompat.Builder(this)
                        .setSmallIcon(mCustomIcon)
                        .setContentTitle(mCustomTitle.getText().toString())
                        .setContentText(mCustomMessage.getText().toString())
                        .setAutoCancel(true)
                        .setContentIntent(viewPendingIntent);

                // This is an example of the NEW WearableNotification SDK.
                // The WearableNotification has special functionality for wearable devices
                // By example the setHintHideIcon hides the APP ICON from the notification.
                // This code is now Up to date thanks to Romin Irani!! Thanks! 
                NotificationCompat.WearableExtender wearableExtender = new NotificationCompat.WearableExtender(mBuilder.build());
                wearableExtender.setHintHideIcon(!showIcon);
                wearableExtender.extend(mBuilder);
                break;
```

Give it a try now and play with the different notifications!!!

#### Bonus

#### Updated Version of the Tutorial. Creating a Simple Grid Page Adapter:

##### Replacing the Watch Stub faces:
In this tutorial we are not going to use the Watch Stub faces. 
We open the file wear_activity and replace the code that is already there with:
```xml
    <?xml version="1.0" encoding="utf-8"?>
    <android.support.wearable.view.GridViewPager
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/gridPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:keepScreenOn="true"
        tools:context=".WearActivity"
     />
```
This code will place a empty GridViewPager adapter. This Grid View Pager will be them populated from the Java Code using a FragmentGridPagerAdapter

##### Creating the GridPage -> This will be a very simple Class that will hold the data for each Page of our GridPagerAdapter

GridPage.java
```java
 public class GridPage  {
     private String mTitle;
     private String mText;
     private int mIcon;
     private int mBackground;
     /**
      * Constructor for the GridPage
      * @param mTitle
      *          Title for the card
      * @param mText
      *          Text for the card
      * @param mIcon
      *          Icon that will be on the right of the title
      * @param mBackground
      *          The Background image to be used by the fragment. The card will overlay on top of the background.
      */
     public GridPage(String mTitle, String mText, int mIcon, int mBackground) {
         this.mTitle = mTitle;
         this.mText = mText;
         this.mIcon = mIcon;
         this.mBackground = mBackground;
     }
     public String getTitle() {
         return mTitle;
     }
     public String getText() {
         return mText;
     }
     public int getIcon() {
         return mIcon;
     }
     public int getBackground() {
         return mBackground;
     }
 }
```
As you can see this is just a very simple class that will hold the Data for the Page and return when we need it.

##### Creating the GridRow -> Grid Row is a very simple class also that will hold a ArrayList of GridPages
The GridRow will also have handy methods for:

1. Adding new Pages

2. Getting the size of the row

3. Setting the pages

4. Getting the desired page

GridRow.java
```java
    public class GridRow  {
        ArrayList<GridPage> mPages = new ArrayList<GridPage>();
        public GridRow() {}
        public GridRow(ArrayList<GridPage> mPages) {
            this.mPages = mPages;
        }
        public GridPage getPage(int index) {
            return mPages.get(index);
        }
        public void addPage(GridPage mPage) {
            mPages.add(mPage);
        }
        public int getSize() {
            return mPages.size();
        }
        public ArrayList<GridPage> getPagesArray() {
            return mPages;
        }
        public void setPages(ArrayList<GridPage> mPages) {
            this.mPages = mPages;
        }
    }
```

###### Creating the GridPageAdapter -> 

There's a few things to notice here.

1. We override the getBackground() to set the background for each of the pages. 

2. We override the getRowCount() so the adapter know where to look for its size

3. getColumnCount() Will return the number of columns in a specific row. also used internally by the adapter.

4. The getFragment() is the one that will Create a new CardFragment with the information from our GridPage

GridPagerAdapter.java
```java
public class GridPagerAdapter extends FragmentGridPagerAdapter {

    private Context mContext;
    private ArrayList<GridRow> mRows;

    public GridPagerAdapter(Context mContext, FragmentManager fm) {
        super(fm);
        this.mContext = mContext;
        initAdapter();
    }
    /**
     * This method is used for demonstration only. In a real app the data and the adapters would
     * probably come from somewhere else.
     */
    private void initAdapter() {
        mRows = new ArrayList<GridRow>();
        GridRow row1 = new GridRow();
        row1.addPage(new GridPage("Pink Flower", "Wow! Such flower! Much smells!", R.drawable.ic_info, R.drawable.flower1));
        row1.addPage(new GridPage("Flower?", "Much Pretty! Such nature!", R.drawable.ic_info, R.drawable.flower2));
        row1.addPage(new GridPage("Red Flower!", "Yes! I know nothing about flowers!", R.drawable.ic_info, R.drawable.flower3));
        GridRow row2 = new GridRow();
        row2.addPage(new GridPage("Flowers!", "This is row 2!!!", R.drawable.ic_info, R.drawable.flower4));
        row2.addPage(new GridPage("Row 2 Col 2..", "ZzzzZzZZzzZZZz", R.drawable.ic_info, R.drawable.flower1));
        GridRow row3 = new GridRow();
        row3.addPage(new GridPage("THIS IS SPA... ops Row 3!", "Yes rows can have different sizes.", R.drawable.ic_info, R.drawable.flower3));
        mRows.add(row1);
        mRows.add(row2);
        mRows.add(row3);
    }

    @Override
    public Fragment getFragment(int row, int col) {
        GridPage page = mRows.get(row).getPage(col);
        CardFragment cardFragment = CardFragment.create(page.getTitle(), page.getText(), page.getIcon());
        return cardFragment;
    }

    /**
     * This method is Overridden so we can set a Custom Background for each element.
     * @param row
     * @param column
     * @return
     */
    @Override
    public ImageReference getBackground(int row, int column) {
        GridPage page = mRows.get(row).getPage(column);
        return ImageReference.forDrawable(page.getBackground());
    }

    @Override
    public int getRowCount() {
        return mRows.size();
    }

    @Override
    public int getColumnCount(int row) {
        return mRows.get(row).getSize();
    }
}
```

[Download flower1.png here](https://github.com/fnk0/Android-Wear-Codelab/blob/master/wear/src/main/res/drawable/flower1.jpeg?raw=true)
[Download flower2.png here](https://github.com/fnk0/Android-Wear-Codelab/blob/master/wear/src/main/res/drawable/flower2.jpg?raw=true)
[Download flower3.png here](https://github.com/fnk0/Android-Wear-Codelab/blob/master/wear/src/main/res/drawable/flower3.jpg?raw=true)
[Download flower4.png here](https://github.com/fnk0/Android-Wear-Codelab/blob/master/wear/src/main/res/drawable/flower4.jpeg?raw=true)
[Download ic_info.png here](https://github.com/fnk0/Android-Wear-Codelab/blob/master/wear/src/main/res/drawable-xxhdpi/ic_info.png?raw=true)

###### Adding the GridPagerAdapter to our Activity

Now it's time to finally connect our adapter to the activity. Inside WearActivity place this following snippet.

```java
private GridViewPager mPager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_wear);
        mPager = (GridViewPager) findViewById(R.id.gridPager);
        mPager.setAdapter(new GridPagerAdapter(this, getFragmentManager()));
    }
```

And.. if everything went OK you should now see in your watch a card that looks kinda like this:
Go ahead and try swiping up, down, left and right to see what happens

![wear page1](https://raw.githubusercontent.com/fnk0/Android-Wear-Codelab/master/screenshots/wear_screen1.png)

###### Thanks for doing this tutorial. I hope you enjoy it!!
Any questions, suggestions email me! marcus@gabilheri.com
