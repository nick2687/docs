# Phase 1: Task Breakdown
## HomeLens - Detailed Task List

**Phase:** 1 of 6  
**Duration:** 10 weeks (2.5 months)  
**Team:** 1 Developer (Full-time)

---

## Week 1: Project Setup & Infrastructure

### 1.1 iOS Project Setup (8 hours)
- [ ] **Task 1.1.1**: Create new Xcode project "HomeLens"
  - Minimum iOS version: 16.0
  - SwiftUI lifecycle
  - Enable Core Data
  - Set up project structure (App, Core, Features, UI, Resources)
  - **Estimate**: 2 hours

- [ ] **Task 1.1.2**: Configure build settings and schemes
  - Debug and Release configurations
  - Code signing (development team)
  - App icon and launch screen
  - **Estimate**: 1 hour

- [ ] **Task 1.1.3**: Set up dependency management (CocoaPods or SPM)
  - Add Firebase SDK
  - Add Sentry
  - Add testing frameworks (Quick, Nimble)
  - **Estimate**: 2 hours

- [ ] **Task 1.1.4**: Create Core Data model
  - Define entities: User, Inspection, InspectionSection, Finding, Photo
  - Set up relationships
  - Add indexes for performance
  - **Estimate**: 3 hours

### 1.2 Android Project Setup (8 hours)
- [ ] **Task 1.2.1**: Create new Android Studio project "HomeLens"
  - Minimum SDK: 29 (Android 10)
  - Kotlin + Jetpack Compose
  - Enable Room database
  - Set up project structure (matching iOS)
  - **Estimate**: 2 hours

- [ ] **Task 1.2.2**: Configure build.gradle files
  - Add dependencies (Room, CameraX, Firebase, Sentry)
  - Configure Kotlin and Compose compiler
  - Set up build variants
  - **Estimate**: 2 hours

- [ ] **Task 1.2.3**: Create Room database schema
  - Define entities (mirror iOS Core Data)
  - Create DAOs (Data Access Objects)
  - Set up database builder with migrations
  - **Estimate**: 3 hours

- [ ] **Task 1.2.4**: Configure app resources
  - App icon and splash screen
  - Colors, themes, strings
  - **Estimate**: 1 hour

### 1.3 Firebase & Backend Setup (6 hours)
- [ ] **Task 1.3.1**: Create Firebase project
  - Enable Authentication (email/password, Apple, Google)
  - Enable Firestore database
  - Enable Storage
  - **Estimate**: 1 hour

- [ ] **Task 1.3.2**: Configure Firebase for iOS
  - Download GoogleService-Info.plist
  - Initialize Firebase in AppDelegate
  - Test connection (basic)
  - **Estimate**: 1.5 hours

- [ ] **Task 1.3.3**: Configure Firebase for Android
  - Download google-services.json
  - Initialize Firebase in Application class
  - Test connection (basic)
  - **Estimate**: 1.5 hours

- [ ] **Task 1.3.4**: Set up Node.js API boilerplate
  - Create Express server
  - Basic health check endpoint
  - Environment variables
  - Deploy to Railway/Render (optional for Phase 1)
  - **Estimate**: 2 hours

### 1.4 CI/CD & Error Tracking (6 hours)
- [ ] **Task 1.4.1**: Set up GitHub repository
  - Initialize git repo: https://github.com/nick2687/swiftinspect.git
  - Create .gitignore (iOS, Android, Node.js)
  - Push initial commit
  - Set up branch protection (main)
  - **Estimate**: 1 hour

- [ ] **Task 1.4.2**: Configure GitHub Actions (iOS)
  - Workflow for build and test
  - Run on PR and push to main
  - **Estimate**: 2 hours

- [ ] **Task 1.4.3**: Configure GitHub Actions (Android)
  - Workflow for build and test
  - Run on PR and push to main
  - **Estimate**: 2 hours

- [ ] **Task 1.4.4**: Set up Sentry
  - Create iOS project in Sentry
  - Create Android project in Sentry
  - Initialize SDK in apps (don't send errors yet)
  - **Estimate**: 1 hour

### 1.5 Testing Framework Setup (4 hours)
- [ ] **Task 1.5.1**: Configure XCTest for iOS
  - Create unit test target
  - Create UI test target
  - Add Quick and Nimble
  - Write sample test
  - **Estimate**: 2 hours

- [ ] **Task 1.5.2**: Configure JUnit and Espresso for Android
  - Configure test dependencies
  - Create sample unit test
  - Create sample UI test
  - **Estimate**: 2 hours

**Week 1 Total**: 32 hours

---

## Week 2: Authentication & User Management

### 2.1 User Data Models (4 hours)
- [ ] **Task 2.1.1**: Implement User entity (iOS)
  - Core Data model attributes
  - CRUD operations
  - Unit tests
  - **Estimate**: 2 hours

- [ ] **Task 2.1.2**: Implement User entity (Android)
  - Room entity
  - DAO methods
  - Unit tests
  - **Estimate**: 2 hours

### 2.2 Authentication Service (10 hours)
- [ ] **Task 2.2.1**: Create AuthService (iOS)
  - Local authentication (email/password)
  - Store credentials in Keychain
  - Session management
  - User profile CRUD
  - **Estimate**: 5 hours

- [ ] **Task 2.2.2**: Create AuthService (Android)
  - Local authentication (email/password)
  - Store credentials in KeyStore
  - Session management
  - User profile CRUD
  - **Estimate**: 5 hours

### 2.3 UI: Authentication Flows (12 hours)
- [ ] **Task 2.3.1**: Welcome/Splash screen (iOS)
  - SwiftUI view
  - App logo and branding
  - Auto-navigate to login or home
  - **Estimate**: 2 hours

- [ ] **Task 2.3.2**: Welcome/Splash screen (Android)
  - Compose screen
  - Match iOS design
  - **Estimate**: 2 hours

- [ ] **Task 2.3.3**: Sign up screen (iOS)
  - Form: email, password, company name
  - Validation
  - Submit and create user
  - Error handling
  - **Estimate**: 3 hours

- [ ] **Task 2.3.4**: Sign up screen (Android)
  - Mirror iOS screen
  - **Estimate**: 3 hours

- [ ] **Task 2.3.5**: Login screen (iOS)
  - Form: email, password
  - Submit and authenticate
  - Navigate to home on success
  - **Estimate**: 1 hour

- [ ] **Task 2.3.6**: Login screen (Android)
  - Mirror iOS screen
  - **Estimate**: 1 hour

### 2.4 UI: Profile Setup (8 hours)
- [ ] **Task 2.4.1**: Terms of Service & Privacy Policy screen (iOS)
  - Display terms and privacy text
  - Accept checkbox
  - Store acceptance timestamp
  - **Estimate**: 2 hours

- [ ] **Task 2.4.2**: Terms of Service & Privacy Policy screen (Android)
  - Mirror iOS screen
  - **Estimate**: 2 hours

- [ ] **Task 2.4.3**: Profile setup screen (iOS)
  - Form: license number, phone, logo upload
  - Image picker for logo
  - Save profile
  - **Estimate**: 2 hours

- [ ] **Task 2.4.4**: Profile setup screen (Android)
  - Mirror iOS screen
  - **Estimate**: 2 hours

### 2.5 Testing (4 hours)
- [ ] **Task 2.5.1**: Unit tests for AuthService
  - Create user
  - Login
  - Update profile
  - **Estimate**: 2 hours

- [ ] **Task 2.5.2**: UI tests for auth flows
  - Sign up flow
  - Login flow
  - Profile setup flow
  - **Estimate**: 2 hours

**Week 2 Total**: 38 hours

---

## Week 3: Core Data Architecture & Repository Layer

### 3.1 Data Models Implementation (10 hours)
- [ ] **Task 3.1.1**: Implement Inspection entity (iOS)
  - Core Data model with relationships
  - CRUD operations
  - Unit tests
  - **Estimate**: 3 hours

- [ ] **Task 3.1.2**: Implement Inspection entity (Android)
  - Room entity with relations
  - DAO methods
  - Unit tests
  - **Estimate**: 3 hours

- [ ] **Task 3.1.3**: Implement InspectionSection entity (iOS)
  - Core Data model
  - Relationship to Inspection
  - Unit tests
  - **Estimate**: 2 hours

- [ ] **Task 3.1.4**: Implement InspectionSection entity (Android)
  - Room entity
  - Relations
  - Unit tests
  - **Estimate**: 2 hours

### 3.2 Finding & Photo Models (8 hours)
- [ ] **Task 3.2.1**: Implement Finding entity (iOS)
  - Core Data model
  - Severity enum
  - Unit tests
  - **Estimate**: 2 hours

- [ ] **Task 3.2.2**: Implement Finding entity (Android)
  - Room entity
  - Unit tests
  - **Estimate**: 2 hours

- [ ] **Task 3.2.3**: Implement Photo entity (iOS)
  - Core Data model with file paths
  - Annotation data (JSON)
  - Unit tests
  - **Estimate**: 2 hours

- [ ] **Task 3.2.4**: Implement Photo entity (Android)
  - Room entity
  - Unit tests
  - **Estimate**: 2 hours

### 3.3 Repository Layer (10 hours)
- [ ] **Task 3.3.1**: Create InspectionRepository (iOS)
  - Fetch all inspections
  - Fetch by ID
  - Create, Update, Delete
  - Fetch with relationships (sections, photos)
  - **Estimate**: 3 hours

- [ ] **Task 3.3.2**: Create InspectionRepository (Android)
  - Mirror iOS repository
  - **Estimate**: 3 hours

- [ ] **Task 3.3.3**: Create PhotoRepository (iOS)
  - Save photo with file system storage
  - Fetch photos for inspection/section
  - Delete photo (and files)
  - **Estimate**: 2 hours

- [ ] **Task 3.3.4**: Create PhotoRepository (Android)
  - Mirror iOS repository
  - **Estimate**: 2 hours

### 3.4 Testing (6 hours)
- [ ] **Task 3.4.1**: Integration tests (iOS)
  - Create inspection → add section → add finding → verify persistence
  - Test relationships
  - **Estimate**: 3 hours

- [ ] **Task 3.4.2**: Integration tests (Android)
  - Mirror iOS tests
  - **Estimate**: 3 hours

**Week 3 Total**: 34 hours

---

## Week 4-5: Camera & Photo Management

### 4.1 Camera Implementation (12 hours)
- [ ] **Task 4.1.1**: Create CameraService (iOS)
  - AVFoundation setup
  - Camera session management
  - Capture photo
  - Capture video (up to 60 sec)
  - Handle permissions
  - **Estimate**: 6 hours

- [ ] **Task 4.1.2**: Create CameraService (Android)
  - CameraX setup
  - Image capture
  - Video capture (60 sec limit)
  - Handle permissions
  - **Estimate**: 6 hours

### 4.2 Photo Compression & Thumbnails (10 hours)
- [ ] **Task 4.2.1**: Implement PhotoCompressor (iOS)
  - Resize to 2048px longest edge
  - JPEG compression at 85%
  - Preserve EXIF data
  - Unit tests (verify size reduction)
  - **Estimate**: 5 hours

- [ ] **Task 4.2.2**: Implement PhotoCompressor (Android)
  - Mirror iOS implementation
  - Unit tests
  - **Estimate**: 5 hours

### 4.3 Thumbnail Generation (8 hours)
- [ ] **Task 4.3.1**: Implement ThumbnailGenerator (iOS)
  - Generate 512px thumbnail
  - Generate 128px list thumbnail
  - Optimize for performance
  - Unit tests
  - **Estimate**: 4 hours

- [ ] **Task 4.3.2**: Implement ThumbnailGenerator (Android)
  - Mirror iOS implementation
  - Unit tests
  - **Estimate**: 4 hours

### 4.4 Photo Annotation Tools (16 hours)
- [ ] **Task 4.4.1**: Create Annotation data model (iOS)
  - Annotation struct (Codable)
  - AnnotationType enum (arrow, circle, rectangle, text)
  - Serialize to/from JSON
  - **Estimate**: 2 hours

- [ ] **Task 4.4.2**: Create Annotation data model (Android)
  - Mirror iOS model
  - **Estimate**: 2 hours

- [ ] **Task 4.4.3**: Implement drawing tools (iOS)
  - Canvas for drawing
  - Arrow tool
  - Circle tool
  - Rectangle tool
  - Text tool
  - Color picker
  - Undo/redo stack
  - **Estimate**: 6 hours

- [ ] **Task 4.4.4**: Implement drawing tools (Android)
  - Mirror iOS implementation
  - **Estimate**: 6 hours

### 4.5 UI: Camera & Annotation (20 hours)
- [ ] **Task 4.5.1**: CameraView (iOS)
  - Live preview
  - Capture button
  - Flash toggle
  - Switch camera (front/back)
  - Video mode toggle
  - **Estimate**: 5 hours

- [ ] **Task 4.5.2**: CameraView (Android)
  - Mirror iOS view
  - **Estimate**: 5 hours

- [ ] **Task 4.5.3**: PhotoAnnotationView (iOS)
  - Display captured photo
  - Overlay drawing canvas
  - Tool selector (arrow, circle, rectangle, text)
  - Color picker UI
  - Undo/redo buttons
  - Save button
  - **Estimate**: 5 hours

- [ ] **Task 4.5.4**: PhotoAnnotationView (Android)
  - Mirror iOS view
  - **Estimate**: 5 hours

### 4.6 Testing (10 hours)
- [ ] **Task 4.6.1**: Unit tests
  - Photo compression (verify dimensions, file size)
  - Thumbnail generation
  - Annotation serialization
  - **Estimate**: 4 hours

- [ ] **Task 4.6.2**: Integration tests
  - Capture → compress → generate thumbnails → save
  - **Estimate**: 3 hours

- [ ] **Task 4.6.3**: UI tests
  - Navigate to camera
  - Capture photo
  - Annotate photo
  - Save
  - **Estimate**: 3 hours

**Week 4-5 Total**: 76 hours (38 hours/week)

---

## Week 6-7: Inspection Management UI

### 6.1 UI: Inspection List (10 hours)
- [ ] **Task 6.1.1**: InspectionListView (iOS)
  - List of inspections
  - Sort by date
  - Filter by status
  - Swipe actions (delete)
  - Pull to refresh
  - Empty state
  - Floating + button (create new)
  - **Estimate**: 5 hours

- [ ] **Task 6.1.2**: InspectionListView (Android)
  - Mirror iOS view
  - **Estimate**: 5 hours

### 6.2 UI: Create Inspection (10 hours)
- [ ] **Task 6.2.1**: CreateInspectionView (iOS)
  - Form: property address, city, state, zip
  - Client name, email, phone
  - Inspection date picker
  - Inspection type dropdown
  - Notes (optional)
  - Save button
  - Validation
  - **Estimate**: 5 hours

- [ ] **Task 6.2.2**: CreateInspectionView (Android)
  - Mirror iOS view
  - **Estimate**: 5 hours

### 6.3 UI: Inspection Detail (14 hours)
- [ ] **Task 6.3.1**: InspectionDetailView (iOS)
  - Header: property address, client name, date
  - Progress indicator (% complete)
  - Sections list (tappable)
  - Add section button
  - Photos tab (all inspection photos)
  - Generate PDF button
  - Edit button (edit inspection details)
  - **Estimate**: 7 hours

- [ ] **Task 6.3.2**: InspectionDetailView (Android)
  - Mirror iOS view
  - **Estimate**: 7 hours

### 6.4 UI: Section Management (16 hours)
- [ ] **Task 6.4.1**: SectionView (iOS)
  - Section header (name, status)
  - Findings list
  - Add finding button
  - Photos list
  - Add photo button (camera or library)
  - Notes field
  - Mark section complete
  - **Estimate**: 8 hours

- [ ] **Task 6.4.2**: SectionView (Android)
  - Mirror iOS view
  - **Estimate**: 8 hours

### 6.5 UI: Finding Management (12 hours)
- [ ] **Task 6.5.1**: CreateFindingView (iOS)
  - Form: title, description
  - Severity picker (minor, major, safety hazard)
  - Recommendation field
  - Add photos button
  - Save button
  - **Estimate**: 6 hours

- [ ] **Task 6.5.2**: CreateFindingView (Android)
  - Mirror iOS view
  - **Estimate**: 6 hours

### 6.6 ViewModels & Business Logic (12 hours)
- [ ] **Task 6.6.1**: InspectionViewModel (iOS)
  - Fetch inspections
  - Create inspection
  - Update inspection
  - Delete inspection
  - Handle state (loading, success, error)
  - **Estimate**: 4 hours

- [ ] **Task 6.6.2**: InspectionViewModel (Android)
  - Mirror iOS ViewModel
  - **Estimate**: 4 hours

- [ ] **Task 6.6.3**: SectionViewModel (iOS)
  - Manage sections for inspection
  - Add/update/delete sections
  - Add findings to section
  - **Estimate**: 2 hours

- [ ] **Task 6.6.4**: SectionViewModel (Android)
  - Mirror iOS ViewModel
  - **Estimate**: 2 hours

### 6.7 Testing (10 hours)
- [ ] **Task 6.7.1**: Unit tests for ViewModels
  - Test business logic
  - Mock repository
  - **Estimate**: 4 hours

- [ ] **Task 6.7.2**: UI tests for inspection flows
  - Create inspection
  - Add section
  - Add finding
  - Add photo
  - **Estimate**: 6 hours

**Week 6-7 Total**: 74 hours (37 hours/week)

---

## Week 8-9: PDF Generation

### 8.1 PDF Generator Implementation (16 hours)
- [ ] **Task 8.1.1**: Create PDFGenerator service (iOS)
  - Initialize PDFKit context
  - Render text
  - Render images
  - Page layout management
  - Save to file
  - **Estimate**: 8 hours

- [ ] **Task 8.1.2**: Create PDFGenerator service (Android)
  - Initialize iText or PdfDocument
  - Render text
  - Render images
  - Page layout
  - Save to file
  - **Estimate**: 8 hours

### 8.2 PDF Template Components (20 hours)
- [ ] **Task 8.2.1**: Cover page template (iOS)
  - Inspector logo
  - Company name
  - Property address
  - Client name
  - Inspection date
  - **Estimate**: 4 hours

- [ ] **Task 8.2.2**: Cover page template (Android)
  - Mirror iOS template
  - **Estimate**: 4 hours

- [ ] **Task 8.2.3**: Table of Contents (iOS)
  - List sections with page numbers
  - Hyperlinks (if supported)
  - **Estimate**: 3 hours

- [ ] **Task 8.2.4**: Table of Contents (Android)
  - Mirror iOS template
  - **Estimate**: 3 hours

- [ ] **Task 8.2.5**: Section template (iOS)
  - Section title
  - Findings list
  - Photos grid (2 per row)
  - Photo captions
  - Page breaks
  - **Estimate**: 3 hours

- [ ] **Task 8.2.6**: Section template (Android)
  - Mirror iOS template
  - **Estimate**: 3 hours

### 8.3 PDF Export & Share (8 hours)
- [ ] **Task 8.3.1**: Export to Files app (iOS)
  - Generate PDF
  - Save to Documents directory
  - Open Files app to location
  - **Estimate**: 2 hours

- [ ] **Task 8.3.2**: Export to device storage (Android)
  - Generate PDF
  - Save to Downloads
  - Open file picker
  - **Estimate**: 2 hours

- [ ] **Task 8.3.3**: Share functionality (iOS)
  - UIActivityViewController
  - Share PDF via email, AirDrop, messages
  - **Estimate**: 2 hours

- [ ] **Task 8.3.4**: Share functionality (Android)
  - Share intent
  - Share PDF via email, messaging, etc.
  - **Estimate**: 2 hours

### 8.4 Performance Optimization (8 hours)
- [ ] **Task 8.4.1**: Optimize image embedding (iOS)
  - Compress images for PDF (if needed)
  - Progressive rendering
  - Memory management
  - **Estimate**: 4 hours

- [ ] **Task 8.4.2**: Optimize image embedding (Android)
  - Mirror iOS optimization
  - **Estimate**: 4 hours

### 8.5 UI: PDF Generation (8 hours)
- [ ] **Task 8.5.1**: PDF generation screen (iOS)
  - Progress indicator
  - Cancel button
  - Success state with preview
  - Error handling
  - **Estimate**: 4 hours

- [ ] **Task 8.5.2**: PDF generation screen (Android)
  - Mirror iOS screen
  - **Estimate**: 4 hours

### 8.6 Testing (12 hours)
- [ ] **Task 8.6.1**: Unit tests
  - PDF generation logic
  - Template rendering
  - **Estimate**: 4 hours

- [ ] **Task 8.6.2**: Integration tests
  - Generate PDF from inspection
  - Verify output file exists
  - **Estimate**: 4 hours

- [ ] **Task 8.6.3**: Performance tests
  - Measure generation time (10, 25, 50, 100 photos)
  - Profile memory usage
  - Optimize bottlenecks
  - **Estimate**: 4 hours

**Week 8-9 Total**: 72 hours (36 hours/week)

---

## Week 10: Testing, Bug Fixes & Documentation

### 10.1 Comprehensive Testing (16 hours)
- [ ] **Task 10.1.1**: Unit test review
  - Verify 70%+ coverage
  - Add missing tests
  - **Estimate**: 4 hours

- [ ] **Task 10.1.2**: Integration test review
  - Run all integration tests
  - Fix failing tests
  - **Estimate**: 4 hours

- [ ] **Task 10.1.3**: UI test review
  - Run all UI tests
  - Fix flaky tests
  - **Estimate**: 4 hours

- [ ] **Task 10.1.4**: Manual testing checklist
  - Complete 3 real inspections on iPhone
  - Complete 3 real inspections on Android
  - Test on older devices (iPhone 12, Pixel 5)
  - **Estimate**: 4 hours

### 10.2 Performance Profiling (8 hours)
- [ ] **Task 10.2.1**: Memory profiling (iOS)
  - Use Instruments
  - Check for leaks
  - Verify <250MB usage
  - **Estimate**: 4 hours

- [ ] **Task 10.2.2**: Memory profiling (Android)
  - Use Android Profiler
  - Check for leaks
  - Verify <250MB usage
  - **Estimate**: 4 hours

### 10.3 Bug Fixes (12 hours)
- [ ] **Task 10.3.1**: Critical bugs
  - Data loss issues
  - Crashes
  - Performance issues
  - **Estimate**: 6 hours

- [ ] **Task 10.3.2**: High-priority bugs
  - UI issues
  - Functional bugs
  - **Estimate**: 4 hours

- [ ] **Task 10.3.3**: Medium-priority bugs
  - Minor UI issues
  - Edge cases
  - **Estimate**: 2 hours

### 10.4 Documentation (8 hours)
- [ ] **Task 10.4.1**: Update README
  - Project overview
  - Setup instructions
  - Build instructions
  - **Estimate**: 2 hours

- [ ] **Task 10.4.2**: Code documentation
  - Add missing comments
  - Document complex logic
  - **Estimate**: 3 hours

- [ ] **Task 10.4.3**: API documentation
  - Document data models
  - Document services
  - **Estimate**: 2 hours

- [ ] **Task 10.4.4**: Testing documentation
  - Testing strategy
  - How to run tests
  - **Estimate**: 1 hour

### 10.5 Phase 1 Wrap-Up (4 hours)
- [ ] **Task 10.5.1**: Code review
  - Review all code
  - Refactor if needed
  - **Estimate**: 2 hours

- [ ] **Task 10.5.2**: Prepare demo
  - Create demo video (3-5 min)
  - Prepare screenshots
  - **Estimate**: 1 hour

- [ ] **Task 10.5.3**: Phase 2 planning
  - Review Phase 2 requirements
  - Update task estimates
  - **Estimate**: 1 hour

**Week 10 Total**: 48 hours

---

## Summary

### Total Hours by Week
- Week 1: 32 hours (Project Setup)
- Week 2: 38 hours (Authentication)
- Week 3: 34 hours (Data Architecture)
- Week 4-5: 76 hours (Camera & Photos)
- Week 6-7: 74 hours (Inspection UI)
- Week 8-9: 72 hours (PDF Generation)
- Week 10: 48 hours (Testing & Bug Fixes)

**Phase 1 Total: 374 hours (~9.4 weeks at 40 hours/week)**

### Key Milestones
- ✅ **End of Week 1**: Project infrastructure complete
- ✅ **End of Week 2**: Authentication and user profile functional
- ✅ **End of Week 3**: Data models and repositories complete
- ✅ **End of Week 5**: Photo capture and annotation working
- ✅ **End of Week 7**: Full inspection workflow functional
- ✅ **End of Week 9**: PDF generation complete
- ✅ **End of Week 10**: Phase 1 complete, ready for beta testing

### Risk Buffer
- Built-in buffer: ~0.6 weeks (24 hours) for unexpected issues
- If tasks take longer: can extend to full 10 weeks (400 hours)

---

**Document Version:** 1.0  
**Last Updated:** October 26, 2025  
**Status:** Ready to Start Development

