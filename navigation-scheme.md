# Mintlify Navigation Scheme

This document outlines the recommended navigation structure for the HomeLens Web documentation in Mintlify.

## Navigation Structure

The documentation is organized into logical groups with tabs for different sections:

### Tabs (Top Level)

1. **Web App** (default) - Main web application documentation
2. **Mobile Apps** - iOS and Android documentation (future)
3. **API** - API reference and integration guides

### Navigation Groups

#### 1. Getting Started

**Purpose**: Introduction and setup for new users

- Introduction
- Getting Started
- Architecture

**Description**: These pages help users understand what HomeLens is and how to set it up.

#### 2. Core Features

**Purpose**: Main application features

- Dashboard
- Inspections
- Templates
- Services
- Calendar
- Contacts

**Description**: Core functionality documentation for daily use.

#### 3. Client Features

**Purpose**: Features for client-facing functionality

- Client Portal
- Widgets

**Description**: Documentation for sharing reports and booking widgets.

#### 4. Configuration

**Purpose**: Settings and integrations

- Settings
- Integrations
- Authentication

**Description**: Configuration and customization options.

#### 5. Technical

**Purpose**: Technical documentation for developers

- Firestore Structure
- API Reference
- Development

**Description**: Technical details for developers and integrations.

## Full Navigation Configuration

```json
{
  "navigation": [
    {
      "group": "Getting Started",
      "pages": [
        "introduction",
        "getting-started",
        "architecture"
      ]
    },
    {
      "group": "Core Features",
      "pages": [
        "dashboard",
        "inspections",
        "templates",
        "services",
        "calendar",
        "contacts"
      ]
    },
    {
      "group": "Client Features",
      "pages": [
        "client-portal",
        "widgets"
      ]
    },
    {
      "group": "Configuration",
      "pages": [
        "settings",
        "integrations",
        "authentication"
      ]
    },
    {
      "group": "Technical",
      "pages": [
        "firestore-structure",
        "api-reference",
        "development"
      ]
    }
  ]
}
```

## Page File Structure

All pages should be in the `docs/` directory with `.mdx` extension:

```
docs/
├── introduction.mdx
├── getting-started.mdx
├── architecture.mdx
├── dashboard.mdx
├── inspections.mdx
├── templates.mdx
├── services.mdx
├── calendar.mdx
├── contacts.mdx
├── client-portal.mdx
├── widgets.mdx
├── settings.mdx
├── integrations.mdx
├── authentication.mdx
├── firestore-structure.mdx
├── api-reference.mdx
└── development.mdx
```

## Recommended Tabs Configuration

```json
{
  "tabs": [
    {
      "name": "Web App",
      "url": "web-app"
    },
    {
      "name": "Mobile Apps",
      "url": "mobile-apps"
    },
    {
      "name": "API",
      "url": "api"
    }
  ]
}
```

## Additional Configuration

### Topbar Links

```json
{
  "topbarLinks": [
    {
      "name": "GitHub",
      "url": "https://github.com/yourorg/swiftinspect"
    },
    {
      "name": "Dashboard",
      "url": "https://app.swiftinspect.com"
    }
  ]
}
```

### Colors

```json
{
  "colors": {
    "primary": "#1E40AF",
    "light": "#3B82F6",
    "dark": "#1E3A8A"
  }
}
```

## Implementation Notes

1. **File Names**: Use kebab-case for file names (e.g., `getting-started.mdx`)
2. **Frontmatter**: Each MDX file should have frontmatter with `title` and `description`
3. **Grouping**: Related pages should be grouped together
4. **Ordering**: Pages within groups should follow logical flow
5. **Tabs**: Use tabs to separate different documentation types

## Future Expansions

### Additional Groups (Future)

- **Mobile Apps**: iOS and Android documentation
- **Integrations**: Detailed integration guides
- **Troubleshooting**: Common issues and solutions
- **Examples**: Code examples and tutorials

### Additional Pages (Future)

- Stripe Setup Guide
- Google Calendar Setup Guide
- Communication Templates Guide
- PDF Generation Guide
- Report Customization Guide

## Mintlify Configuration File

The complete `mint.json` or `mintlify.config.json` should include:

- Navigation structure
- Tabs configuration
- Colors and branding
- Topbar links
- Footer social links
- Logo and favicon

See `mintlify.config.json` for the complete configuration file.

