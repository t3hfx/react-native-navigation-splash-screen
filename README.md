# react-native-navigation-splash-screen
react-native-navigation v7 splash screen for Android and iOS 


# Android
splash screen jumps a bit up or scales, but @guyca's method worked. But I needed to do some adjustments to the xml file, because I've had a full screen sized splash screen. So, if you have this issue and you need to place full screen splash try this:
1. Add ConstraintsLayout to your dependencies as @guyca mentioned in android/app/build.gradle
```
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation "androidx.constraintlayout:constraintlayout:2.0.1" // <- add this
    ...
}
```
2. Add a folder layouts in the folder res, so you have this path: 
`android/app/src/main/res/layouts `
Modify your MainActivity.java to: 
```
package your_package_name;

import android.os.Bundle;

import com.reactnativenavigation.NavigationActivity;

import androidx.annotation.Nullable;

public class MainActivity extends NavigationActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setSplashLayout();
    }

    private void setSplashLayout() {
        setContentView(R.layouts.launch_screen); //launch_screen is the name of the xml file that we are going to create in layouts folder
    }
}
```
3. Create an xml file in layouts folder, in my case launch_screen.xml
```
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    android:orientation="vertical">
    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:scaleType="centerCrop"
        android:contentDescription="@string/app_name"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/splash_screen" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
```
android:layout_width="wrap_content" stands for wrapping image width to parent view
 android:layout_height="wrap_content" stands for wrapping image height to parent view
 android:scaleType="centerCrop" is like 'cover' for react-native image resizeMode (Scale the image uniformly (maintain the image's aspect ratio) so that both dimensions (width and height) of the image will be equal to or larger than the corresponding dimension of the view)
```
4. After that you need to add your image named splash_screen to the android/app/src/res/drawable folder
Now build and you don't have jumping splash screen anymore 

5. You can always add animations here by adding this to styles.xml near other <style>...</style>
```
<style name="SplashScreenThemeAnimation">
        <item name="android:windowExitAnimation">@anim/slide_out</item>
        <item name="android:windowEnterAnimation">@anim/slide_in</item>
</style>
```
