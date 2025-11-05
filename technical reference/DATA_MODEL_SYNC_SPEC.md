# HomeLens Data Model Sync Specification

**Version:** 1.0  
**Last Updated:** November 2, 2025  
**Status:** Active

---

## 1. Executive Summary

This document serves as the source of truth for data model compatibility between HomeLens's mobile (iOS/Swift/CoreData) and web (TypeScript/Firestore) platforms.

### Statistics
- **Total Entities:** 8 core entities
- **Critical Discrepancies:** 4 (require immediate action)
- **High-Priority Issues:** 2 (should be addressed soon)
- **Medium-Priority Issues:** 2 (nice-to-have)
- **Sync Status:** Partially functional (Inspections fixed, Templates working, Services/Photos missing from web)

### Purpose
- Ensure data compatibility between platforms
- Prevent integration breakage when adding new features
- Provide clear guidelines for developers
- Document type conversion rules
- Track discrepancies and their resolution status

---

## 2. Core Type Mapping Guide

### Fundamental Type Conversions

| Mobile (Swift/CoreData) | Firestore (TypeScript) | Notes |
|------------------------|------------------------|-------|
| `UUID` | `string` | Convert using `.uuidString` / `UUID(uuidString:)` |
| `Date` | `Timestamp` / `Date` | Use `Timestamp.fromDate()` / `.toDate()` |
| `NSDecimalNumber` | `number` | Use `.doubleValue` / precision loss possible |
| `String` | `string` | Direct mapping |
| `Bool` | `boolean` | Direct mapping |
| `Int16`, `Int32`, `Int64` | `number` | Check bounds for large numbers |
| `Set<T>` | Array relationship | Convert to IDs array for sync |
| `NSSet` | Array relationship | Convert to IDs array for sync |
| `Data` | Base64 `string` | For small binary data (annotations, etc.) |
| Local file path | Firebase Storage URL | Upload file, store download URL |

### Optional vs Required Fields

**Rule:** If a field is optional in CoreData (`?`), it should be optional in TypeScript (`?:`), unless:
1. It has a default value on creation
2. It's required for core functionality (document separately)
3. It's added later for backward compatibility

---

## 3. Entity Comparison Tables

### 3.1 Inspection Model

**Priority:** 🔴 CRITICAL  
**Sync Status:** ✅ Partially Fixed (scheduledTime/estimatedDuration added to sync service)  
**Collections:** `inspections`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Sync Required | Notes |
|-------|-------------|----------|----------------|-------------|---------------|-------|
| `id` | `UUID?` | `string` | ✅ | ✅ | ✅ | Primary key |
| `userId` | `UUID?` | `string` | ✅ | ✅ | ✅ | Owner reference |
| **Property Information** |
| `propertyAddress` | `String?` | `string` | ✅ | ✅ | ✅ | Required field |
| `propertyCity` | `String?` | `string` | ✅ | ✅ | ✅ | |
| `propertyState` | `String?` | `string` | ✅ | ✅ | ✅ | |
| `propertyZip` | `String?` | `string` | ✅ | ✅ | ✅ | |
| `propertyCountry` | `String?` | - | ✅ | ❌ | ⚠️ | Mobile-only, not synced |
| **Client Information** |
| `clientName` | `String?` | `string` | ✅ | ✅ | ✅ | Required field |
| `clientEmail` | `String?` | `string` | ✅ | ✅ | ✅ | |
| `clientPhone` | `String?` | `string` | ✅ | ✅ | ✅ | |
| **Scheduling** |
| `scheduledDate` | `Date?` | `Date` | ✅ | ✅ | ✅ | Full timestamp on mobile |
| `scheduledTime` | - | `string` | ❌ | ✅ | ✅ | **Added to sync service**, format: "HH:mm" |
| `estimatedDuration` | - | `number` | ❌ | ✅ | ✅ | **Added to sync service**, minutes, default: 180 |
| **Status** |
| `status` | `String?` | `enum` | ✅ | ✅ | ✅ | 'scheduled' \| 'in_progress' \| 'completed' \| 'cancelled' |
| `notes` | `String?` | `string?` | ✅ | ✅ | ✅ | |
| **Service & Pricing** |
| `serviceId` | `UUID?` | `string?` | ✅ | ✅ | ✅ | Reference to Service |
| `serviceName` | `String?` | `string?` | ✅ | ✅ | ✅ | Snapshot of service name |
| `templateId` | `UUID?` | - | ✅ | ❌ | ⚠️ | Mobile tracks template used |
| `basePrice` | `NSDecimalNumber?` | - | ✅ | ❌ | ⚠️ | Pricing breakdown mobile-only |
| `sizeFeeName` | `String?` | - | ✅ | ❌ | ⚠️ | Tier pricing mobile-only |
| `sizeFee` | `NSDecimalNumber?` | - | ✅ | ❌ | ⚠️ | |
| `distanceFeeName` | `String?` | - | ✅ | ❌ | ⚠️ | |
| `distanceFee` | `NSDecimalNumber?` | - | ✅ | ❌ | ⚠️ | |
| `ageFeeName` | `String?` | - | ✅ | ❌ | ⚠️ | |
| `ageFee` | `NSDecimalNumber?` | - | ✅ | ❌ | ⚠️ | |
| `subtotal` | `NSDecimalNumber?` | - | ✅ | ❌ | ⚠️ | |
| `taxRate` | `NSDecimalNumber?` | - | ✅ | ❌ | ⚠️ | |
| `taxAmount` | `NSDecimalNumber?` | - | ✅ | ❌ | ⚠️ | |
| `totalPrice` | `NSDecimalNumber?` | `number?` | ✅ | ✅ | ✅ | Final price |
| **Media** |
| `propertyImagePath` | `String?` | - | ✅ | ❌ | ❌ | Local path, mobile-only |
| `propertyPhoto` | - | `string?` | ❌ | ✅ | ⚠️ | Firebase Storage URL |
| **Location** |
| `latitude` | - | `number?` | ❌ | ✅ | ⚠️ | GPS coordinates |
| `longitude` | - | `number?` | ❌ | ✅ | ⚠️ | GPS coordinates |
| **Calendar Sync (Mobile-Only)** |
| `calendarEventId` | `String?` | - | ✅ | ❌ | ❌ | EventKit identifier |
| `googleCalendarEventId` | `String?` | - | ✅ | ❌ | ❌ | Google Calendar ID |
| `isSyncedToCalendar` | `Bool` | - | ✅ | ❌ | ❌ | Sync status flag |
| `syncError` | `String?` | - | ✅ | ❌ | ❌ | Last error message |
| **Sync Metadata** |
| `syncStatus` | `String?` | - | ✅ | ❌ | ❌ | "synced", "pending", "error" |
| `lastSyncedAt` | `Date?` | - | ✅ | ❌ | ❌ | Mobile tracking |
| `cloudVersion` | `Int64` | - | ✅ | ❌ | ❌ | Conflict resolution |
| `deviceId` | `String?` | - | ✅ | ❌ | ❌ | Source device |
| `cloudURL` | `String?` | - | ✅ | ❌ | ❌ | Firestore path |
| **Timestamps** |
| `createdAt` | `Date?` | `Date` | ✅ | ✅ | ✅ | |
| `updatedAt` | `Date?` | `Date` | ✅ | ✅ | ✅ | |

**Action Items:**
- ✅ DONE: Add `scheduledTime` extraction in mobile sync service
- ✅ DONE: Add `estimatedDuration` default (180 min) in mobile sync service
- 🔲 TODO: Add `estimatedDuration` field to mobile CoreData model for future use
- 🔲 TODO: Consider adding pricing breakdown fields to web if needed for invoicing
- 🔲 TODO: Add `propertyCountry` to web model for international support

---

### 3.2 Template Model

**Priority:** 🟢 LOW  
**Sync Status:** ✅ Working  
**Collections:** `templates`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Sync Required | Notes |
|-------|-------------|----------|----------------|-------------|---------------|-------|
| `id` | `UUID` | `string` | ✅ | ✅ | ✅ | Primary key |
| `userId` | `UUID` | `string` | ✅ | ✅ | ✅ | Owner reference |
| `name` | `String` | `string` | ✅ | ✅ | ✅ | Required |
| `templateDescription` | `String?` | `string?` | ✅ | ✅ | ✅ | Optional |
| `isActive` | `Bool` | `boolean` | ✅ | ✅ | ✅ | Default: true |
| `usageCount` | `Int32` | `number` | ✅ | ✅ | ✅ | Track popularity |
| `createdAt` | `Date` | `Timestamp` | ✅ | ✅ | ✅ | |
| `updatedAt` | `Date` | `Timestamp` | ✅ | ✅ | ✅ | |
| **Relationships** |
| `sections` | `NSSet?` | - | ✅ | ❌ | ❌ | CoreData relationship only |

**Action Items:**
- ✅ No action needed - models are compatible

---

### 3.3 TemplateSection Model

**Priority:** 🟡 MEDIUM  
**Sync Status:** ⚠️ Working but inconsistent  
**Collections:** `templateSections`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Sync Required | Notes |
|-------|-------------|----------|----------------|-------------|---------------|-------|
| `id` | `UUID` | `string` | ✅ | ✅ | ✅ | Primary key |
| `templateId` | - | `string` | ❌ | ✅ | ✅ | **Missing from mobile** |
| `name` | `String` | `string` | ✅ | ✅ | ✅ | Required |
| `sectionDescription` | `String?` | `string?` | ✅ | ✅ | ✅ | Optional |
| `iconName` | `String?` | `string?` | ✅ | ✅ | ✅ | Icon identifier |
| `order` | `Int16` | `number` | ✅ | ✅ | ✅ | Display order |
| `reminders` | `String?` | `string?` | ✅ | ✅ | ✅ | Inspector notes |
| `standardsOfPractice` | `String?` | `string?` | ✅ | ✅ | ✅ | Reference text |
| **Relationships** |
| `template` | `Template?` | - | ✅ | ❌ | ❌ | CoreData relationship |
| `items` | `NSSet?` | - | ✅ | ❌ | ❌ | CoreData relationship |

**Action Items:**
- 🔲 TODO: Add `templateId` field to mobile CoreData model
- 🔲 TODO: Update mobile sync service to populate `templateId` from relationship

---

### 3.4 TemplateItem Model

**Priority:** 🔴 CRITICAL  
**Sync Status:** ⚠️ Major feature gaps  
**Collections:** `templateItems`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Sync Required | Notes |
|-------|-------------|----------|----------------|-------------|---------------|-------|
| `id` | `UUID` | `string` | ✅ | ✅ | ✅ | Primary key |
| `sectionId` | - | `string` | ❌ | ✅ | ✅ | **Missing from mobile** |
| `name` | `String` | `string` | ✅ | ✅ | ✅ | Required |
| `itemDescription` | `String?` | `string?` | ✅ | ✅ | ✅ | Optional |
| `order` | `Int16` | `number` | ✅ | ✅ | ✅ | Display order |
| `reminders` | `String?` | - | ✅ | ❌ | ⚠️ | Mobile-only field |
| `standardsOfPractice` | `String?` | - | ✅ | ❌ | ⚠️ | Mobile-only field |
| **Item Type & Behavior** |
| `itemType` | - | `ItemType?` | ❌ | ✅ | 🔴 | **Missing from mobile** |
| `isRequired` | - | `boolean?` | ❌ | ✅ | 🔴 | **Missing from mobile** |
| `isInformationalOnly` | - | `boolean?` | ❌ | ✅ | 🔴 | **Missing from mobile** |
| `checkboxOptions` | - | `string[]?` | ❌ | ✅ | 🔴 | **Missing from mobile** |
| **Relationships** |
| `section` | `TemplateSection?` | - | ✅ | ❌ | ❌ | CoreData relationship |
| `commentTemplates` | `NSSet?` | - | ✅ | ❌ | ❌ | CoreData relationship |

**Web ItemType Values:**
- `'checkbox'` - Single checkbox item
- `'multipleChoice'` - Multiple choice options
- `'text'` - Free text input
- `'rating'` - Rating scale
- `'note'` - Inspector note only

**Action Items:**
- 🔴 CRITICAL: Add `sectionId` field to mobile CoreData model
- 🔴 CRITICAL: Add `itemType` field to mobile CoreData model
- 🔴 CRITICAL: Add `isRequired` field to mobile CoreData model
- 🔴 CRITICAL: Add `isInformationalOnly` field to mobile CoreData model
- 🔴 CRITICAL: Add `checkboxOptions` field to mobile CoreData model (as Transformable)
- 🔲 TODO: Update mobile sync service to handle new fields
- 🔲 TODO: Consider adding `reminders` and `standardsOfPractice` to web model

---

### 3.5 CommentTemplate Model

**Priority:** 🟠 HIGH  
**Sync Status:** ⚠️ Field name mismatches  
**Collections:** `commentTemplates`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Sync Required | Notes |
|-------|-------------|----------|----------------|-------------|---------------|-------|
| `id` | `UUID` | `string` | ✅ | ✅ | ✅ | Primary key |
| `itemId` | - | `string` | ❌ | ✅ | ✅ | **Missing from mobile** |
| `commentGroup` | `String` | `CommentGroup` | ✅ | ✅ | ✅ | 'information' \| 'limitations' \| 'deficiencies' |
| `title` | - | `string` | ❌ | ✅ | 🔴 | **Web uses 'title'** |
| `commentText` | `String` | - | ✅ | ❌ | 🔴 | **Mobile uses 'commentText'** |
| `defaultText` | `String?` | `string?` | ✅ | ✅ | ✅ | Pre-filled text |
| `commentType` | `String` | `CommentType` | ✅ | ✅ | ✅ | See type values below |
| `checkboxOptions` | `[String]?` | `string[]?` | ✅ | ✅ | ✅ | For multiple choice |
| `allowOther` | `Bool` | - | ✅ | ❌ | ⚠️ | Mobile-only feature |
| `category` | `String?` | - | ✅ | ❌ | ⚠️ | Mobile categorization |
| `order` | `Int16` | `number` | ✅ | ✅ | ✅ | Display order |
| `isDefaultSelected` | `Bool` | `boolean?` | ✅ | ✅ | ✅ | Auto-select flag |
| **Media** |
| `photoURLs` | `[String]?` | `string[]?` | ✅ | ✅ | ✅ | Firebase Storage URLs |
| **Sync Metadata** |
| `firestoreId` | `String?` | - | ✅ | ❌ | ❌ | Mobile tracking |
| `lastSyncedAt` | `Date?` | - | ✅ | ❌ | ❌ | Mobile tracking |
| **Timestamps** |
| `createdAt` | `Date` | `Timestamp?` | ✅ | ✅ | ✅ | |
| `updatedAt` | `Date` | `Timestamp?` | ✅ | ✅ | ✅ | |
| **Relationships** |
| `item` | `TemplateItem?` | - | ✅ | ❌ | ❌ | CoreData relationship |
| `photos` | `Set<Photo>?` | - | ✅ | ❌ | ❌ | CoreData relationship |

**CommentType Values (must match exactly):**
- `'checkbox'` - Single checkbox
- `'multipleChoice'` - Multiple options
- `'date'` - Date picker
- `'number'` - Number input
- `'numericRange'` - Min/max range
- `'signature'` - Signature capture
- `'text'` - Free text
- `'temperature'` - Temperature reading

**Action Items:**
- 🔴 CRITICAL: Rename mobile `commentText` to `title` OR update sync service to map between them
- 🔲 TODO: Add `itemId` field to mobile CoreData model
- 🔲 TODO: Update mobile sync service to handle field name mapping
- 🔲 TODO: Consider adding `allowOther` and `category` to web model

---

### 3.6 Service Model

**Priority:** 🔴 CRITICAL  
**Sync Status:** ❌ Not syncing (web model missing)  
**Collections:** `services`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Notes |
|-------|-------------|----------|----------------|-------------|-------|
| `id` | `UUID` | - | ✅ | ❌ | **Web model doesn't exist** |
| `userId` | `UUID` | - | ✅ | ❌ | |
| `name` | `String` | - | ✅ | ❌ | |
| `serviceDescription` | `String?` | - | ✅ | ❌ | |
| `estimatedDuration` | `Int32` | - | ✅ | ❌ | In minutes |
| `isActive` | `Bool` | - | ✅ | ❌ | |
| `isDefault` | `Bool` | - | ✅ | ❌ | |
| `basePrice` | `NSDecimalNumber` | - | ✅ | ❌ | |
| `currency` | `String` | - | ✅ | ❌ | Default: "USD" |
| `taxRate` | `NSDecimalNumber` | - | ✅ | ❌ | Percentage |
| `taxName` | `String` | - | ✅ | ❌ | e.g., "Sales Tax" |
| `usageCount` | `Int32` | - | ✅ | ❌ | Track popularity |
| `totalRevenue` | `NSDecimalNumber` | - | ✅ | ❌ | Analytics |
| `templateIds` | `[UUID]?` | - | ✅ | ❌ | Associated templates |
| `agreementIds` | `[UUID]?` | - | ✅ | ❌ | Associated agreements |
| `createdAt` | `Date` | - | ✅ | ❌ | |
| `updatedAt` | `Date` | - | ✅ | ❌ | |
| **Relationships** |
| `fees` | `NSSet?` | - | ✅ | ❌ | Size/Distance/Age tiers |

**Action Items:**
- 🔴 CRITICAL: Create `web/lib/firebase/services.ts` with full Service interface
- 🔴 CRITICAL: Add CRUD operations for Service management
- 🔴 CRITICAL: Update Firestore security rules for `services` collection
- 🔲 TODO: Add Service management UI to web app

---

### 3.7 Photo Model

**Priority:** 🔴 CRITICAL  
**Sync Status:** ❌ Not syncing (web model missing)  
**Collections:** `photos`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Notes |
|-------|-------------|----------|----------------|-------------|-------|
| `id` | `UUID?` | - | ✅ | ❌ | **Web model doesn't exist** |
| **Parent References** |
| `inspectionId` | `UUID?` | - | ✅ | ❌ | |
| `reportItemInstanceId` | `UUID?` | - | ✅ | ❌ | |
| `commentId` | `UUID?` | - | ✅ | ❌ | |
| `commentInstanceId` | `UUID?` | - | ✅ | ❌ | |
| **File Information** |
| `fileName` | `String?` | - | ✅ | ❌ | |
| `filePath` | `String?` | - | ✅ | ❌ | Local path |
| `thumbnailPath` | `String?` | - | ✅ | ❌ | Local thumbnail |
| `listThumbnailPath` | `String?` | - | ✅ | ❌ | Small thumbnail |
| **Metadata** |
| `originalWidth` | `Int32` | - | ✅ | ❌ | |
| `originalHeight` | `Int32` | - | ✅ | ❌ | |
| `fileSize` | `Int64` | - | ✅ | ❌ | Bytes |
| **Display** |
| `caption` | `String?` | - | ✅ | ❌ | |
| `order` | `Int16` | - | ✅ | ❌ | |
| **Annotations** |
| `hasAnnotations` | `Bool` | - | ✅ | ❌ | |
| `annotationData` | `Data?` | - | ✅ | ❌ | Binary data |
| **Cloud Sync** |
| `syncStatus` | `String?` | - | ✅ | ❌ | |
| `lastSyncedAt` | `Date?` | - | ✅ | ❌ | |
| `cloudURL` | `String?` | - | ✅ | ❌ | Firebase Storage URL |
| `cloudThumbnailURL` | `String?` | - | ✅ | ❌ | |
| **Timestamps** |
| `createdAt` | `Date?` | - | ✅ | ❌ | |
| `updatedAt` | `Date?` | - | ✅ | ❌ | |
| **Relationships** |
| `inspection` | `Inspection?` | - | ✅ | ❌ | |
| `reportItemInstance` | `ReportItemInstance?` | - | ✅ | ❌ | |
| `reportCommentInstance` | `ReportCommentInstance?` | - | ✅ | ❌ | |
| `commentTemplate` | `CommentTemplate?` | - | ✅ | ❌ | |

**Current Web Implementation:**
- Photos only stored as `photoURLs: string[]` arrays in parent objects
- No centralized Photo entity
- No metadata tracking
- No annotation support

**Action Items:**
- 🔴 CRITICAL: Create `web/lib/firebase/photos.ts` with Photo interface
- 🔴 CRITICAL: Add photo upload/download functions
- 🔴 CRITICAL: Add photo metadata tracking
- 🔲 TODO: Add photo gallery UI to web app
- 🔲 TODO: Consider annotation support in web (future enhancement)

---

### 3.8 User/Profile Model

**Priority:** 🟠 HIGH  
**Sync Status:** ⚠️ Different structures  
**Collections:** `users`

| Field | Mobile Type | Web Type | Mobile Present | Web Present | Sync Required | Notes |
|-------|-------------|----------|----------------|-------------|---------------|-------|
| `id` | `UUID` | `string` | ✅ | ✅ | ✅ | Primary key |
| `email` | `String` | `string` | ✅ | ✅ | ✅ | Required |
| **SECURITY WARNING** |
| `passwordHash` | `String?` | - | ✅ | ❌ | ❌ | **NEVER sync to Firestore!** |
| **Personal Info** |
| `firstName` | - | `string?` | ❌ | ✅ | 🔲 | Web-only |
| `lastName` | - | `string?` | ❌ | ✅ | 🔲 | Web-only |
| `phoneNumber` | `String?` | `string?` | ✅ | ✅ | ✅ | |
| `title` | - | `string?` | ❌ | ✅ | ⚠️ | Web-only (e.g., "Lead Inspector") |
| **Company Info** |
| `companyName` | `String?` | `string?` | ✅ | ✅ | ✅ | |
| `licenseNumber` | `String?` | `string?` | ✅ | ✅ | ✅ | |
| `website` | `String?` | `string?` | ✅ | ✅ | ✅ | |
| **Address** |
| `businessAddress` | `String?` | `string?` | ✅ | ✅ | ✅ | Single line on mobile |
| `address2` | - | `string?` | ❌ | ✅ | ⚠️ | Web has detailed address |
| `city` | - | `string?` | ❌ | ✅ | ⚠️ | |
| `state` | - | `string?` | ❌ | ✅ | ⚠️ | |
| `zipCode` | - | `string?` | ❌ | ✅ | ⚠️ | |
| `country` | - | `string?` | ❌ | ✅ | ⚠️ | |
| **Branding** |
| `logoImagePath` | `String?` | - | ✅ | ❌ | ❌ | Local path (mobile) |
| `logoUrl` | - | `string?` | ❌ | ✅ | ✅ | Firebase Storage URL (web) |
| `profilePhotoPath` | `String?` | - | ✅ | ❌ | ❌ | Local path (mobile) |
| `photoUrl` | - | `string?` | ❌ | ✅ | ✅ | Firebase Storage URL (web) |
| `branding` | - | `object?` | ❌ | ✅ | ⚠️ | Web has color customization |
| `branding.primaryColor` | - | `string` | ❌ | ✅ | ⚠️ | Hex color |
| `branding.accentColor` | - | `string` | ❌ | ✅ | ⚠️ | Hex color |
| **Legal** |
| `tosAcceptedAt` | `Date?` | - | ✅ | ❌ | 🔲 | Terms of Service |
| `privacyAcceptedAt` | `Date?` | - | ✅ | ❌ | 🔲 | Privacy Policy |
| **Timestamps** |
| `createdAt` | `Date` | `any?` | ✅ | ✅ | ✅ | |
| `updatedAt` | `Date` | `any?` | ✅ | ✅ | ✅ | |

**Structural Difference:**
- **Mobile:** Single `User` entity with all fields
- **Web:** `UserProfile` + `CompanyProfile` interfaces (logically separated, stored together)

**Action Items:**
- 🔲 TODO: Add `firstName`/`lastName` to mobile model
- 🔲 TODO: Add detailed address fields to mobile model
- 🔲 TODO: Add `branding` fields to mobile model (or ignore if mobile doesn't need them)
- 🔲 TODO: Add `tosAcceptedAt`/`privacyAcceptedAt` to web model
- 🔲 TODO: Update mobile sync service to handle split UserProfile/CompanyProfile structure
- 🔴 SECURITY: Ensure `passwordHash` is NEVER included in sync operations

---

## 4. Critical Issues Requiring Immediate Action

### 🔴 Issue #1: Inspection Calendar Fields (RESOLVED)
**Status:** ✅ Fixed  
**Impact:** Calendar view couldn't display mobile-created inspections  
**Solution:** Added `scheduledTime` and `estimatedDuration` to mobile sync service  
**Follow-up:** Add `estimatedDuration` field to mobile CoreData model

### 🔴 Issue #2: TemplateItem Missing Fields
**Status:** ❌ Not Fixed  
**Impact:** Mobile app can't sync item types, requirements, or checkbox options  
**Affected Features:**
- Different question types (checkbox vs text vs rating)
- Required field validation
- Multiple choice options
- Informational-only items

**Action Required:**
1. Add fields to mobile CoreData model:
   - `itemType: String?`
   - `isRequired: Bool` (default: false)
   - `isInformationalOnly: Bool` (default: false)
   - `checkboxOptions: [String]?` (Transformable attribute)
2. Update mobile sync service to serialize/deserialize new fields
3. Update mobile UI to support different item types

### 🔴 Issue #3: CommentTemplate Field Name Mismatch
**Status:** ❌ Not Fixed  
**Impact:** Comment titles may not sync correctly between platforms  
**Conflict:** Mobile uses `commentText`, Web uses `title` + `defaultText`  

**Action Required (Choose one):**
- **Option A:** Rename mobile `commentText` → `title`, add `defaultText`
- **Option B:** Map in sync service: mobile `commentText` ↔ web `title`
- **Recommended:** Option A (align with web structure)

### 🔴 Issue #4: Service Model Missing from Web
**Status:** ❌ Not Fixed  
**Impact:** Cannot manage services from web app, inspection creation limited  
**Business Impact:** Web app can't:
- Create/edit services
- Set pricing
- Associate templates with services
- View service analytics

**Action Required:**
1. Create `web/lib/firebase/services.ts` with full Service interface
2. Add Service CRUD functions
3. Add Service management UI (Settings → Services page)
4. Update Firestore security rules
5. Add Service selector to web inspection form

### 🔴 Issue #5: Photo Model Missing from Web
**Status:** ❌ Not Fixed  
**Impact:** Photos only work through parent objects, no centralized management  
**Limitations:**
- No photo metadata tracking
- Can't query photos independently
- No annotation support
- Hard to implement gallery features

**Action Required:**
1. Create `web/lib/firebase/photos.ts` with Photo interface
2. Add photo upload with metadata capture
3. Add photo gallery UI
4. Consider annotation support (future)

---

## 5. Sync Service Implementation Checklist

### Mobile Sync Services

#### InspectionSyncService.swift
- ✅ Extract `scheduledTime` from `scheduledDate`
- ✅ Add default `estimatedDuration` (180 minutes)
- ⬜ Add `propertyCountry` to sync
- ⬜ Add pricing breakdown fields to sync
- ⬜ Handle `templateId` serialization

#### TemplateSyncService.swift
- ✅ Basic template sync working
- ⬜ Add `templateId` to sections when syncing
- ⬜ Verify all template fields sync correctly

#### TemplateItemSyncService.swift (needs creation/update)
- ⬜ Add `sectionId` serialization
- ⬜ Add `itemType` handling
- ⬜ Add `isRequired` handling
- ⬜ Add `isInformationalOnly` handling
- ⬜ Add `checkboxOptions` array conversion

#### CommentTemplateSyncService.swift (needs creation/update)
- ⬜ Map `commentText` ↔ `title` field
- ⬜ Add `itemId` serialization
- ⬜ Handle `photoURLs` vs `photos` relationship

#### ServiceSyncService.swift
- ✅ Service sync exists
- ⬜ Verify all fields syncing correctly
- ⬜ Handle `fees` relationship serialization

#### UserSyncService.swift (needs creation/update)
- ⬜ **CRITICAL:** Exclude `passwordHash` from sync
- ⬜ Handle `UserProfile` + `CompanyProfile` split structure
- ⬜ Map address fields correctly
- ⬜ Handle logo/photo URL conversion

### Web Models Needed

#### services.ts
- ⬜ Create Service interface
- ⬜ Add `getServices()` function
- ⬜ Add `createService()` function
- ⬜ Add `updateService()` function
- ⬜ Add `deleteService()` function
- ⬜ Add fee tier interfaces (SizeTier, DistanceTier, AgeTier)

#### photos.ts
- ⬜ Create Photo interface
- ⬜ Add `uploadPhoto()` function with metadata
- ⬜ Add `getPhotos()` function
- ⬜ Add `deletePhoto()` function
- ⬜ Add thumbnail generation
- ⬜ Add annotation data handling (future)

### Firestore Security Rules

Update `firestore.rules`:
- ✅ `inspections` collection rules added
- ✅ `services` collection rules added
- ⬜ `photos` collection rules needed
- ⬜ Verify all fields are properly validated

---

## 6. Type Conversion Examples

### UUID ↔ String

```swift
// Swift: UUID to String
let idString = inspection.id?.uuidString

// Swift: String to UUID
let uuid = UUID(uuidString: idString)
```

```typescript
// TypeScript: Always use string
const id: string = "123e4567-e89b-12d3-a456-426614174000"
```

### Date ↔ Timestamp

```swift
// Swift: Date to Firestore Timestamp
import FirebaseFirestore

let timestamp = Timestamp(date: inspection.scheduledDate ?? Date())

// Swift: Timestamp to Date
let date = timestamp.toDate()
```

```typescript
// TypeScript: Date to Firestore Timestamp
import { Timestamp } from 'firebase/firestore'

const timestamp = Timestamp.fromDate(new Date())

// TypeScript: Timestamp to Date
const date = timestamp.toDate()
```

### NSDecimalNumber ↔ number

```swift
// Swift: NSDecimalNumber to Double
let priceDouble = inspection.totalPrice?.doubleValue ?? 0.0

// Swift: Double to NSDecimalNumber
inspection.totalPrice = NSDecimalNumber(value: 499.99)
```

```typescript
// TypeScript: Use number
const price: number = 499.99

// Note: JavaScript numbers have precision limitations
// For financial calculations, consider using integers (cents)
```

### Arrays and Sets

```swift
// Swift: Set to Array
let photoURLs = Array(commentTemplate.photos?.map { $0.cloudURL } ?? [])

// Swift: Array to Set
commentTemplate.photos = Set(photos)
```

```typescript
// TypeScript: Always use arrays
const photoURLs: string[] = ["url1", "url2"]
```

---

## 7. Future Development Guidelines

### Adding New Fields

**Process:**
1. Design field on paper first
2. Add to BOTH mobile AND web models simultaneously
3. Update this document with new field
4. Update sync services (both platforms)
5. Write migration code if needed
6. Update Firestore security rules
7. Test sync in both directions
8. Deploy mobile and web together

**Field Naming Convention:**
- Use camelCase on both platforms
- Keep names consistent between mobile/web
- Use clear, descriptive names
- Prefer full words over abbreviations
- Boolean fields start with `is`, `has`, `can`, or `should`

### Data Model Versioning

**Not yet implemented, but recommended:**
- Add `schemaVersion` field to each entity
- Increment when making breaking changes
- Write migration code to handle version differences
- Mobile app can check version and prompt update if needed

### Testing Sync Compatibility

**Before deploying model changes:**
1. Create test account with data on mobile
2. Sync to Firestore
3. Verify data appears correctly in Firestore console
4. View same data in web app
5. Modify data in web app
6. Sync back to mobile
7. Verify changes appear correctly on mobile

### Documentation Requirements

**When adding new entities:**
1. Add to this document immediately
2. Include full field comparison table
3. Document any platform-specific fields
4. Add to sync service checklist
5. Update affected sync services
6. Add code examples if complex

---

## 8. Common Pitfalls and Solutions

### Pitfall #1: Forgetting to Add Foreign Keys

**Problem:** Mobile uses CoreData relationships without explicit IDs  
**Solution:** Always add both relationship AND foreign key field  
**Example:**
```swift
// Add both:
@NSManaged public var template: Template?  // Relationship
@NSManaged public var templateId: UUID?    // Foreign key for Firestore
```

### Pitfall #2: Optional vs Required Mismatches

**Problem:** Mobile field is optional but web requires it  
**Solution:** Use sensible defaults in sync service  
**Example:**
```swift
data["estimatedDuration"] = inspection.estimatedDuration ?? 180
```

### Pitfall #3: Different Field Names

**Problem:** Mobile calls it X, web calls it Y  
**Solution:** Either rename to match, or map in sync service  
**Example:**
```swift
// Map commentText to title
data["title"] = commentTemplate.commentText
data["defaultText"] = commentTemplate.defaultText
```

### Pitfall #4: Syncing Sensitive Data

**Problem:** Mobile has `passwordHash`, must not sync  
**Solution:** Explicitly exclude sensitive fields  
**Example:**
```swift
// NEVER include:
// data["passwordHash"] = user.passwordHash  // ❌ WRONG!

// Only sync public fields:
data["email"] = user.email
data["companyName"] = user.companyName
```

### Pitfall #5: File Paths vs URLs

**Problem:** Mobile stores local paths, web needs URLs  
**Solution:** Upload file to Firebase Storage, store download URL  
**Example:**
```swift
// Mobile stores local path
user.logoImagePath = "/path/to/logo.png"

// In sync service, upload and get URL
let storageRef = Storage.storage().reference().child("logos/\(userId).png")
let downloadURL = try await storageRef.putFile(from: localURL)
data["logoUrl"] = downloadURL.absoluteString  // Store URL for web
```

---

## 9. Change Log

### Version 1.0 - November 2, 2025
- Initial document creation
- Documented all 8 core entities
- Identified critical discrepancies
- Fixed Inspection `scheduledTime` and `estimatedDuration` issues
- Established sync guidelines and processes

### Future Updates
- This document should be updated whenever:
  - New fields are added to any model
  - Sync issues are discovered
  - Platform-specific features are added
  - Migration strategies are implemented

---

## 10. Quick Reference

### Entity Status Summary

| Entity | Sync Status | Priority | Action Needed |
|--------|-------------|----------|---------------|
| Inspection | ⚠️ Partial | 🟢 Low | Minor fields missing |
| Template | ✅ Working | 🟢 Low | None |
| TemplateSection | ⚠️ Partial | 🟡 Medium | Add templateId to mobile |
| TemplateItem | ❌ Critical | 🔴 Critical | Add 5+ fields to mobile |
| CommentTemplate | ⚠️ Partial | 🟠 High | Fix field name mismatch |
| Service | ❌ Missing | 🔴 Critical | Create web model |
| Photo | ❌ Missing | 🔴 Critical | Create web model |
| User/Profile | ⚠️ Partial | 🟠 High | Align structures |

### Platform Contacts

- **Mobile (iOS):** HomeLens Team
- **Web (TypeScript):** HomeLens Team
- **Backend (Firebase):** HomeLens Team
- **This Document:** `docs/DATA_MODEL_SYNC_SPEC.md`

---

**END OF DOCUMENT**

This specification should be treated as a living document and updated whenever data models change on either platform.

