For launcher icons
====

**android>app>src>main>res>**  
This is where the launcher files according to different densities of devices are stored.  

** There are ways to create these:

* App icon generators like:
    * https://appicon.co

* [PREFFERED] Use the package icons_launcher from pub.dev
    * Add config (at the root) to pubspec.yaml or create a separate icons_launcher.yaml:
    
    <pre>icons_launcher:
  image_path: "assets/ic_logo_border.png"
  platforms:
    android:
      enable: true
      image_path: "assets/ic_logo_border.png"
      # adaptive_background_color: '#ffffff'
      adaptive_background_image: 'assets/ic_background.png'
      adaptive_foreground_image: 'assets/ic_foreground.png'
      adaptive_round_image: 'assets/ic_logo_round.png'      
    ios:
      enable: true
      image_path: "assets/ic_logo_border.png"
    </pre>

    * flutter pub get
    * flutter pub run icons_launcher:create

For launch screen 
====

 It is shown before splash screen (that we created).

**For IOS:**

*  Open Xcode and go to Runner>Runner>Assets>LaunchImage
    * Create three images and copy them in these sizes:  
        * 640x1138
        * 750x1334
        * 1224x2208
    * While doing this also change the **Background color** to the color of your image and **Content Mode** to **Scale to fill** Otherwise it is show between two white borders in vertical ends.

**For Android:**    
* Go to **android>app>src>main>res>values**
* Create a file **colors.xml** like:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="bgColor">#000000</color>
</resources>
```
* Change the color value in the drawable/launch_background.xml and drawable-v21/launch_background.xml like:
```
<item android:drawable="@color/bgColor" />
```
* Uncomment the **You can insert your own image assets here** section
* Copy the files under the **android>app>src>main>res>midmap-blabla** folders
* **android>app>src>main>AndroidManifest.xml**

```
<meta-data 
 android:name="io.flutter.embedding.android.SplashScreenDrawable"
 android:resource="@drawable/launch_background">
```