# Spectora Migration Guide

## Overview

This guide will help you migrate your inspection templates from Spectora to HomeLens. HomeLens supports direct import of Spectora CSV exports, preserving your template structure, comments, and even default photos.

## What Gets Imported

HomeLens imports the following data from Spectora:

### ✅ Fully Supported
- **Template Structure**: Sections, Items, and Comments hierarchy
- **Comment Types**: Information, Limitations, and Deficiencies
- **Comment Text**: Full HTML content (converted to plain text)
- **Answer Types**: Boolean, checkbox, text, number, range, and date fields
- **Multiple Choice Options**: Dropdown and checkbox options
- **Default Photos**: Up to 10 photos per comment with captions
- **Recommendations**: Contractor type suggestions (handyman, roofer, plumber, etc.)
- **Default Values**: Pre-filled answers for inspectors
- **Cost Estimates**: Min/max repair cost estimates
- **Default Locations**: Pre-filled location text
- **Unit Types**: Measurement units (Celsius, gallons, etc.)

### ⚠️ Partially Supported
- **HTML Formatting**: HTML tags are stripped but content is preserved
- **External Photo URLs**: Photos are imported as URLs (download to Firebase Storage coming soon)

### ❌ Not Imported
- **Locked Status**: Comments marked as "locked" in Spectora
- **Simple Format**: Display mode preferences
- **Disable Photos**: Photo restriction settings
- **Uses/Tier**: Subscription tier restrictions
- **Last Modified**: Timestamp metadata

## Step-by-Step Migration

### Step 1: Export from Spectora

1. Log in to your Spectora account
2. Navigate to **Templates** → **My Templates**
3. Select the template you want to export
4. Click **Export** → **CSV Format**
5. Save the CSV file to your computer

> **Note**: Spectora exports include all template data in a single CSV file with 40+ columns.

### Step 2: Import to HomeLens

1. Log in to HomeLens web app
2. Navigate to **Templates** page
3. Click **Import Template** button
4. Drag and drop your Spectora CSV file or click to browse
5. Review the import preview:
   - Number of sections, items, and comments
   - Any validation errors or warnings
6. Click **Import Template**
7. Wait for the import to complete (typically 10-30 seconds)

### Step 3: Review Imported Template

After import, review your template:

1. Open the imported template
2. Check that all sections are present
3. Verify items and comments are correctly organized
4. Review comment text (HTML formatting will be converted to plain text)
5. Check that photos are linked (if any)
6. Test the template with a sample inspection

## Field Mapping Reference

| Spectora Field | HomeLens Field | Notes |
|----------------|-------------------|-------|
| Section Name | Section.name | Direct mapping |
| Item Name | Item.name | Direct mapping |
| Comment Name | Comment.title | Direct mapping |
| Comment Text | Comment.defaultText | HTML stripped to plain text |
| Comment Type | Comment.commentGroup | info→information, limit→limitations, defect→deficiencies |
| Answer Type | Comment.commentType | boolean→checkbox, checkbox→multipleChoice, range→numericRange |
| Multiple Choice Options | Comment.checkboxOptions | Comma-separated list preserved |
| Recommendation | Comment.recommendation | Direct mapping (new field) |
| Default Value | Comment.defaultValue | Direct mapping (new field) |
| Default Value 2 | Comment.defaultValue2 | For range types (new field) |
| Default Unit Type | Comment.defaultUnitType | e.g., "Celsius (C)" (new field) |
| Default Location | Comment.defaultLocation | Direct mapping (new field) |
| Default Estimate Min | Comment.defaultEstimateMin | Parsed as number (new field) |
| Default Estimate Max | Comment.defaultEstimateMax | Parsed as number (new field) |
| Default Photo 1-10 | Comment.photoURLs | Array of URLs (new field) |
| Default Photo Caption 1-10 | Comment.photoCaptions | Array of captions (new field) |
| Order | Comment.order | Preserves display order |

## Common Issues & Solutions

### Issue: Import fails with "Invalid CSV format"

**Solution**: Make sure you're uploading the CSV file directly from Spectora. Don't open and re-save it in Excel, as this can change the format.

### Issue: HTML formatting is lost

**Solution**: This is expected. HomeLens converts HTML to plain text to ensure compatibility across all platforms. Lists are converted to bullet points, and line breaks are preserved.

### Issue: Some comments are missing

**Solution**: Check the validation warnings during import. Comments without a name/title are skipped. You can enable "Skip invalid rows" to import everything else.

### Issue: Photos aren't showing

**Solution**: Currently, photo URLs from Spectora are imported as external links. A future update will download and re-host these photos in Firebase Storage. For now, photos will load from Spectora's servers.

### Issue: Special characters look wrong

**Solution**: Spectora uses HTML entities (e.g., `&amp;` for `&`). These are automatically converted during import. If you see issues, please report them.

## Best Practices

### Before Migration
1. **Review your Spectora template** - Clean up any outdated or unused comments
2. **Export a backup** - Keep a copy of your Spectora export
3. **Test with a small template first** - Try importing a simple template to familiarize yourself with the process

### After Migration
1. **Review thoroughly** - Check all sections, items, and comments
2. **Test with a sample inspection** - Create a test inspection to verify everything works
3. **Update as needed** - Make any necessary adjustments in HomeLens's template editor
4. **Train your team** - Show inspectors the new template structure

### Multiple Templates
If you have multiple templates in Spectora:
1. Export each template separately
2. Import them one at a time
3. Review each before moving to the next
4. Consider consolidating similar templates if possible

## Advanced Features

### Photo Management
After import, you can:
- Add more photos to comments
- Replace external photo URLs with locally uploaded photos
- Add or edit photo captions
- Reorder photos

### Customization
HomeLens offers additional features not available in Spectora:
- **Drag-and-drop reordering** - Easily reorganize sections, items, and comments
- **Icon selection** - Add visual icons to sections
- **Template preview** - See how templates look to inspectors and clients
- **Version control** - Track changes to templates over time

## Getting Help

If you encounter any issues during migration:

1. **Check the validation errors** - The import wizard shows detailed error messages
2. **Review this guide** - Common issues are covered above
3. **Contact support** - Email support@swiftinspect.com with:
   - Your Spectora CSV file (if possible)
   - Screenshots of any error messages
   - Description of the issue

## FAQ

**Q: Can I import multiple templates at once?**  
A: Not currently. Import templates one at a time.

**Q: Will my Spectora subscription be affected?**  
A: No. Exporting from Spectora doesn't affect your account.

**Q: Can I go back to Spectora after migrating?**  
A: Yes. Your Spectora data remains unchanged. You can use both platforms simultaneously.

**Q: How long does import take?**  
A: Most templates import in 10-30 seconds. Large templates (500+ comments) may take up to 2 minutes.

**Q: Are there any limits on template size?**  
A: HomeLens supports templates of any size. We've successfully tested imports with 1000+ comments.

**Q: Can I edit the template after importing?**  
A: Yes! Use HomeLens's template editor to make any changes.

**Q: Will photos be downloaded to my account?**  
A: Currently, photos remain as external URLs. A future update will download and re-host photos in Firebase Storage for better performance and reliability.

## Next Steps

After successfully migrating your templates:

1. **Explore HomeLens features** - Check out the template editor, inspection workflow, and reporting tools
2. **Customize your templates** - Add icons, reorder items, and adjust as needed
3. **Create a test inspection** - Try out your imported template
4. **Train your team** - Schedule a demo or training session
5. **Provide feedback** - Let us know how the migration went and what could be improved

---

**Last Updated**: November 2025  
**Version**: 1.0  
**Contact**: support@swiftinspect.com

