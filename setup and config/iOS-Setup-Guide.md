# iOS Xcode Project Setup Guide

## Step-by-Step: Creating the HomeLens iOS Project

### Prerequisites
- macOS 13.0 or later
- Xcode 15.0 or later
- CocoaPods installed (`sudo gem install cocoapods`)

---

## Part 1: Create Xcode Project

### Step 1: Open Xcode
1. Open Xcode
2. Click "Create New Project" or go to **File > New > Project**

### Step 2: Choose Template
1. Select **iOS** tab at the top
2. Select **App** template
3. Click **Next**

### Step 3: Configure Project
Fill in the following details:

| Field | Value |
|-------|-------|
| Product Name | `HomeLens` |
| Team | Select your development team |
| Organization Identifier | `com.swiftinspect` (or your identifier) |
| Bundle Identifier | `com.swiftinspect.HomeLens` |
| Interface | **SwiftUI** |
| Language | **Swift** |
| Storage | Check **Use Core Data** ✅ |
| Include Tests | Check both boxes ✅ |

4. Click **Next**

### Step 4: Choose Location
1. Navigate to: `/Users/nclark/Documents/Inspection App/swiftinspect/`
2. Create a new folder called `ios` (if it doesn't exist)
3. Save inside the `ios` folder
4. **IMPORTANT**: Uncheck "Create Git repository" (we already have one)
5. Click **Create**

### Step 5: Configure Build Settings
1. Select the **HomeLens** project in the navigator (blue icon at top)
2. Select the **HomeLens** target
3. Go to **General** tab:
   - **Minimum Deployments**: Set to **iOS 16.0**
   - **Supported Destinations**: iPhone and iPad
4. Go to **Signing & Capabilities**:
   - Select your team for signing
   - Ensure "Automatically manage signing" is checked

---

## Part 2: Organize Project Structure

### Step 6: Create Folder Structure
In Xcode's Project Navigator (left sidebar):

1. **Right-click** on the `HomeLens` folder (yellow folder with app icon)
2. Select **New Group** and create the following folders:

```
HomeLens/
├── App/                    (Create this)
├── Core/                   (Create this)
│   ├── Data/              (Create this)
│   │   ├── Models/        (Create this)
│   │   └── Repositories/  (Create this)
│   ├── Domain/            (Create this)
│   └── Utilities/         (Create this)
├── Features/              (Create this)
│   ├── Authentication/    (Create this)
│   ├── Inspection/        (Create this)
│   ├── Camera/            (Create this)
│   ├── Photo/             (Create this)
│   └── PDF/               (Create this)
├── UI/                    (Create this)
│   ├── Components/        (Create this)
│   ├── Theme/             (Create this)
│   └── Navigation/        (Create this)
└── Resources/             (Already exists - move Assets.xcassets here)
```

**To create groups:**
- Right-click parent folder → New Group
- Name it according to structure above
- Repeat for all folders

### Step 7: Move Existing Files
Move the auto-generated files into proper locations:

1. **Move** `HomeLensApp.swift` → **App/** folder
   - Drag and drop in Xcode
2. **Move** `ContentView.swift` → **UI/** folder (we'll organize later)
3. **Move** `Assets.xcassets` → **Resources/** folder
4. **Move** `HomeLens.xcdatamodeld` → **Core/Data/** folder
5. Keep `Info.plist` and `Preview Content` where they are

---

## Part 3: Set Up CocoaPods

### Step 8: Close Xcode
Close Xcode completely before setting up CocoaPods.

### Step 9: Create Podfile
Open Terminal and run:

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect/ios"
pod init
```

This creates a `Podfile`.

### Step 10: Edit Podfile
Open the Podfile in a text editor:

```bash
open -a Xcode Podfile
```

Replace the contents with:

```ruby
# Podfile for HomeLens

platform :ios, '16.0'
use_frameworks!

target 'HomeLens' do
  # Firebase
  pod 'Firebase/Auth'
  pod 'Firebase/Firestore'
  pod 'Firebase/Storage'
  
  # Error Tracking
  pod 'Sentry', '~> 8.0'
  
  # Testing
  target 'HomeLensTests' do
    inherit! :search_paths
    pod 'Quick', '~> 7.0'
    pod 'Nimble', '~> 12.0'
  end
  
  target 'HomeLensUITests' do
    inherit! :search_paths
  end
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '16.0'
    end
  end
end
```

Save and close.

### Step 11: Install Pods
In Terminal:

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect/ios"
pod install
```

This will take a few minutes. You should see:
```
Pod installation complete! There are X dependencies from the Podfile...
```

### Step 12: Open Workspace (IMPORTANT!)
From now on, **always open the `.xcworkspace` file**, not the `.xcodeproj`:

```bash
open HomeLens.xcworkspace
```

Or double-click `HomeLens.xcworkspace` in Finder.

---

## Part 4: Configure Firebase

### Step 13: Create Firebase Project
1. Go to https://console.firebase.google.com
2. Click **Add project**
3. Name: `HomeLens` (or `swiftinspect-dev` for development)
4. Enable Google Analytics (optional, can disable)
5. Click **Create project**

### Step 14: Add iOS App to Firebase
1. In Firebase Console, click the iOS icon (or **Add app**)
2. Enter iOS bundle ID: `com.swiftinspect.HomeLens`
3. App nickname: `HomeLens iOS`
4. **Skip** App Store ID for now
5. Click **Register app**

### Step 15: Download GoogleService-Info.plist
1. Download the `GoogleService-Info.plist` file
2. In Xcode, **right-click** on **Resources/** folder
3. Select **Add Files to "HomeLens"...**
4. Navigate to Downloads, select `GoogleService-Info.plist`
5. **IMPORTANT**: Check "Copy items if needed"
6. **IMPORTANT**: Check "HomeLens" under **Add to targets**
7. Click **Add**

### Step 16: Initialize Firebase in App
Open `HomeLensApp.swift` (in the **App/** folder) and replace with:

```swift
import SwiftUI
import FirebaseCore

@main
struct HomeLensApp: App {
    
    init() {
        // Configure Firebase
        FirebaseApp.configure()
    }
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

---

## Part 5: Verify Setup

### Step 17: Build and Run
1. In Xcode, select a simulator (e.g., iPhone 15)
2. Press **Cmd + B** to build
3. If successful, press **Cmd + R** to run
4. You should see the default SwiftUI "Hello, world!" screen

### Step 18: Run Tests
1. Press **Cmd + U** to run tests
2. All tests should pass (there's just a basic example test)

---

## Part 6: Configure Sentry

### Step 19: Create Sentry Project
1. Go to https://sentry.io (create free account if needed)
2. Create new project
3. Select **iOS** / **Swift**
4. Name it: `swiftinspect-ios`
5. Copy the DSN (looks like: `https://xxx@xxx.ingest.sentry.io/xxx`)

### Step 20: Initialize Sentry
Open `HomeLensApp.swift` and update:

```swift
import SwiftUI
import FirebaseCore
import Sentry

@main
struct HomeLensApp: App {
    
    init() {
        // Configure Firebase
        FirebaseApp.configure()
        
        // Configure Sentry
        SentrySDK.start { options in
            options.dsn = "YOUR_SENTRY_DSN_HERE"
            options.debug = true // Disable in production
            options.tracesSampleRate = 1.0 // Adjust for production
        }
    }
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

---

## Part 7: Git Commit

### Step 21: Commit iOS Project
In Terminal:

```bash
cd "/Users/nclark/Documents/Inspection App/swiftinspect"
git add ios/
git commit -m "Add iOS Xcode project with CocoaPods and Firebase setup

- Created HomeLens iOS project with SwiftUI
- Organized project structure (App, Core, Features, UI, Resources)
- Added CocoaPods dependencies (Firebase, Sentry, testing frameworks)
- Configured Firebase with GoogleService-Info.plist
- Configured Sentry for crash reporting
- Set minimum iOS version to 16.0
- Enabled Core Data

Status: Week 1 Task 1.1 complete"
```

---

## ✅ iOS Setup Complete!

You should now have:
- ✅ Xcode project with proper structure
- ✅ CocoaPods dependencies installed
- ✅ Firebase configured
- ✅ Sentry configured
- ✅ Project builds and runs successfully

### Next Steps:
1. Proceed to Android setup (see Android guide)
2. Start implementing Core Data models (Week 3)
3. Build authentication UI (Week 2)

---

## Troubleshooting

### Issue: "No such module 'Firebase'"
**Solution**: Make sure you opened `HomeLens.xcworkspace`, not `.xcodeproj`

### Issue: Pod install fails
**Solution**: 
```bash
sudo gem install cocoapods
pod repo update
pod install --repo-update
```

### Issue: Build fails with signing errors
**Solution**: Go to Signing & Capabilities, select your team, or use "Automatically manage signing"

### Issue: Simulator not showing
**Solution**: Go to Xcode > Settings > Platforms and download iOS simulators

---

**Document Version:** 1.0  
**Last Updated:** October 26, 2025  
**Status:** Ready for Use

