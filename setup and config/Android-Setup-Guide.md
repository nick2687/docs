# Android Project Setup Guide

## Step-by-Step: Creating the HomeLens Android Project

### Prerequisites
- JDK 17 installed
- Android Studio Hedgehog (2023.1.1) or later
- Android SDK 34 (Android 14)
- At least 8GB RAM available

---

## Part 1: Create Android Project via Command Line

We'll create the entire project structure using command-line tools, which is faster and more reproducible.

### Step 1: Open Terminal

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect"
```

### Step 2: Create Android Directory Structure

```bash
mkdir -p android/app/src/main/java/com/swiftinspect
mkdir -p android/app/src/main/res/values
mkdir -p android/app/src/main/res/drawable
mkdir -p android/app/src/main/res/mipmap-hdpi
mkdir -p android/app/src/main/res/mipmap-mdpi
mkdir -p android/app/src/main/res/mipmap-xhdpi
mkdir -p android/app/src/main/res/mipmap-xxhdpi
mkdir -p android/app/src/main/res/mipmap-xxxhdpi
mkdir -p android/app/src/test/java/com/swiftinspect
mkdir -p android/app/src/androidTest/java/com/swiftinspect
mkdir -p android/gradle/wrapper
```

### Step 3: Create Root build.gradle

```bash
cat > android/build.gradle << 'EOF'
// Top-level build file
buildscript {
    ext {
        kotlin_version = '1.9.20'
        compose_version = '1.5.4'
        compose_compiler_version = '1.5.4'
    }
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:8.1.4'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath 'com.google.gms:google-services:4.4.0'
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
EOF
```

### Step 4: Create settings.gradle

```bash
cat > android/settings.gradle << 'EOF'
pluginManagement {
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}

dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.name = "HomeLens"
include ':app'
EOF
```

### Step 5: Create gradle.properties

```bash
cat > android/gradle.properties << 'EOF'
# Project-wide Gradle settings
org.gradle.jvmargs=-Xmx2048m -Dfile.encoding=UTF-8
android.useAndroidX=true
android.enableJetifier=true
kotlin.code.style=official
android.nonTransitiveRClass=true
EOF
```

### Step 6: Create App build.gradle

```bash
cat > android/app/build.gradle << 'EOF'
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-kapt'
    id 'com.google.gms.google-services'
}

android {
    namespace 'com.swiftinspect'
    compileSdk 34

    defaultConfig {
        applicationId "com.swiftinspect"
        minSdk 29
        targetSdk 34
        versionCode 1
        versionName "1.0.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
        vectorDrawables {
            useSupportLibrary true
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
        debug {
            applicationIdSuffix ".debug"
            debuggable true
        }
    }
    
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_17
        targetCompatibility JavaVersion.VERSION_17
    }
    
    kotlinOptions {
        jvmTarget = '17'
    }
    
    buildFeatures {
        compose true
    }
    
    composeOptions {
        kotlinCompilerExtensionVersion = '1.5.4'
    }
    
    packaging {
        resources {
            excludes += '/META-INF/{AL2.0,LGPL2.1}'
        }
    }
}

dependencies {
    // Core Android
    implementation 'androidx.core:core-ktx:1.12.0'
    implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.7.0'
    implementation 'androidx.activity:activity-compose:1.8.2'
    
    // Jetpack Compose
    implementation platform('androidx.compose:compose-bom:2023.10.01')
    implementation 'androidx.compose.ui:ui'
    implementation 'androidx.compose.ui:ui-graphics'
    implementation 'androidx.compose.ui:ui-tooling-preview'
    implementation 'androidx.compose.material3:material3'
    implementation 'androidx.compose.material:material-icons-extended'
    
    // Navigation
    implementation 'androidx.navigation:navigation-compose:2.7.6'
    
    // Room Database
    implementation "androidx.room:room-runtime:2.6.1"
    implementation "androidx.room:room-ktx:2.6.1"
    kapt "androidx.room:room-compiler:2.6.1"
    
    // CameraX
    implementation "androidx.camera:camera-camera2:1.3.1"
    implementation "androidx.camera:camera-lifecycle:1.3.1"
    implementation "androidx.camera:camera-view:1.3.1"
    
    // Firebase
    implementation platform('com.google.firebase:firebase-bom:32.7.0')
    implementation 'com.google.firebase:firebase-auth-ktx'
    implementation 'com.google.firebase:firebase-firestore-ktx'
    implementation 'com.google.firebase:firebase-storage-ktx'
    
    // Sentry
    implementation 'io.sentry:sentry-android:6.34.0'
    
    // PDF Generation
    implementation 'com.itextpdf:itext7-core:7.2.5'
    
    // Coil (Image Loading)
    implementation 'io.coil-kt:coil-compose:2.5.0'
    
    // Coroutines
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3'
    
    // Testing
    testImplementation 'junit:junit:4.13.2'
    testImplementation 'org.mockito:mockito-core:5.7.0'
    testImplementation 'androidx.arch.core:core-testing:2.2.0'
    testImplementation 'org.jetbrains.kotlinx:kotlinx-coroutines-test:1.7.3'
    
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
    androidTestImplementation platform('androidx.compose:compose-bom:2023.10.01')
    androidTestImplementation 'androidx.compose.ui:ui-test-junit4'
    
    debugImplementation 'androidx.compose.ui:ui-tooling'
    debugImplementation 'androidx.compose.ui:ui-test-manifest'
}
EOF
```

### Step 7: Create AndroidManifest.xml

```bash
cat > android/app/src/main/AndroidManifest.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- Permissions -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
        android:maxSdkVersion="28" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"
        android:maxSdkVersion="32" />
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
    <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />

    <uses-feature android:name="android.hardware.camera" android:required="false" />
    <uses-feature android:name="android.hardware.camera.autofocus" android:required="false" />

    <application
        android:name=".HomeLensApplication"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.HomeLens">
        
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:theme="@style/Theme.HomeLens"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        
        <!-- FileProvider for sharing files -->
        <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="${applicationId}.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths" />
        </provider>
    </application>
</manifest>
EOF
```

### Step 8: Create Application Class

```bash
cat > android/app/src/main/java/com/swiftinspect/HomeLensApplication.kt << 'EOF'
package com.swiftinspect

import android.app.Application
import com.google.firebase.FirebaseApp
import io.sentry.android.core.SentryAndroid

class HomeLensApplication : Application() {
    
    override fun onCreate() {
        super.onCreate()
        
        // Initialize Firebase
        FirebaseApp.initializeApp(this)
        
        // Initialize Sentry
        SentryAndroid.init(this) { options ->
            options.dsn = "YOUR_SENTRY_DSN_HERE"
            options.isDebug = BuildConfig.DEBUG
            options.tracesSampleRate = 1.0
        }
    }
}
EOF
```

### Step 9: Create MainActivity

```bash
cat > android/app/src/main/java/com/swiftinspect/MainActivity.kt << 'EOF'
package com.swiftinspect

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.swiftinspect.ui.theme.HomeLensTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            HomeLensTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    Greeting("HomeLens")
                }
            }
        }
    }
}

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Text(
        text = "Hello $name!",
        modifier = modifier
    )
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    HomeLensTheme {
        Greeting("HomeLens")
    }
}
EOF
```

### Step 10: Create Theme Files

```bash
# Create Theme.kt
cat > android/app/src/main/java/com/swiftinspect/ui/theme/Theme.kt << 'EOF'
package com.swiftinspect.ui.theme

import android.app.Activity
import android.os.Build
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.darkColorScheme
import androidx.compose.material3.dynamicDarkColorScheme
import androidx.compose.material3.dynamicLightColorScheme
import androidx.compose.material3.lightColorScheme
import androidx.compose.runtime.Composable
import androidx.compose.runtime.SideEffect
import androidx.compose.ui.graphics.toArgb
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.platform.LocalView
import androidx.core.view.WindowCompat

private val DarkColorScheme = darkColorScheme(
    primary = Purple80,
    secondary = PurpleGrey80,
    tertiary = Pink80
)

private val LightColorScheme = lightColorScheme(
    primary = Purple40,
    secondary = PurpleGrey40,
    tertiary = Pink40
)

@Composable
fun HomeLensTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }
    val view = LocalView.current
    if (!view.isInEditMode) {
        SideEffect {
            val window = (view.context as Activity).window
            window.statusBarColor = colorScheme.primary.toArgb()
            WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = darkTheme
        }
    }

    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography,
        content = content
    )
}
EOF

# Create Color.kt
cat > android/app/src/main/java/com/swiftinspect/ui/theme/Color.kt << 'EOF'
package com.swiftinspect.ui.theme

import androidx.compose.ui.graphics.Color

val Purple80 = Color(0xFFD0BCFF)
val PurpleGrey80 = Color(0xFFCCC2DC)
val Pink80 = Color(0xFFEFB8C8)

val Purple40 = Color(0xFF6650a4)
val PurpleGrey40 = Color(0xFF625b71)
val Pink40 = Color(0xFF7D5260)
EOF

# Create Type.kt
cat > android/app/src/main/java/com/swiftinspect/ui/theme/Type.kt << 'EOF'
package com.swiftinspect.ui.theme

import androidx.compose.material3.Typography
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.FontFamily
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.sp

val Typography = Typography(
    bodyLarge = TextStyle(
        fontFamily = FontFamily.Default,
        fontWeight = FontWeight.Normal,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.5.sp
    )
)
EOF
```

### Step 11: Create Resources

```bash
# strings.xml
cat > android/app/src/main/res/values/strings.xml << 'EOF'
<resources>
    <string name="app_name">HomeLens</string>
</resources>
EOF

# colors.xml
cat > android/app/src/main/res/values/colors.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="purple_200">#FFBB86FC</color>
    <color name="purple_500">#FF6200EE</color>
    <color name="purple_700">#FF3700B3</color>
    <color name="teal_200">#FF03DAC5</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
</resources>
EOF

# themes.xml
cat > android/app/src/main/res/values/themes.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.HomeLens" parent="android:Theme.Material.Light.NoActionBar" />
</resources>
EOF

# file_paths.xml (for FileProvider)
mkdir -p android/app/src/main/res/xml
cat > android/app/src/main/res/xml/file_paths.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <files-path name="files" path="." />
    <cache-path name="cache" path="." />
    <external-files-path name="external_files" path="." />
</paths>
EOF
```

### Step 12: Create Gradle Wrapper

```bash
cd android

# Download and set up Gradle wrapper
gradle wrapper --gradle-version 8.2

# Make gradlew executable
chmod +x gradlew
```

### Step 13: Create proguard-rules.pro

```bash
cat > android/app/proguard-rules.pro << 'EOF'
# Add project specific ProGuard rules here.
-keepattributes *Annotation*
-keepattributes SourceFile,LineNumberTable
-keep public class * extends java.lang.Exception

# Firebase
-keep class com.google.firebase.** { *; }

# Room
-keep class * extends androidx.room.RoomDatabase
-keep @androidx.room.Entity class *
-dontwarn androidx.room.paging.**

# Sentry
-keepattributes LineNumberTable,SourceFile
-dontwarn org.slf4j.**
EOF
```

---

## Part 2: Firebase Setup

### Step 14: Add Android App to Firebase
1. Go to Firebase Console: https://console.firebase.google.com
2. Select your HomeLens project (created for iOS)
3. Click **Add app** → Select **Android**
4. Enter package name: `com.swiftinspect`
5. App nickname: `HomeLens Android`
6. **Skip** SHA-1 for now (needed later for Google Sign-In)
7. Click **Register app**

### Step 15: Download google-services.json
1. Download `google-services.json`
2. Move it to the correct location:

```bash
mv ~/Downloads/google-services.json "/Users/nclark/Documents/Inspection App/swiftinspect/android/app/"
```

---

## Part 3: Build and Verify

### Step 16: Build the Project

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect/android"
./gradlew build
```

This will take 5-10 minutes the first time as it downloads dependencies.

### Step 17: Run on Emulator (Optional)

If you have an Android emulator set up:

```bash
./gradlew installDebug
```

Or open in Android Studio:

```bash
open -a "Android Studio" .
```

Then click the green "Run" button.

---

## Part 4: Organize Project Structure

### Step 18: Create Package Structure

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect/android/app/src/main/java/com/swiftinspect"

# Create directory structure
mkdir -p core/data/database
mkdir -p core/data/entities
mkdir -p core/data/repositories
mkdir -p core/domain/models
mkdir -p core/domain/usecases
mkdir -p core/utils
mkdir -p features/auth/ui
mkdir -p features/auth/viewmodel
mkdir -p features/inspection/ui
mkdir -p features/inspection/viewmodel
mkdir -p features/camera/ui
mkdir -p features/camera/viewmodel
mkdir -p features/photo
mkdir -p features/pdf
mkdir -p ui/components
mkdir -p ui/navigation

echo "✅ Package structure created!"
```

---

## Part 5: Git Commit

### Step 19: Commit Android Project

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect"
git add android/
git commit -m "Add Android project with Gradle and Firebase setup

- Created Android project with Jetpack Compose
- Organized project structure (core, features, ui packages)
- Added Gradle dependencies (Room, Firebase, CameraX, Sentry)
- Configured Firebase with google-services.json
- Configured Sentry for crash reporting
- Set minimum SDK to 29 (Android 10)
- Created MainActivity with Compose
- Set up Material 3 theming

Status: Week 1 Task 1.2 complete"
```

---

## ✅ Android Setup Complete!

You should now have:
- ✅ Complete Android project structure
- ✅ All Gradle dependencies configured
- ✅ Firebase configured
- ✅ Sentry configured
- ✅ Jetpack Compose with Material 3
- ✅ Ready to build and run

### Verify Setup:

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect/android"
./gradlew build --info
```

If successful, you'll see:
```
BUILD SUCCESSFUL in XXs
```

---

## Next Steps

1. Open project in Android Studio to verify
2. Create an Android emulator (if you don't have one)
3. Run the app to test
4. Start implementing Room database (Week 3)
5. Build authentication UI (Week 2)

---

## Troubleshooting

### Issue: "SDK location not found"
**Solution**: Create `local.properties`:
```bash
echo "sdk.dir=/Users/$USER/Library/Android/sdk" > android/local.properties
```

### Issue: Gradle build fails
**Solution**: 
```bash
cd android
./gradlew clean
./gradlew build --refresh-dependencies
```

### Issue: "Could not find com.android.tools.build:gradle"
**Solution**: Update Android Studio or run:
```bash
./gradlew wrapper --gradle-version=8.2 --distribution-type=all
```

### Issue: Firebase not initialized
**Solution**: Make sure `google-services.json` is in `android/app/` directory

---

**Document Version:** 1.0  
**Last Updated:** October 26, 2025  
**Status:** Ready for Use

