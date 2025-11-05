# Icon System Guide

## Overview

InspectFlow uses a **semantic icon system** to maintain consistency between the web and iOS platforms. Each icon has a unique semantic ID (like `electrical`, `plumbing`, `structure`) that both platforms understand, but each platform renders the icon using its native icon library.

- **Web**: Uses [Lucide Icons](https://lucide.dev/icons/)
- **iOS**: Uses [SF Symbols](https://developer.apple.com/sf-symbols/)

## How It Works

1. **Semantic IDs**: Icons are identified by semantic names (e.g., `electrical`, `plumbing`)
2. **Platform Mapping**: Each platform maps the semantic ID to its best native icon
3. **Firestore Storage**: Only the semantic ID is stored in the database
4. **Cross-Platform Sync**: Templates automatically work on both platforms

## Icon Mapping Table

| Semantic ID | Display Name | Web (Lucide) | iOS (SF Symbol) |
|-------------|--------------|--------------|-----------------|
| `structure` | Structure | `Home` | `house.fill` |
| `exterior-structure` | Exterior | `Building2` | `building.2.fill` |
| `electrical` | Electrical | `Zap` | `bolt.fill` |
| `plumbing` | Plumbing | `Droplet` | `drop.fill` |
| `hvac` | HVAC | `Wind` | `wind` |
| `heating` | Heating | `Flame` | `flame.fill` |
| `tools-general` | Tools | `Wrench` | `wrench.fill` |
| `doors-windows` | Doors/Windows | `DoorOpen` | `door.left.hand.closed` |
| `garage` | Garage | `Warehouse` | `building.fill` |
| `safety` | Safety | `Shield` | `shield.fill` |
| `bedroom` | Bedroom | `Bed` | `bed.double.fill` |
| `bathroom` | Bathroom | `Bath` | `bathtub.fill` |
| `kitchen` | Kitchen | `ChefHat` | `fork.knife` |
| `exterior-landscape` | Landscape | `TreePine` | `tree.fill` |
| `driveway-parking` | Driveway | `Car` | `car.fill` |
| `lighting` | Lighting | `Lightbulb` | `lightbulb.fill` |
| `roofing-material` | Roofing Material | `Square` | `square.fill` |
| `foundation` | Foundation | `HardHat` | `hammer.fill` |
| `documents` | Documents | `FileText` | `doc.text.fill` |
| `roof-standard` | Roof | `RoofIcon` (custom) | `custom.roof.fill` (asset) |
| `roof-simple` | Roof Simple | `RoofSimpleIcon` (custom) | `custom.roof.simple` (asset) |
| `roof-pitched` | Roof Pitched | `RoofPitchedIcon` (custom) | `custom.roof.pitched` (asset) |
| `roof-gable` | Roof Gable | `RoofGableIcon` (custom) | `custom.roof.gable` (asset) |

## Adding New Icons

Follow these steps to add a new icon that works across both platforms:

### 1. Choose a Semantic ID

Pick a descriptive, lowercase, hyphenated name:
- ✅ Good: `attic-space`, `water-heater`, `air-conditioning`
- ❌ Bad: `Attic`, `waterHeater`, `ac`

### 2. Add to Web (TypeScript)

**File**: `web/lib/constants/iconIds.ts`

```typescript
export const ICON_IDS = {
  // ... existing icons
  ATTIC_SPACE: 'attic-space',  // Add your new ID
} as const
```

**File**: `web/components/templates/IconPicker.tsx`

```typescript
// Import the Lucide icon
import { Home, Building2, ..., YourNewIcon } from 'lucide-react'

// Add to SEMANTIC_ICON_MAP
const SEMANTIC_ICON_MAP = {
  // ... existing mappings
  [ICON_IDS.ATTIC_SPACE]: YourNewIcon,
}

// Add to ICONS array
const ICONS = [
  // ... existing icons
  { id: ICON_IDS.ATTIC_SPACE, name: 'Attic', Icon: YourNewIcon }
]
```

### 3. Add to iOS (Swift)

**File**: `ios/Features/Template/Models/IconSource.swift`

```swift
// In semanticId computed property
var semanticId: String {
    switch self {
    // ... existing cases
    case .sfSymbol("house.lodge.fill"):
        return "attic-space"
    // ...
    }
}

// In fromSemanticId function
static func fromSemanticId(_ id: String) -> IconSource? {
    switch id {
    // ... existing cases
    case "attic-space":
        return .sfSymbol("house.lodge.fill")
    // ...
    }
}

// Add to commonIcons array
static let commonIcons: [(IconSource, String)] = [
    // ... existing icons
    (.sfSymbol("house.lodge.fill"), "Attic"),
]
```

### 4. Update This Documentation

Add the new icon to the mapping table above.

### 5. Test Both Platforms

1. **Web**: Restart dev server, open icon picker, verify new icon appears
2. **iOS**: Rebuild app, create section, verify new icon appears
3. **Cross-platform**: Create a section with the new icon on web, verify it appears on iOS (and vice versa)

## Finding Icons

### Web (Lucide)
- Browse: https://lucide.dev/icons/
- Search by name or category
- All icons are in the `lucide-react` package

### iOS (SF Symbols)
- Download: https://developer.apple.com/sf-symbols/
- Browse using SF Symbols app (macOS)
- Over 6,900 symbols available

## Backward Compatibility

The system supports old icon names automatically:

- **Legacy IDs**: Old names like `home`, `zap`, `droplet` are still supported
- **Automatic Migration**: Legacy names are converted to semantic IDs
- **No Data Loss**: Existing templates continue to work

## Troubleshooting

### Icon Not Showing on Web

1. Check if semantic ID is in `ICON_IDS` constant
2. Verify mapping in `SEMANTIC_ICON_MAP`
3. Ensure icon is in `ICONS` array
4. Restart dev server: `npm run dev`
5. Clear browser cache: Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows)

### Icon Not Showing on iOS

1. Check semantic ID in `fromSemanticId` function
2. Verify SF Symbol name is correct (use SF Symbols app)
3. Check if SF Symbol requires specific iOS version
4. Rebuild app in Xcode
5. Check for typos in symbol name

### Icon Shows Square/Generic Icon

- This is the **fallback** behavior when a semantic ID isn't recognized
- Check spelling of semantic ID in Firestore
- Verify mapping exists on the platform you're testing
- Review recent changes to icon mapping files

### Icons Don't Sync Between Platforms

- Verify both platforms use the same semantic ID
- Check Firestore to confirm semantic ID is being stored correctly
- Ensure both platforms have the mapping defined
- Clear app caches and rebuild

## Best Practices

1. **Choose Semantic Names**: Use descriptive, platform-agnostic names
2. **Visual Similarity**: Pick icons that look similar across platforms
3. **Test Both Platforms**: Always test new icons on web and iOS
4. **Document Changes**: Update this file when adding new icons
5. **Consistent Naming**: Use lowercase with hyphens for IDs
6. **Group Related Icons**: Keep similar icons together in code

## Technical Details

### Storage Format

Icons are stored in Firestore as semantic IDs:

```json
{
  "sectionName": "Electrical System",
  "iconName": "electrical"
}
```

### Loading Process

**Web**:
1. Read `iconName` from Firestore (`"electrical"`)
2. Look up in `ICON_MAP` → `Zap` component
3. Render `<Zap />` React component

**iOS**:
1. Read `iconName` from Firestore (`"electrical"`)
2. Call `IconSource.from(storageString: "electrical")`
3. Map to `.sfSymbol("bolt.fill")`
4. Render as `Image(systemName: "bolt.fill")`

### Type Safety

**Web**: TypeScript ensures icon IDs are valid at compile time
**iOS**: Swift's enum and switch statements provide exhaustive checking

## Files Reference

- `web/lib/constants/iconIds.ts` - Semantic ID constants
- `web/components/templates/IconPicker.tsx` - Web icon picker and mappings
- `ios/Features/Template/Models/IconSource.swift` - iOS icon source and mappings
- `docs/Icon-System-Guide.md` - This documentation file

## Questions?

If you encounter issues or have questions about the icon system:

1. Check this documentation first
2. Review the mapping table above
3. Test with a known working icon (e.g., `electrical`)
4. Check Firestore to see what's actually stored
5. Review recent git changes to icon files




