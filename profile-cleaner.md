# Salesforce Profile Cleaner - Usage Tutorial

## Overview
The `clean_all_profiles.py` script helps manage Salesforce profile metadata by automatically commenting out invalid or unwanted sections in profile XML files. It supports multiple section types and can process either specific profiles or all profiles in a directory.

## Features
- Comments out invalid sections while preserving XML structure
- Supports multiple metadata types (tabs, layouts, permissions, etc.)
- Handles existing comments safely
- Processes single files or entire directories
- Preserves original formatting and indentation

## Installation
1. Ensure you have Python 3.6+ installed
2. Save the script as `clean_all_profiles.py` in your project directory

## Basic Usage

### Process all profiles in default directory
```bash
python3 clean_all_profiles.py
```

### Process specific profiles
```bash
python3 clean_all_profiles.py --profiles "Admin.profile-meta.xml" "User.profile-meta.xml"
```

### Custom project root directory
```bash
python3 clean_all_profiles.py --project-root "my-project"
```

## Configuration
Edit the `section_rules` dictionary in the script to configure which items should be commented out:

```python
section_rules = {
    'tabVisibilities': {
        'invalid_items': [
            "standard-UnwantedTab1",
            "standard-UnwantedTab2"
        ],
        'item_pattern': r'<tab>(.+?)</tab>'
    },
    # ... other sections
}
```

## How It Works
1. The script reads each profile XML file
2. For each configured section type:
   - Finds all complete section blocks
   - Checks for invalid items using the specified pattern
   - Comments out entire blocks containing invalid items
3. Preserves:
   - Existing comments
   - Original formatting
   - XML structure

## Output Examples
### Before
```xml
<tabVisibilities>
    <tab>standard-UnwantedTab</tab>
    <visibility>DefaultOn</visibility>
</tabVisibilities>
```

### After
```xml
<!-- <tabVisibilities>
    <tab>standard-UnwantedTab</tab>
    <visibility>DefaultOn</visibility>
</tabVisibilities> -->
```

## Best Practices
1. Always back up your profiles before running the script
2. Test with `--profiles` option first before processing all files
3. Verify changes with `git diff` before committing
4. Add new invalid items to the appropriate section in `section_rules`

## Troubleshooting
- **XML errors after running**: Restore from backup and check for malformed XML in originals
- **Profile not found**: Verify the file exists and has `.profile-meta.xml` extension
- **No changes made**: The profile didn't contain any of the invalid items

## Advanced
- Add new section types by extending `section_rules`
- Modify `item_pattern` for different XML structures
- Combine with version control for easy rollback
