# _Splash Screen | Android 12(API 31)_

Android 12 introduced us new [SplashScreen](https://developer.android.com/reference/android/window/SplashScreen) API, which enables a new app launch animation.

<p align="center">
  <img width="300" height="500" src="https://developer.android.com/about/versions/12/images/splash-screen-gmail-example.gif">

  *SplashScreen Example for Android Developers*
</p>

## How the splash screen works
When a user launches an app while the app's process is not running (a [cold start](https://developer.android.com/topic/performance/vitals/launch-time#cold)) or the Activity has not been created (a [warm start](https://developer.android.com/topic/performance/vitals/launch-time#warm)), the following events occur. (The splash screen is never shown during a [hot start](https://developer.android.com/topic/performance/vitals/launch-time#hot).)

1. The system shows the splash screen using themes and any animations that you've defined.

2. When the app is ready, the splash screen is dismissed and the app is displayed.

## Implementing the Splash Screen API
The new [SplashScreen](https://developer.android.com/reference/android/window/SplashScreen) API will only work if the application is compiled in Android 12 (API 31). Update the ``` compileSdkVersion ``` to ``` 31 ``` in the ``` build.gradle ``` file.

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

<script src="https://gist.github.com/patildnyaneshwar/f44573637e40d6d73638050deb641d5e.js"></script>
