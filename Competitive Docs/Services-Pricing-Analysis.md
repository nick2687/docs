# Services & Pricing Module: Competitive Analysis & Implementation Plan

## Executive Summary

Based on analysis of Tap Inspect, Spectora, and industry best practices, "Services" are the fundamental building blocks that allow inspectors to:
1. **Define what they offer** (e.g., "Residential Home Inspection", "Commercial Inspection")
2. **Automate pricing** (flat rates, tiered by property size/age/distance, or custom)
3. **Generate reports** automatically using pre-defined templates
4. **Collect agreements** and signatures
5. **Calculate invoices** with proper line items and tax
6. **Block calendar time** for scheduling

This is NOT just a pricing calculator—it's the **core business configuration** that drives the entire job workflow.

---

## 🔍 Competitor Deep Dive

### **Tap Inspect** (Screenshots Provided)

#### What They Do Well:
1. **Service as Workflow Orchestrator**
   - Each service encapsulates: Template + Agreement + Pricing + Duration
   - Adding a service to a job triggers: Report creation + Agreement sending + Invoice generation + Calendar blocking

2. **Flexible Pricing Models**
   - **Flat Rate**: Simple CA$300 base price
   - **Tiered Pricing by Property Size**: 
     - 0-2000 sqft = $300
     - 2000-3000 sqft = $450
     - 3000+ sqft = $600
   - **Distance-Based Fees**: 
     - 0-30 km = $0
     - 30-60 km = $50
     - 60+ km = $100
   - **Year Built Fees**: 
     - Pre-1970 = +$100 (older homes need more scrutiny)
     - 1970-1990 = +$50
     - 1990+ = $0

3. **"Add Fee" Extensibility**
   - Can add unlimited pricing variables (square footage, distance, year, etc.)
   - Each variable has min/max ranges and corresponding fees
   - System auto-calculates total based on job details

4. **Service Variations**
   - "Price Depends on Year Built" badge shows dynamic pricing
   - Different services can have completely different pricing structures

#### Their UX Strengths:
- **Clear Information Hierarchy**: Service info → Templates → Agreements → Pricing → Tax
- **Visual Pricing Tables**: Easy-to-scan rows with "When at Least" / "And Less Than" / "Fee" columns
- **Reorderable Rows**: Drag handles (≡) suggest you can reorder pricing tiers
- **Inline Editing**: "+ Add Row" buttons for each fee type
- **Real-Time Preview**: "Reschedule Job" screen shows final calculated price

#### Their UX Weaknesses:
- **Overwhelming for Beginners**: Lots of fields, no guided setup
- **No Pricing Preview**: You have to create a test job to see how pricing calculates
- **Limited Pricing Logic**: Can't do "First 30 km free, then $2/km" (only tiered)

---

### **Spectora** (Based on Research)

#### What They Do:
1. **All-in-One Service Definition**
   - Service name, description, duration
   - Associated templates and agreements
   - Pricing (flat or variable)
   - Tax rates
   - Upsell options (allow clients to add during booking)

2. **"Allow Upsell" Feature**
   - Clients can add ancillary services during online booking (radon, mold, etc.)
   - Increases revenue without inspector having to "sell"

3. **Template Library Integration**
   - Services link to pre-built report templates
   - New inspectors can clone and customize

4. **Automatic Workflow Triggers**
   - Service added → Report generated → Agreement sent → Invoice created → Calendar blocked

#### Their Strengths:
- **Seamless Integration**: Service system deeply integrated with scheduling, invoicing, agreements
- **Client-Facing Booking**: Services visible on public scheduler with prices
- **Modern UI**: Clean, intuitive interface

#### Their Weaknesses:
- **Pricing Flexibility**: Less granular than Tap Inspect (no multi-variable tiers)
- **Complexity**: So many features that setup is overwhelming for new users

---

### **ISN** (Enterprise Focus)

#### What They Do:
1. **Service = Business Logic Unit**
   - Defines pricing, templates, agreements, inspector assignments
   - "Smart Scheduling" auto-assigns best inspector based on service type

2. **Advanced Pricing**
   - Per-inspection fees (variable by volume)
   - FlexFund (pay at closing)
   - Evergreen revenue-sharing

#### Their Strengths:
- **Multi-Inspector Support**: Services can have different inspectors assigned
- **Advanced Analytics**: Track which services are most profitable

#### Their Weaknesses:
- **Enterprise-Only**: Pricing model ($7.25/inspection) doesn't work for solo inspectors
- **Overkill**: Too complex for simple one-person operations

---

## 📊 Pricing Model Patterns in the Industry

### **Common Pricing Variables**:
1. **Property Size** (Square Footage)
   - Most common pricing factor
   - Example: <2000 sqft = $300, 2000-3000 = $450, 3000+ = $600

2. **Property Age** (Year Built)
   - Older homes require more detailed inspection
   - Example: Pre-1970 = +$100, 1970-1990 = +$50, 1990+ = $0

3. **Travel Distance**
   - Compensate for drive time and fuel
   - Example: <30 km = $0, 30-60 km = $50, 60+ = $100

4. **Flat Rate**
   - Simplest model, used by newer inspectors
   - Example: $400 for any residential inspection

5. **Ancillary Services**
   - Add-ons: Radon ($150), Mold ($200), Pool ($100), etc.
   - Can be upsold during booking

### **Pricing Calculation Logic**:
- **Base Price** (required)
- **+ Size Tier** (if applicable)
- **+ Age Fee** (if applicable)
- **+ Distance Fee** (if applicable)
- **+ Add-On Services** (if selected)
- **= Subtotal**
- **+ Tax** (HST/GST)
- **= Total Price**

---

## 🎯 HomeLens Strategy: What to Do the Same vs. Different

### ✅ **What to Keep (Proven Best Practices)**

1. **Service as Workflow Orchestrator**
   - Service = Template + Agreement + Pricing + Duration
   - Adding service to job triggers all automation

2. **Flexible Tiered Pricing**
   - Support square footage, distance, year built, etc.
   - Multiple tiers per pricing variable

3. **Flat Rate Option**
   - Allow simple fixed pricing for beginners
   - Can upgrade to complex pricing later

4. **Template & Agreement Linking**
   - Each service links to specific templates and agreements
   - Auto-generates report and collects signatures

5. **Tax Configuration**
   - Per-service tax rates (HST, GST, provincial tax)

6. **Duration/Time Blocking**
   - Service specifies how long it takes (e.g., 3 hours)
   - Blocks calendar accordingly

---

### 🚀 **What to Improve (Our Competitive Advantages)**

#### **1. Pricing Wizard for Beginners**
**Problem**: Tap Inspect's UI is overwhelming. New inspectors don't know where to start.

**Our Solution**: 
- **Step-by-step wizard** for creating first service
- **"Quick Start" templates**: 
  - "Simple Flat Rate" ($400)
  - "Standard Tiered" (size-based: <2000 sqft / 2000-3000 / 3000+)
  - "Full Custom" (size + distance + age)
- **Visual Price Preview**: Show example calculations as they build pricing

**UX**: 
```
┌─────────────────────────────────────────────┐
│ Let's set up your first service! 🎯         │
├─────────────────────────────────────────────┤
│                                             │
│ What type of pricing do you want?          │
│                                             │
│ ○ Simple Flat Rate                          │
│   → One price for all inspections          │
│   Example: $400 per inspection             │
│                                             │
│ ○ Tiered by Size (Recommended)             │
│   → Price changes based on square footage  │
│   Example: <2000 sqft=$300, 2000+=$450     │
│                                             │
│ ○ Advanced (Size + Distance + Age)         │
│   → Most accurate, but more setup          │
│                                             │
│         [Continue →]                        │
└─────────────────────────────────────────────┘
```

---

#### **2. Live Pricing Calculator**
**Problem**: You have to create a test job to see how pricing works.

**Our Solution**:
- **Real-time preview** on service edit screen
- Slide inputs (sqft, distance, year) and see price update
- "Try it out" section with example properties

**UX**:
```
┌─────────────────────────────────────────────┐
│ Pricing Preview                             │
├─────────────────────────────────────────────┤
│ Example Property:                           │
│ • 2,500 sqft   🏠                           │
│ • Built in 1985 📅                          │
│ • 45 km away   🚗                           │
│                                             │
│ Base Price:           $300.00               │
│ Size (2000-3000):     $150.00               │
│ Age (1970-1990):       $50.00               │
│ Distance (30-60km):    $50.00               │
│ ───────────────────────────────              │
│ Subtotal:            $550.00               │
│ HST (13%):            $71.50               │
│ ───────────────────────────────              │
│ Total:               $621.50 ✓             │
└─────────────────────────────────────────────┘
```

---

#### **3. Smart Defaults & AI Recommendations**
**Problem**: New inspectors don't know what to charge.

**Our Solution**:
- **Market-Based Pricing Suggestions**: 
  - "Based on inspectors in Ontario, typical rates are:"
  - <2000 sqft: $300-400
  - 2000-3000 sqft: $450-550
  - 3000+ sqft: $600-750
- **Auto-Fill from Location**: 
  - Detect region → suggest typical pricing
  - "In Toronto, inspectors typically charge..."

---

#### **4. Pricing Templates Marketplace**
**Problem**: Every inspector reinvents the wheel.

**Our Solution**:
- **Community Pricing Templates**:
  - "Toronto Residential Standard" by @user123
  - "Vancouver Condo Pricing" by @user456
  - "Rural Alberta" by @user789
- **Clone & Customize**: 
  - Start with proven pricing structure
  - Adjust to your market
- **Anonymized Data**: 
  - "87% of inspectors in your region charge $400-500 for 2000 sqft"

---

#### **5. Better Pricing Logic: "Per Unit" Fees**
**Problem**: Tap Inspect only does tiered ranges. Can't do "First 30 km free, then $2/km after".

**Our Solution**:
- **Hybrid Pricing Models**:
  - **Tiered**: 0-2000 sqft = $300, 2000-3000 = $450 (current)
  - **Per-Unit**: Base $300 + $0.10/sqft over 2000 (NEW)
  - **Stepped**: First 30 km free, then $2/km (NEW)

**Example**:
```
Distance Pricing:
├─ First 30 km: Included (free)
├─ 30-60 km: $2.00 per km
└─ 60+ km: $1.50 per km (volume discount)

For 45 km inspection:
• First 30 km: $0
• Next 15 km (30-45): 15 × $2 = $30
• Total Distance Fee: $30
```

---

#### **6. Mobile-First Service Management**
**Problem**: Competitors force you to use desktop for service setup.

**Our Solution**:
- **Full CRUD on mobile**: Create, edit, delete services from iOS/Android
- **Quick Price Adjustments**: Change pricing on-the-go
- **Copy Service**: "Duplicate this service with new pricing"

---

#### **7. Client-Facing Transparency**
**Problem**: Clients don't understand how pricing is calculated.

**Our Solution**:
- **Pricing Breakdown on Quotes**:
  ```
  Residential Home Inspection
  ────────────────────────────
  Base Fee (2,500 sqft):       $450.00
  Property Age Surcharge:       $50.00
  Travel (45 km):              $50.00
  ────────────────────────────
  Subtotal:                   $550.00
  HST (13%):                   $71.50
  ────────────────────────────
  Total:                      $621.50
  ```
- **Transparent Pricing on Booking Page**: 
  - "Enter your property details for instant quote"
  - No hidden fees, no surprises

---

#### **8. Upsell Suggestions**
**Problem**: Spectora has "Allow Upsell" but it's manual.

**Our Solution**:
- **Smart Upsell Recommendations**:
  - "This home was built in 1965. Recommend adding:"
    - ✓ Radon Testing (+$150)
    - ✓ Asbestos Inspection (+$200)
- **One-Click Upsell**: Inspector can add with a tap
- **Agent-Facing Upsells**: Show recommendations to agent during booking

---

#### **9. Service Performance Analytics**
**Problem**: No visibility into which services are most profitable.

**Our Solution**:
- **Service Dashboard**:
  - Most booked service
  - Average revenue per service
  - Time vs. revenue efficiency
  - Upsell conversion rates

---

## 🏗️ Data Model Design

### **Service Entity**:
```typescript
Service {
  id: UUID
  userId: UUID (inspector who owns this)
  
  // Basic Info
  name: string                    // "Residential Home Inspection"
  description: string             // "Full service residential home inspection"
  estimatedDuration: int          // minutes (e.g., 180 = 3 hours)
  isActive: boolean               // can be disabled without deleting
  
  // Pricing Configuration
  pricingType: enum {
    FLAT_RATE,                    // One fixed price
    TIERED,                       // Multiple tiers based on variables
    PER_UNIT,                     // Base + per-unit fee
    CUSTOM                        // Complex formula
  }
  
  basePrice: decimal              // Starting price (required)
  currency: string                // "CAD", "USD"
  
  // Pricing Variables (array of rules)
  pricingRules: [
    {
      variable: enum {            // What drives the price
        SQUARE_FOOTAGE,
        DISTANCE_TRAVELED,
        YEAR_BUILT,
        CUSTOM
      },
      type: enum {                // How it's calculated
        TIERED,                   // Ranges with fixed fees
        PER_UNIT,                 // Per sqft, per km, etc.
        FLAT_ADDITION             // Simple +$X
      },
      tiers: [                    // For TIERED type
        {
          min: int,               // e.g., 0 sqft
          max: int,               // e.g., 2000 sqft
          fee: decimal            // e.g., $0 (included in base)
        },
        {
          min: 2000,
          max: 3000,
          fee: 150.00
        }
      ],
      perUnitRate: decimal,       // For PER_UNIT type (e.g., $0.10/sqft)
      freeUnits: int,             // e.g., first 2000 sqft free
      flatFee: decimal            // For FLAT_ADDITION type
    }
  ],
  
  // Linked Resources
  templateId: UUID?               // Report template to use
  agreementIds: [UUID]            // Legal agreements to collect
  
  // Tax Configuration
  taxRate: decimal                // e.g., 0.13 for 13% HST
  taxName: string                 // "HST", "GST", "Sales Tax"
  
  // Metadata
  createdAt: timestamp
  updatedAt: timestamp
  usageCount: int                 // How many times used in jobs
  totalRevenue: decimal           // Lifetime revenue from this service
}
```

### **PricingCalculation (Helper Functions)**:
```typescript
calculateServicePrice(
  service: Service,
  propertyDetails: {
    squareFootage?: int,
    yearBuilt?: int,
    distanceKm?: int,
    // ... other variables
  }
) => {
  subtotal: decimal,
  breakdown: [
    { label: string, amount: decimal }
  ],
  tax: decimal,
  total: decimal
}
```

---

## 📱 UX Design: Service Screens

### **1. Service List View**
```
┌─────────────────────────────────────────────┐
│ ← Services                            + New  │
├─────────────────────────────────────────────┤
│                                             │
│ 🏠 Residential Home Inspection              │
│    3 hours • $ Varies                       │
│    Used 47 times • $18,800 revenue          │
│    ────────────────────────────────          │
│                                             │
│ 🏢 Commercial Inspection                    │
│    5 hours • $800.00                        │
│    Used 12 times • $9,600 revenue           │
│    ────────────────────────────────          │
│                                             │
│ 🏊 Pool & Spa Inspection                    │
│    1 hour • $150.00                         │
│    Used 8 times • $1,200 revenue            │
│    ────────────────────────────────          │
│                                             │
└─────────────────────────────────────────────┘
```

**Interactions**:
- Tap service → Edit
- Swipe left → Delete / Duplicate / Deactivate
- Long press → Quick actions menu

---

### **2. Create Service Flow (Wizard)**

#### **Step 1: Basic Info**
```
┌─────────────────────────────────────────────┐
│ ← New Service                    Step 1 of 4│
├─────────────────────────────────────────────┤
│                                             │
│ Service Name *                              │
│ [Residential Home Inspection           ]    │
│                                             │
│ Description                                 │
│ [Full service residential inspection   ]    │
│ [including all major systems           ]    │
│                                             │
│ How long does this take?                    │
│ [3] hours [0] minutes                       │
│                                             │
│                  [Continue →]               │
└─────────────────────────────────────────────┘
```

#### **Step 2: Pricing Type**
```
┌─────────────────────────────────────────────┐
│ ← Pricing                        Step 2 of 4│
├─────────────────────────────────────────────┤
│                                             │
│ How do you want to price this service?      │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ ● Simple Flat Rate          EASIEST     │ │
│ │   Same price every time                 │ │
│ │   Example: Always $400                  │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ ○ Tiered by Property Size   POPULAR    │ │
│ │   Price based on square footage         │ │
│ │   Example: <2000 sqft = $300            │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ ○ Advanced Custom          FLEXIBLE     │ │
│ │   Size + Distance + Age                 │ │
│ │   Example: $300 base + size + travel    │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│                  [Continue →]               │
└─────────────────────────────────────────────┘
```

#### **Step 3a: Flat Rate Pricing**
```
┌─────────────────────────────────────────────┐
│ ← Pricing Details                Step 3 of 4│
├─────────────────────────────────────────────┤
│                                             │
│ Set your flat rate price                    │
│                                             │
│ Price                                       │
│ $ [400.00]                                  │
│                                             │
│ 💡 Suggested pricing in your region:        │
│    $350 - $450 (Ontario average)            │
│                                             │
│ Tax Rate                                    │
│ HST (13%) ▼                                 │
│                                             │
│ ────────────────────────────────            │
│ Example: 2,500 sqft home in Toronto        │
│                                             │
│ Service Fee:            $400.00             │
│ HST (13%):               $52.00             │
│ ────────────────────────────────            │
│ Total:                  $452.00             │
│ ────────────────────────────────            │
│                                             │
│                  [Continue →]               │
└─────────────────────────────────────────────┘
```

#### **Step 3b: Tiered Pricing**
```
┌─────────────────────────────────────────────┐
│ ← Size-Based Pricing             Step 3 of 4│
├─────────────────────────────────────────────┤
│                                             │
│ Set pricing tiers by square footage         │
│                                             │
│ ≡ Under 2,000 sqft                          │
│   $ [300.00]                          ✕     │
│                                             │
│ ≡ 2,000 - 3,000 sqft                        │
│   $ [450.00]                          ✕     │
│                                             │
│ ≡ Over 3,000 sqft                           │
│   $ [600.00]                          ✕     │
│                                             │
│         [+ Add Tier]                        │
│                                             │
│ Do you charge extra for travel?             │
│ [  Yes, add distance fees  ]                │
│                                             │
│ Do older homes cost more?                   │
│ [  Yes, charge based on age ]               │
│                                             │
│                  [Continue →]               │
└─────────────────────────────────────────────┘
```

#### **Step 3c: Distance Fees (Optional)**
```
┌─────────────────────────────────────────────┐
│ ← Travel Distance Fees           Step 3b of 4│
├─────────────────────────────────────────────┤
│                                             │
│ How much do you charge for travel?          │
│                                             │
│ ≡ First 30 km                               │
│   $ [0.00] (Free)                     ✕     │
│                                             │
│ ≡ 30 - 60 km                                │
│   $ [50.00]                           ✕     │
│                                             │
│ ≡ Over 60 km                                │
│   $ [100.00]                          ✕     │
│                                             │
│         [+ Add Distance Tier]               │
│                                             │
│ OR                                          │
│                                             │
│ Charge per kilometer:                       │
│ $ [2.00] per km after [30] km               │
│                                             │
│                  [Continue →]               │
└─────────────────────────────────────────────┘
```

#### **Step 4: Templates & Agreements**
```
┌─────────────────────────────────────────────┐
│ ← Templates & Agreements         Step 4 of 4│
├─────────────────────────────────────────────┤
│                                             │
│ Report Template                             │
│ Which template should be used for           │
│ reports created by this service?            │
│                                             │
│ ○ Standard Residential (Recommended)        │
│ ○ Detailed Residential                      │
│ ○ Condo Inspection                          │
│ ○ None (I'll choose later)                  │
│                                             │
│ Agreement                                   │
│ Which agreement should clients sign?        │
│                                             │
│ ☑ Home Inspection Agreement                 │
│ ☐ Additional Terms & Conditions             │
│                                             │
│                  [Create Service ✓]         │
└─────────────────────────────────────────────┘
```

---

### **3. Service Detail / Edit View**
```
┌─────────────────────────────────────────────┐
│ ← Edit Service                       [Save] │
├─────────────────────────────────────────────┤
│                                             │
│ Residential Home Inspection                 │
│ Full service residential inspection         │
│ including all major systems                 │
│                                             │
│ ──────────────────────────────────          │
│                                             │
│ 📋 Basic Info                               │
│ • Duration: 3 hours                         │
│ • Status: Active                            │
│                                             │
│ 💰 Pricing                                  │
│ • Type: Tiered by Size                      │
│ • Base: $300                                │
│ • Tax: HST (13%)                            │
│                                             │
│   Size Tiers:                               │
│   • <2,000 sqft: $300                       │
│   • 2,000-3,000 sqft: $450                  │
│   • 3,000+ sqft: $600                       │
│                                             │
│   Distance Fees:                            │
│   • <30 km: $0                              │
│   • 30-60 km: $50                           │
│   • 60+ km: $100                            │
│                                             │
│ 📄 Templates & Agreements                   │
│ • Template: Standard Residential            │
│ • Agreement: Home Inspection Agreement      │
│                                             │
│ 📊 Performance                              │
│ • Used 47 times                             │
│ • Total Revenue: $18,800                    │
│ • Avg. Price: $400                          │
│                                             │
│ ──────────────────────────────────          │
│                                             │
│ Try It Out 🧪                               │
│ See how pricing calculates for a            │
│ sample property                             │
│                                             │
│ [  Test Pricing Calculator  ]               │
│                                             │
└─────────────────────────────────────────────┘
```

---

### **4. Pricing Calculator (Modal)**
```
┌─────────────────────────────────────────────┐
│                    Pricing Preview      ✕   │
├─────────────────────────────────────────────┤
│                                             │
│ Enter property details:                     │
│                                             │
│ Square Footage                              │
│ ────────●─────────────  2,500 sqft          │
│     1,000      3,000      5,000             │
│                                             │
│ Distance from Office                        │
│ ────────────●─────────  45 km               │
│       10      30      60      90            │
│                                             │
│ Year Built                                  │
│ ──────●───────────────  1985                │
│    1950    1970    1990    2010             │
│                                             │
│ ──────────────────────────────────          │
│                                             │
│ 💵 Price Breakdown:                         │
│                                             │
│ Base Fee:                   $300.00         │
│ Size (2,000-3,000 sqft):    $150.00         │
│ Age Surcharge (1970-1990):   $50.00         │
│ Travel (30-60 km):           $50.00         │
│ ──────────────────────────────────          │
│ Subtotal:                   $550.00         │
│ HST (13%):                   $71.50         │
│ ──────────────────────────────────          │
│ Total:                      $621.50 ✓       │
│                                             │
│             [Use This Quote]                │
└─────────────────────────────────────────────┘
```

---

## 🛠️ Implementation Plan

### **Phase 1: MVP (Week 3-4)**

#### **Core Features**:
1. **Service CRUD**
   - Create, Read, Update, Delete services
   - List view with usage stats
   - Detail/edit view

2. **Pricing Types**:
   - ✅ Flat Rate (simple $X per service)
   - ✅ Tiered by Square Footage (3-5 tiers)
   - ✅ Distance Fees (optional)
   - ❌ Year Built fees (Phase 2)
   - ❌ Per-unit pricing (Phase 2)

3. **Service Configuration**:
   - Name, description, duration
   - Base price + tiers
   - Tax rate (single rate per service)
   - Link to template (optional for MVP)

4. **Pricing Calculator**:
   - Calculate total price given property details
   - Show breakdown of fees
   - Display tax and total

5. **Integration with Inspections**:
   - Add service to inspection
   - Auto-calculate price
   - Store breakdown in inspection

#### **Data Model**:
```swift
// iOS Core Data
Service {
  id: UUID
  userId: UUID (relationship to User)
  name: String
  description: String
  estimatedDuration: Int16  // minutes
  isActive: Bool
  
  // Pricing
  pricingType: String  // "flat", "tiered_size", "tiered_size_distance"
  basePrice: Decimal
  currency: String  // "CAD"
  taxRate: Decimal  // 0.13 for 13%
  taxName: String   // "HST"
  
  // Size Tiers (JSON or separate entity)
  sizeTiers: [SizeTier]  // Transformable or relationship
  distanceTiers: [DistanceTier]?  // Optional
  
  // Linked Resources
  templateId: UUID?
  
  // Metadata
  createdAt: Date
  updatedAt: Date
  usageCount: Int32
  totalRevenue: Decimal
}

SizeTier {
  id: UUID
  service: Service (relationship)
  minSqft: Int32
  maxSqft: Int32?  // nil = no upper limit
  fee: Decimal
  order: Int16  // for sorting
}

DistanceTier {
  id: UUID
  service: Service (relationship)
  minKm: Int32
  maxKm: Int32?
  fee: Decimal
  order: Int16
}
```

#### **UI Screens (iOS)**:
1. `ServiceListView` - List all services
2. `ServiceDetailView` - View/edit service details
3. `ServiceFormView` - Create new service (wizard)
4. `PricingCalculatorView` - Test pricing (modal)
5. `SelectServiceView` - Choose service when creating inspection

#### **Backend (if needed)**:
- Services stored in Firestore (optional for MVP)
- Sync with local Core Data
- For now: local-only is fine

---

### **Phase 2: Enhanced Pricing (Week 5)**

#### **Additional Features**:
1. **Year Built Fees**
   - Add age-based pricing tiers
   - Example: Pre-1970 = +$100

2. **Per-Unit Pricing**
   - Base + $X per sqft over threshold
   - Example: $300 + $0.10/sqft over 2000

3. **Complex Formulas**
   - Combine multiple pricing rules
   - Custom calculation logic

4. **Service Templates**
   - Pre-built service configs
   - "Toronto Residential Standard"
   - Clone and customize

5. **Pricing Suggestions**
   - Market-based recommendations
   - "Inspectors in your region charge..."

---

### **Phase 3: Advanced Features (Week 6+)**

#### **Additional Features**:
1. **Template Linking**
   - Service automatically selects report template
   - Create report from service

2. **Agreement Linking**
   - Service triggers agreement collection
   - Auto-send agreements when service added

3. **Upsell Engine**
   - Suggest add-on services
   - "This home was built in 1965. Recommend radon testing?"

4. **Service Analytics**
   - Most profitable services
   - Time vs. revenue efficiency
   - Conversion rates

5. **Client-Facing Pricing**
   - Show pricing calculator on booking page
   - Instant quote for clients
   - Transparent breakdown

6. **Multi-Currency**
   - Support USD, CAD, etc.
   - Auto-convert based on region

---

## 🎨 Design Principles

### **1. Progressive Disclosure**
- **Beginners**: Simple flat rate → Get started in 60 seconds
- **Intermediate**: Tiered sizing → Most common use case
- **Advanced**: Custom formulas → Full flexibility

### **2. Visual Feedback**
- **Live Price Preview**: See pricing as you build it
- **Example Properties**: "Here's how a 2,500 sqft home would be priced"
- **Validation**: "This tier overlaps with the previous one"

### **3. Smart Defaults**
- **Region-Based Suggestions**: "Inspectors in Ontario typically charge..."
- **Template Library**: Start with proven pricing structures
- **Auto-Fill**: Detect location → suggest typical rates

### **4. Mobile-First**
- **Full CRUD on mobile**: Don't force desktop usage
- **Touch-Friendly**: Large tap targets, swipe gestures
- **Quick Actions**: Duplicate service, adjust pricing

### **5. Transparency**
- **Show the Math**: Always display pricing breakdown
- **Client-Facing**: Clients see how price is calculated
- **No Hidden Fees**: Be upfront about all charges

---

## 🏆 Competitive Advantages Summary

| Feature | Tap Inspect | Spectora | HomeLens |
|---------|------------|----------|--------------|
| **Tiered Pricing** | ✅ | ❌ | ✅ |
| **Distance Fees** | ✅ | ❌ | ✅ |
| **Year Built Fees** | ✅ | ❌ | ✅ (Phase 2) |
| **Per-Unit Pricing** | ❌ | ❌ | ✅ (Phase 2) |
| **Live Price Preview** | ❌ | ❌ | ✅ |
| **Pricing Wizard** | ❌ | ❌ | ✅ |
| **Market Suggestions** | ❌ | ❌ | ✅ (Phase 2) |
| **Mobile CRUD** | ⚠️ (iOS only) | ✅ | ✅ |
| **Template Linking** | ✅ | ✅ | ✅ (Phase 2) |
| **Agreement Linking** | ✅ | ✅ | ✅ (Phase 3) |
| **Service Analytics** | ❌ | ✅ | ✅ (Phase 3) |
| **Client-Facing Pricing** | ❌ | ✅ | ✅ (Phase 3) |
| **Upsell Engine** | ❌ | ✅ | ✅ (Phase 3) |

---

## 📝 Next Steps

### **Immediate (This Week)**:
1. ✅ Review this analysis document
2. ⏳ Create detailed task breakdown for Phase 1
3. ⏳ Design iOS data model (Core Data entities)
4. ⏳ Create UI mockups for service screens
5. ⏳ Implement basic Service CRUD (iOS)

### **Week 3-4 (MVP)**:
1. Build Service List & Detail views
2. Implement Pricing Wizard (flat rate + tiered)
3. Create Pricing Calculator
4. Integrate services with inspections
5. Test with real pricing scenarios

### **Week 5 (Enhanced)**:
1. Add year-built fees
2. Add per-unit pricing
3. Build pricing templates
4. Add market-based suggestions

### **Week 6+ (Advanced)**:
1. Template & agreement linking
2. Upsell engine
3. Service analytics
4. Client-facing pricing calculator

---

## 🤔 Questions for User

1. **Pricing Focus**: Should we prioritize flat rate + size tiers for MVP, or also include distance/age fees?
2. **Market Suggestions**: Do you want AI-powered pricing recommendations, or just static regional averages?
3. **Templates**: Should services link to report templates in Phase 1 (MVP) or Phase 2?
4. **Backend**: Do services need to sync to Firestore, or local-only Core Data for now?
5. **Android**: Should we build iOS first, then port to Android? Or parallel development?

---

**Ready to proceed with implementation?** 🚀

