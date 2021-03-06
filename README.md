# Final Year Project (INTI Digital Signage Mobile Application for INTI International College @ Subang Jaya)
This is my Final Year Project that I have completed in July of 2017. 
The app is a native app to both Android and iOS platforms and is designed for our college, INTI International College Subang.

Unfortunately, this FYP and many of other group projects were hosted on Hostinger by my friend and his account got deleted(probably due to no continuous subscription fees), which results me unable to get some screenshots for the functions mentioned below.

The hightlights for me in this project are...

## `Contents`
* [To create login API for users and guests on Android.](#login-api)            --Solo--
* [To create a backend push notification API.](#push-notification-api)          --Solo--
* [To implement interaction with Estimote beacons and registration for Firebase API.](#estimote-beacons)   --Solo--
* [To assist another back-end developer on map editor/constructing database.](#map-editor)


****************************************************************************************************************************************
****************************************************************************************************************************************
## `Login API`
This is a login screen created for my Final Year Project(Android App) that interacts with online database.
This login screen provide user login (require student login credentials) and guest login (generate session token).

#### Permissions     
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
*INTERNET        : To call API which then returns 0 or 1 based on the parameters(Username and Password) passed to API
READ_PHONE_STATE : To generate UUID based on IMEI which is then insert to online database in order to grant access to Guests*    
    
#### User Login     
Login with student ID and password, which shares the same login credentials with the campus website.(to be done by INTI, as for now we are logging in from online database). The app will student ID and password to API, the API then compare the parameters with the database and return 0 or 1 based on result.

#### Guest Login
Provide access to non INTI students (app will generate a token and insert to database in order to grant access to guests)

#### Screenshot     
![image](https://github.com/shinjiat/Android-Login/blob/master/AndroidLogin/ScreenShot_20170829203644.png)

There are only 2 PHP files created [here](https://github.com/shinjiat/INTI-DIGITAL-SIGNAGE/tree/master/source%20codes/login) for this API, [insert.php](https://github.com/shinjiat/INTI-DIGITAL-SIGNAGE/blob/master/source%20codes/login/insert.php) inserts tokens to grant access to Guests and [login.php](https://github.com/shinjiat/INTI-DIGITAL-SIGNAGE/blob/master/source%20codes/login/login.php) to compare user's parameters to the database to return either 0 or 1.

[Back to top](#contents)
****************************************************************************************************************************************
****************************************************************************************************************************************
## `Push Notification API`
This API was an extra feature that I implemented for our clients.
For this to work, when a student open this APP within INTI College, if one of the beacons in the College detected the student's phone, the APP will register a unique token and insert the token into database via [insert.php](https://github.com/shinjiat/INTI-DIGITAL-SIGNAGE/blob/master/source%20codes/notification/insert.php).

However, the token will change if the user reinstall the APP. So to reduce the number of "orphan" tokens, I insert the token together with the device's UUID(based on IMEI if I remember correctly), in such a way that if the device's UUID already exists in the database, then update the token. Else, insert a new token.

To not annoy the student too much, the token is deleted when the APP is closed via [delete.php](https://github.com/shinjiat/INTI-DIGITAL-SIGNAGE/blob/master/source%20codes/notification/delete.php).

So this basically means that the push notifications will only work, only when students or guests are within the College area.

#### Screenshot
![image](https://github.com/shinjiat/INTI-DIGITAL-SIGNAGE/blob/master/screenshots/ScreenShot_20171027041955.png?raw=true)

[Back to top](#contents)
****************************************************************************************************************************************
****************************************************************************************************************************************
## `Estimote beacons`
Please refer [here](https://drive.google.com/file/d/0Bx9LRWgMTzbZaHJvVHlrV1g2VmM/view?usp=sharing) for more screenshots and a detailed report made by me for our FYP.

#### Permissions
    <uses-permission android:name="android.permission.BLUETOOTH"/>
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
    <uses-permission android:name="android.permission.BLUETOOTH_PRIVILEGED"/>

#### To monitor beacons in range
```
    Override
    public void onServiceReady() {
    beaconManager.startMonitoring(new BeaconRegion(
    "monitored region",
    UUID.fromString("B9407F30-F5F8-466E-AFF9-25556B57FE6D"), 22504, 48827));
```
The above function will be able to pick up all the beacons that have the same UUID ("B9407F30-F5F8-466E-AFF9-25556B57FE6D"), in order for multiple beacons to work.

We will change the major and minor to, ("B9407F30-F5F8-466E-AFF9-25556B57FE6D"), null, null).
So that it will pick up whatever the beacon’s Major and Minor are.

#### Display nearest beacon
```
    private List<String> placesNearBeacon(Beacon beacon) {
    String beaconKey = String.format("%d:%d" , beacon.getMajor(), beacon.getMinor());
    if(PLACES_BY_BEACONS.containsKey(beaconKey)) {
    return PLACES_BY_BEACONS.get(beaconKey);
    }
    return Collections.emptyList();
    }
```
This function will take a beacon object, then it will return a list of beacons, sorted with distance to the device.

To access the nearest beacon, you just need the below code. The list is compute in another function where it will detect and store the beacons in a list and sort them with distance. Where list.get(0) is the first index and also the closest one.

`Beacon nearestBeacon = list.get(0);`
    
   
[Back to top](#contents)
****************************************************************************************************************************************
****************************************************************************************************************************************

## `Map Editor`
Please refer [here](https://drive.google.com/file/d/0Bx9LRWgMTzbZaHJvVHlrV1g2VmM/view?usp=sharing) for more screenshots and a detailed report made by me for our FYP.

##### Updated source codes were all deleted on my [friend](https://github.com/buyback)'s hostinger account, I'm unable to get any screenshot from the original online hosted Website unfortunately.
##### I am unable to replicate how the website would look like on localhost, due to missing too many updated files, table and hard-coded data on hostinger's database.

[Back to top](#contents)
****************************************************************************************************************************************
****************************************************************************************************************************************
