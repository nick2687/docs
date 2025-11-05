# HomeLens Web Documentation

Comprehensive documentation for HomeLens Web application in MDX format, compatible with Mintlify.

## Documentation Structure

### Getting Started
- **introduction.mdx** - Overview of HomeLens Web
- **getting-started.mdx** - Setup and installation guide
- **architecture.mdx** - Technical architecture and design decisions

### Core Features
- **dashboard.mdx** - Dashboard overview and navigation
- **inspections.mdx** - Inspection management
- **templates.mdx** - Template system and management
- **services.mdx** - Service and pricing configuration
- **calendar.mdx** - Calendar views and scheduling
- **contacts.mdx** - CRM contact management

### Client Features
- **client-portal.mdx** - Client portal and report sharing
- **widgets.mdx** - Embeddable scheduling widgets

### Configuration
- **settings.mdx** - Application settings
- **integrations.mdx** - Third-party integrations
- **authentication.mdx** - Authentication system

### Technical
- **firestore-structure.mdx** - Database schema
- **api-reference.mdx** - API endpoint documentation
- **development.mdx** - Development guide

## Mintlify Configuration

The documentation is configured for Mintlify with:

- **Navigation Groups**: Organized into logical sections
- **Tabs**: Web App, Mobile Apps, API
- **Reusable Snippets**: Located in `snippets/` directory

### Configuration File

`mint.json` contains the complete Mintlify configuration including:
- Navigation structure
- Tabs configuration
- Colors and branding
- Topbar links

## Usage

### Local Development

1. Install Mintlify CLI:
```bash
npm install -g mintlify
```

2. Preview documentation:
```bash
mintlify dev
```

3. Build for production:
```bash
mintlify build
```

### Deployment

Deploy to Mintlify:
1. Connect your repository to Mintlify
2. Mintlify will automatically detect `mint.json`
3. Documentation will be available at your Mintlify domain

## Reusable Snippets

Snippets are located in `snippets/` directory:

- **tech-stack.mdx** - Technology stack component and variable

To use snippets in pages:

```mdx
import TechStack from '/snippets/tech-stack.mdx'

<TechStack />
```

## Documentation Features

All documentation pages include:

- **Frontmatter**: Title and description for SEO and navigation
- **Code Examples**: TypeScript/JavaScript examples with syntax highlighting
- **Cross-References**: Links to related documentation
- **Structured Content**: Organized sections with clear headings

## Navigation Scheme

See `navigation-scheme.md` for detailed navigation structure and recommendations.

## Contributing

When adding new documentation:

1. Create `.mdx` file in `docs/` directory
2. Add frontmatter with `title` and `description`
3. Update `mint.json` navigation
4. Use reusable snippets when appropriate
5. Follow existing documentation style

## Related Files

- `mint.json` - Mintlify configuration
- `navigation-scheme.md` - Navigation structure documentation
- `snippets/` - Reusable documentation components

