# Adding New Files to Xcode Project

## Files Created

The following files have been created and need to be added to the Xcode project:

### Core Data Models:
1. `ios/Core/Data/Models/User+CoreDataClass.swift`
2. `ios/Core/Data/Models/User+CoreDataProperties.swift`

### Repository:
3. `ios/Core/Data/Repositories/UserRepository.swift`

### Tests:
4. `ios/HomeLensTests/UserRepositoryTests.swift`

### Updated:
5. `ios/Core/Data/HomeLens.xcdatamodeld/HomeLens.xcdatamodel/contents` (Core Data model)
6. `ios/HomeLens/Persistence.swift` (Updated to use User instead of Item)

---

## How to Add Files to Xcode (Manual Steps):

### Option A: Using Xcode GUI (Recommended)

1. **Open the project:**
   ```bash
   open /Users/nclark/Documents/swiftinspect/ios/HomeLens.xcworkspace
   ```

2. **Add User Model Files:**
   - Right-click on `Core/Data/Models` folder in Xcode
   - Select "Add Files to 'HomeLens'..."
   - Navigate to `/Users/nclark/Documents/swiftinspect/ios/Core/Data/Models/`
   - Select both:
     - `User+CoreDataClass.swift`
     - `User+CoreDataProperties.swift`
   - Make sure "HomeLens" target is checked
   - Click "Add"

3. **Add User Repository:**
   - Right-click on `Core/Data/Repositories` folder
   - Select "Add Files to 'HomeLens'..."
   - Navigate to `/Users/nclark/Documents/swiftinspect/ios/Core/Data/Repositories/`
   - Select `UserRepository.swift`
   - Make sure "HomeLens" target is checked
   - Click "Add"

4. **Add Tests:**
   - Right-click on `HomeLensTests` folder
   - Select "Add Files to 'HomeLens'..."
   - Navigate to `/Users/nclark/Documents/swiftinspect/ios/HomeLensTests/`
   - Select `UserRepositoryTests.swift`
   - Make sure "HomeLensTests" target is checked
   - Click "Add"

5. **Build the project:**
   - Press `Cmd+B` to build
   - Should build successfully now!

6. **Run the tests:**
   - Press `Cmd+U` to run all tests
   - UserRepositoryTests should pass!

---

### Option B: Using Command Line (Faster)

Run this command to add the files:

```bash
cd /Users/nclark/Documents/swiftinspect/ios
open -a Xcode HomeLens.xcworkspace
```

Then manually drag the files into Xcode as described above.

---

## Verification

After adding the files, verify by:

1. **Check files appear in project navigator**
2. **Build the project** (`Cmd+B`) - should succeed
3. **Run tests** (`Cmd+U`) - all UserRepositoryTests should pass

---

## Next Steps

Once the files are added and building successfully, we can:
1. ✅ Mark User Model tasks as complete
2. Move on to AuthService (Keychain integration)
3. Build the authentication UI flows


