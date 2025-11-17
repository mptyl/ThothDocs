# ThothDocs Documentation Planning

## Asset Cleanup - November 17, 2025

### Completed Tasks
- ✅ Removed unused assets from docs/assets folder

### Deleted Assets
The following unused assets and folders were removed:
- `authentication/` folder (9 PNG files) - not referenced in any documentation
- `installer_images/` folder (installer.png) - not referenced in any documentation  
- `quickstart/` folder (Sidebar-feedback.png) - not referenced in any documentation
- `setup/` folder (9 PNG files) - not referenced in any documentation
- `text_to_speech/` folder (6 markdown files) - not referenced in any documentation
- `ps-e.png` - not referenced in any documentation
- `screenshot-01.png` - not referenced in any documentation
- All `.DS_Store` files throughout the assets directory

### Retained Assets (Currently Used)
- `favicon.ico` - referenced in mkdocs.yml
- `images/0-basics/index_homepage.png` - referenced in index.md
- `images/1-docker_install/` folder (2 PNG files) - referenced in docker installation docs
- `images/4-user-manual/` folder - all files referenced in user manual documentation
- `images/index_pngs/` folder (9 PNG files) - referenced in quickstart documentation

### Summary
Removed approximately 25 unused files and 5 unused folders, significantly reducing the assets directory size while maintaining all currently referenced documentation images.

## Current Task
- Review and update the user manual page `4.1.3.3-agents.md` to reflect the latest frontend `sql_generator` release.
- ✅ Translated all Italian text in `4.1.6.1-sql_dbs.md` to English for consistency across the user manual.
- ✅ Translated all Italian text in `4.1.6.2-sql_tables.md` to English.

## Plan
1. Collect information about changes introduced in the latest frontend `sql_generator` release and identify impacts on agent configuration.
2. Compare the existing documentation with the updated behaviour and UI to detect mismatches or missing details.
3. Update the manual section with clarifications on agent types, new UI elements, escalation logic, and cross-references to other docs if relevant.
4. Proofread and ensure terminology aligns with frontend labels and workflow descriptions.

## Notes
- Keep documentation focused on user-facing behaviour; avoid implementation details.
- Ensure screenshots and labels mentioned remain current; mark any outdated assets for future refresh.
