# _Splash Screen | Android 12(API 31)_

Android 12 introduced us new [SplashScreen](https://developer.android.com/reference/android/window/SplashScreen) API, which enables a new app launch animation.

<p align="center">
  <img width="300" height="500" src="https://developer.android.com/about/versions/12/images/splash-screen-gmail-example.gif">
</p>
<p align="center">
*SplashScreen Example for Android Developers*
</p>

## How the splash screen works
When a user launches an app while the app's process is not running (a [cold start](https://developer.android.com/topic/performance/vitals/launch-time#cold)) or the Activity has not been created (a [warm start](https://developer.android.com/topic/performance/vitals/launch-time#warm)), the following events occur. (The splash screen is never shown during a [hot start](https://developer.android.com/topic/performance/vitals/launch-time#hot).)

1. The system shows the splash screen using themes and any animations that you've defined.

2. When the app is ready, the splash screen is dismissed and the app is displayed.

## Implementing the Splash Screen API
The new [SplashScreen](https://developer.android.com/reference/android/window/SplashScreen) API will only work if the application is compiled in Android 12 (API 31). Update the ``` compileSdkVersion ``` to ``` 31 ``` in the ``` build.gradle ``` file.
>  [Offical Documentation](https://developer.android.com/about/versions/12/features/splash-screen#customize-animation)

```
android {
    compileSdkVersion "android-S" // while Android 12 is in preview
    // compileSdkVersion 31 - when the Android 12 is stable
}
```
We add the dependency for this new library:
```
implementation 'androidx.core:core-splashscreen:1.0.0-alpha01'
```
Sync the ``` build.gradle ``` file, we need to add splash screen theme code in ``` values/themes.xml ``` with the new parameters.

Here we create theme, in our example ``` Theme.App.Starting ```. Then we add the following parameters/customizing the splash screen.

- The parent of this theme needs to be ``` Theme.SplashScreen ```
- ``` windowSplashScreenBackground ``` to set the background color (specific only single color)
- ``` windowSplashScreenAnimatedIcon ``` to set either a drawable or an animated drawable.
- ``` windowSplashScreenAnimationDuration ``` to set the amount of time the splash screen appears before it is dismissed. The maximum time is 1,000 ms.
- ``` windowSplashScreenIconBackgroundColor ``` to set a background behind the splash screen icon (specific to api ``` above 31 ```).
- ``` windowSplashScreenBrandingImage ``` to set an image to be shown at the bottom of the splash screen (specific to api ``` above 31 ```).
- ``` postSplashScreenTheme ``` set the theme that will be used once the Splash screen is no longer visible.

```js
<style name="Theme.App.Starting" parent="Theme.SplashScreen">
    <item name="windowSplashScreenBackground">@color/purple_700</item>

    <item name="windowSplashScreenAnimatedIcon">@drawable/ic_splash</item>
    <item name="windowSplashScreenAnimationDuration">500</item>

    <item name="android:windowSplashScreenIconBackgroundColor" tools:targetApi="31">@color/teal_700</item>
    <item name="android:windowSplashScreenBrandingImage" tools:targetApi="31">@drawable/ic_branding</item>

    <item name="postSplashScreenTheme">@style/Theme.SplashScreens</item>
</style>
```

Now, we can set theme to ``` Application ``` or ``` Activity ```.
```js
<application
        ...
        android:theme="@style/Theme.App.Starting">
        ...
</application>
```

<p align="center">
** or **
</p>

```js
<activity
    android:name=".MainActivity"
    android:exported="true"
    android:theme="@style/Theme.App.Starting">
    ...
</activity>
```

After setting up the themes, a very important step is to call the splash screen install before the setContentView in the Activity.
```js
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        installSplashScreen()
        setContentView(R.layout.activity_main)
    }
}
```

If above step is not followed, the application will fail with:
```
java.lang.IllegalStateException: You need to use a Theme.AppCompat theme (or descendant) with this activity.
```

## Keep the splash screen on-screen for longer periods
When the app draws the first frame, the splashscreen will disappear. If we need to load a data, we can use ViewTreeObserver.OnPreDrawListener to suspend the app to draw its first frame.
```js
    var isReady = false

    Handler(mainLooper).postDelayed({
        isReady = true
    }, 2000)

    // Set up an OnPreDrawListener to the root view.
    val content: View = findViewById(android.R.id.content)
    content.viewTreeObserver.addOnPreDrawListener(
        object : ViewTreeObserver.OnPreDrawListener {
            override fun onPreDraw(): Boolean {
                // Check if the initial data is ready.
                return if (isReady) {
                    // The content is ready; start drawing.
                    content.viewTreeObserver.removeOnPreDrawListener(this)
                    true
                } else {
                    // The content is not ready; suspend.
                    false
                }
            }
        }
    )
```

