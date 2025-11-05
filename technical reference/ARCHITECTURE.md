# HomeLens - Architecture Overview

## System Architecture

HomeLens is built on an offline-first, mobile-native architecture designed to work seamlessly without internet connectivity while supporting cloud sync when online.

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Client Applications                       │
│                                                                  │
│  ┌──────────────────────┐         ┌──────────────────────┐     │
│  │   iOS Application    │         │  Android Application │     │
│  │   (Swift/SwiftUI)    │         │  (Kotlin/Compose)    │     │
│  │                      │         │                      │     │
│  │  ┌────────────────┐  │         │  ┌────────────────┐  │     │
│  │  │  Presentation  │  │         │  │  Presentation  │  │     │
│  │  │  (Views/UI)    │  │         │  │  (Composables) │  │     │
│  │  └────────────────┘  │         │  └────────────────┘  │     │
│  │  ┌────────────────┐  │         │  ┌────────────────┐  │     │
│  │  │  Business      │  │         │  │  Business      │  │     │
│  │  │  Logic         │  │         │  │  Logic         │  │     │
│  │  │  (ViewModels)  │  │         │  │  (ViewModels)  │  │     │
│  │  └────────────────┘  │         │  └────────────────┘  │     │
│  │  ┌────────────────┐  │         │  ┌────────────────┐  │     │
│  │  │  Data Layer    │  │         │  │  Data Layer    │  │     │
│  │  │  (Repositories)│  │         │  │  (Repositories)│  │     │
│  │  └────────────────┘  │         │  └────────────────┘  │     │
│  │  ┌────────────────┐  │         │  ┌────────────────┐  │     │
│  │  │  Local Storage │  │         │  │  Local Storage │  │     │
│  │  │  (Core Data)   │  │         │  │  (Room)        │  │     │
│  │  └────────────────┘  │         │  └────────────────┘  │     │
│  └──────────────────────┘         └──────────────────────┘     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ Internet (Optional)
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        Cloud Services                            │
│                                                                  │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐  │
│  │  Firebase Auth   │  │  Firebase        │  │  Cloudflare  │  │
│  │  (User           │  │  Firestore       │  │  R2          │  │
│  │  Authentication) │  │  (Cloud DB)      │  │  (Photos)    │  │
│  └──────────────────┘  └──────────────────┘  └──────────────┘  │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │              Node.js API Server (Railway/Render)           │ │
│  │  - PDF Generation (server-side fallback)                   │ │
│  │  - Payment Processing (Stripe)                             │ │
│  │  - Email Notifications (SendGrid)                          │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Data Flow

### Phase 1: Offline-Only (Current)

```
User Action → View → ViewModel → Repository → Core Data/Room → File System
                                                    ↓
                                            Local Storage
```

### Phase 3: With Cloud Sync

```
User Action → View → ViewModel → Repository → Core Data/Room
                                                    ↓
                                         Sync Service
                                                    ↓
                                    ┌───────────────┴────────────┐
                                    │                            │
                            Firebase Firestore          Cloudflare R2
                            (Metadata/Text)             (Photos/Videos)
```

## Mobile Application Architecture

### iOS (Swift + SwiftUI)

**Layer Structure:**

1. **Presentation Layer** (`UI/` and `Features/*/Views/`)
   - SwiftUI views
   - View composition
   - User interface logic
   - Navigation

2. **Business Logic Layer** (`Features/*/ViewModels/`)
   - ViewModels (ObservableObject)
   - Use cases
   - Business rules
   - State management

3. **Data Layer** (`Core/Data/Repositories/`)
   - Repository pattern
   - Data transformation
   - Caching strategy
   - Sync coordination

4. **Storage Layer** (`Core/Data/`)
   - Core Data stack
   - File system access
   - Keychain for credentials
   - UserDefaults for settings

**Key Technologies:**
- **UI**: SwiftUI + Combine
- **Database**: Core Data
- **Photo Processing**: Core Image + UIKit
- **PDF Generation**: PDFKit
- **Camera**: AVFoundation
- **Testing**: XCTest + XCUITest

### Android (Kotlin + Jetpack Compose)

**Layer Structure:**

1. **Presentation Layer** (`ui/` and `features/*/ui/`)
   - Jetpack Compose screens
   - Composable functions
   - UI state
   - Navigation

2. **Business Logic Layer** (`features/*/viewmodels/`)
   - ViewModels (AndroidViewModel)
   - Use cases
   - Business rules
   - StateFlow/LiveData

3. **Data Layer** (`core/data/repositories/`)
   - Repository pattern
   - Data transformation
   - Caching
   - Sync coordination

4. **Storage Layer** (`core/data/database/`)
   - Room database
   - File system access
   - EncryptedSharedPreferences for credentials
   - DataStore for settings

**Key Technologies:**
- **UI**: Jetpack Compose
- **Database**: Room
- **Photo Processing**: Android Image API
- **PDF Generation**: iText or PdfDocument API
- **Camera**: CameraX
- **Testing**: JUnit + Espresso

## Data Models

### Core Entities

1. **User**
   - Profile information
   - Company details
   - Settings
   - Subscription info

2. **Inspection**
   - Property details
   - Client information
   - Inspection metadata
   - Status tracking

3. **InspectionSection**
   - Section name (Roof, Foundation, etc.)
   - Order
   - Completion status
   - Notes

4. **Finding**
   - Title and description
   - Severity level
   - Recommendations
   - Associated photos

5. **Photo**
   - File paths (original, compressed, thumbnails)
   - Metadata (dimensions, size, timestamp)
   - Annotations (stored as JSON)
   - Relationships to inspection/section/finding

### Data Relationships

```
User
  └─ Inspections (1:N)
       ├─ Sections (1:N)
       │    ├─ Findings (1:N)
       │    │    └─ Photos (1:N)
       │    └─ Photos (1:N)
       └─ Photos (1:N)
```

## File Storage Strategy

### Photo Storage Structure

```
App Documents/
├── users/
│   └── {userId}/
│       └── inspections/
│           └── {inspectionId}/
│               ├── photos/
│               │   ├── original/          # Temporary, deleted after compression
│               │   ├── compressed/        # 2048px, 85% quality (used in reports)
│               │   ├── thumbnails/        # 512px (gallery view)
│               │   └── list-thumbnails/   # 128px (list view)
│               ├── videos/
│               │   └── {videoId}.mp4
│               └── generated-reports/
│                   └── {reportId}.pdf
```

### File Naming Convention

```
Photo: {inspectionId}_{sectionId}_{timestamp}_{uuid}.jpg
Video: {inspectionId}_{sectionId}_{timestamp}_{uuid}.mp4
PDF: {inspectionId}_{date}_{clientName}.pdf
```

## Offline-First Strategy

### Core Principles

1. **All operations work offline first**
   - Create, read, update, delete happen locally
   - Cloud sync happens in background when online
   - User never blocked by network state

2. **Sync queue**
   - Operations queued when offline
   - Auto-retry when connection restored
   - Conflict resolution on sync

3. **Progressive enhancement**
   - Base functionality: 100% offline
   - Enhanced functionality: Requires connectivity (Phase 3+)

### Sync Strategy (Phase 3)

**Sync Triggers:**
1. App launch (if online)
2. Return from background (if online)
3. Manual refresh
4. After significant changes

**Sync Priority:**
1. User profile and settings
2. Inspection metadata
3. Photo thumbnails
4. Full-resolution photos
5. Videos

**Conflict Resolution:**
- **Simple fields**: Last-write-wins with timestamp
- **Complex fields**: Show both versions, user chooses
- **Photos**: No conflicts (immutable once uploaded)

## Performance Optimization

### Photo Processing Pipeline

```
Capture (12-48MP)
    ↓
Compress to 2048px @ 85% quality (~500KB)
    ↓
Generate 512px thumbnail (~50KB)
    ↓
Generate 128px list thumbnail (~10KB)
    ↓
Save all to local storage
    ↓
(Phase 3) Upload thumbnail first, then compressed
```

### PDF Generation Strategy

**Phase 1: On-Device Only**
- Generate PDF on device using PDFKit (iOS) or iText (Android)
- Target: <30 seconds for 50 photos
- Memory limit: <250MB
- Optimization: Load photos progressively, release after rendering

**Phase 3+: Hybrid Approach**
- Simple reports (<25 photos): On-device
- Complex reports (>25 photos): Server-side generation (optional)
- User can choose preference

### Database Query Optimization

1. **Indexes** on frequently queried fields:
   - User ID
   - Inspection date
   - Inspection status
   - Section order

2. **Lazy loading**:
   - Load inspection list without photos
   - Load photos only when viewing section
   - Paginate large photo lists

3. **Caching**:
   - Cache recent inspections in memory
   - Cache user profile and settings
   - Invalidate cache on updates

## Security Architecture

### Phase 1: Local Security

1. **Authentication**
   - Email/password stored in Keychain (iOS) or KeyStore (Android)
   - Simple local authentication
   - No network auth yet

2. **Data Storage**
   - App sandbox (iOS/Android native security)
   - No additional encryption needed (device encryption sufficient)
   - Photos in app's private directory

### Phase 2+: Cloud Security

1. **Authentication**
   - Firebase Auth (email/password, Apple, Google)
   - JWT tokens
   - Automatic token refresh

2. **Data Access**
   - Firestore security rules: User can only access their own data
   - Storage security rules: User can only access their own photos
   - API authentication with Bearer tokens

3. **Data in Transit**
   - HTTPS everywhere
   - Certificate pinning (optional, Phase 3)

4. **Data at Rest**
   - Firebase automatic encryption
   - Cloudflare R2 encryption
   - Local database encryption (if needed for compliance)

## Testing Strategy

### Unit Testing

**iOS (XCTest):**
```
Tests/
├── CoreTests/
│   ├── DataModelTests/
│   ├── RepositoryTests/
│   └── UseCaseTests/
├── PhotoTests/
│   ├── CompressionTests/
│   ├── AnnotationTests/
│   └── ThumbnailTests/
└── PDFTests/
    └── GenerationTests/
```

**Android (JUnit):**
```
test/
├── core/
│   ├── data/
│   └── domain/
├── photo/
│   ├── compression/
│   └── annotation/
└── pdf/
    └── generation/
```

### Integration Testing

- Test complete user flows
- Mock external dependencies
- Test offline → online transitions
- Test data persistence

### UI Testing

**iOS (XCUITest):**
- Test critical user journeys
- Test on multiple device sizes
- Test accessibility

**Android (Espresso):**
- Test critical user journeys
- Test on multiple device sizes
- Test accessibility

### Manual Testing

- Real device testing (iPhone, Android)
- Complete real inspections
- Performance profiling
- Memory leak detection

## Monitoring & Analytics

### Phase 1: Development

- **Crash Reporting**: Sentry
- **Local Logging**: OSLog (iOS), Logcat (Android)
- **Performance**: Instruments (iOS), Android Profiler

### Phase 6+: Production

- **Crash Reporting**: Sentry
- **Analytics**: Firebase Analytics
- **Performance**: Firebase Performance Monitoring
- **Uptime**: UptimeRobot
- **Error Tracking**: Sentry

### Key Metrics to Track

1. **Performance**
   - App launch time
   - Photo capture time
   - PDF generation time
   - Memory usage

2. **Reliability**
   - Crash rate
   - Data loss incidents
   - Sync success rate

3. **Usage**
   - Daily/Monthly active users
   - Inspections per user
   - Photos per inspection
   - PDF generations

4. **Business**
   - User retention
   - Conversion rate
   - Average revenue per user

## Scalability Considerations

### Current Capacity (Phase 1)

- Device storage is limiting factor
- Typical iPhone: 64-256GB
- 100 inspections × 50 photos × 500KB = ~2.5GB

### Future Scaling (Phase 3+)

**Storage:**
- Cloudflare R2: Unlimited scalability
- Cost: ~$0.015/GB/month
- 1000 users × 10GB = 10TB = $150/month

**Database:**
- Firestore: Auto-scaling
- 100K operations/day free
- Beyond: $0.06 per 100K reads

**API Server:**
- Railway/Render: Horizontal scaling
- Auto-scale based on CPU/memory
- Add instances as needed

---

**Document Version:** 1.0  
**Last Updated:** October 26, 2025  
**Status:** Phase 1 - Foundation

