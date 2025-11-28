# Claude Code - Project Notes

This file contains notes, context, and decisions made while working on this project with Claude Code and other AI assistants.

## 📅 Session History

### Session: 2024-11-28 - Profile Picture & UX Enhancements

**Objective**: Add profile picture to about section with enhanced user experience features.

**Changes Made**:

1. ✅ Added profile picture display below resume button in about section
2. ✅ Implemented download tooltip with directional arrow
3. ✅ Added image zoom/lightbox functionality using Bootstrap modal
4. ✅ Created layout override system to avoid theme modifications
5. ✅ Created comprehensive README.md documentation

**Files Modified**:
- `data/en/sections/about.yaml` - Added `image` field
- `layouts/partials/sections/about.html` - Custom template override
- `static/images/site/picture.jpg` - Profile picture (343KB)
- `assets/images/site/picture.jpg` - Source image backup

**Commits**:
- `961db3a5` - "Add profile picture and download tooltip to about section"

---

## 🎯 Important Context for Future Sessions

### Theme Architecture

This project uses the **Toha theme as a git submodule**. This is critical to understand:

- **DO NOT** modify files in `themes/toha/` directly
- **DO USE** Hugo's layout override system: create files in `layouts/` folder
- Files in `layouts/` automatically override corresponding files in `themes/toha/layouts/`

**Why?**
- The theme is a submodule pointing to the original repository
- Direct modifications would be lost on theme updates
- Can't push changes to the original theme repo (no write access)
- Layout overrides are preserved across theme updates

### Current Overrides

**Active overrides** in this project:

1. **`layouts/partials/sections/about.html`**
   - Overrides: `themes/toha/layouts/partials/sections/about.html`
   - Purpose: Custom about section with image display, tooltips, zoom functionality
   - Features:
     - Profile picture rendering from `about.yaml` image field
     - Resume button with "← Click to download" tooltip
     - Bootstrap modal for image zoom on click
     - Responsive image sizing

### Image Handling

**Dual Image Locations** - This is intentional:

- `static/images/site/picture.jpg` ✅ **Used by website**
  - Hugo serves static files directly
  - No processing needed
  - Fast loading
  - Required for `relURL` to work correctly

- `assets/images/site/picture.jpg` 📦 **Source/backup**
  - Original high-quality source
  - Can be processed by Hugo if needed
  - Backup copy

**Why both?**
- The template uses `{{ .image | relURL }}` which expects files in `static/`
- Keeping source in `assets/` provides backup and allows future processing

### Configuration Files

**Key configuration locations**:

1. **`config.yaml`** - Main Hugo configuration
   - Base URL, site title, theme
   - Language settings
   - Global parameters

2. **`data/en/author.yaml`** - Author profile information
   - Name, greeting, contact info
   - Main profile image (hero section)

3. **`data/en/sections/about.yaml`** - About section configuration
   - Designation, company
   - Resume PDF path
   - **Profile picture** (our addition)
   - Summary text
   - Social links
   - Badges and certifications

4. **`netlify.toml`** - Deployment configuration
   - Build command
   - Hugo version (0.111.3)
   - Node version (18)

---

## 🛠️ Technical Decisions & Rationale

### 1. Layout Override vs Theme Fork

**Decision**: Use layout overrides in `layouts/` folder

**Alternatives Considered**:
- Fork the theme repository
- Remove submodule and commit theme directly
- Modify theme files directly

**Why we chose overrides**:
- ✅ Cleanest solution
- ✅ No submodule management complexity
- ✅ Preserves ability to update theme
- ✅ Standard Hugo best practice
- ✅ Works perfectly with Netlify

### 2. Bootstrap Modal for Image Zoom

**Decision**: Use Bootstrap modal for image zoom functionality

**Alternatives Considered**:
- Third-party lightbox library (Lightbox.js, PhotoSwipe, etc.)
- Simple anchor tag with `target="_blank"`
- Custom CSS-only solution

**Why we chose Bootstrap modal**:
- ✅ Bootstrap already included in theme (no extra dependencies)
- ✅ Built-in accessibility features
- ✅ Responsive and mobile-friendly
- ✅ Familiar UX pattern
- ✅ Zero additional JavaScript needed

### 3. Resume Button Tooltip Enhancement

**Decision**: Add visible text "← Click to download" with arrow

**Rationale**:
- Browser tooltips (title attribute) are not always discoverable
- Visible hint improves UX for all users
- Arrow clearly indicates the clickable element
- Follows accessibility best practices

---

## 🚀 Development Workflow

### Local Development

```bash
# Start Hugo server with full rebuilds
hugo server -D --disableFastRender

# Or with fast render (may miss some changes)
hugo server -D
```

**Port**: http://localhost:1313/

**Auto-reload**: Yes, changes trigger automatic rebuilds

### Making Changes

**For content changes**:
1. Edit YAML files in `data/en/sections/`
2. Hugo auto-detects and rebuilds
3. Browser auto-refreshes

**For template changes**:
1. Create/modify files in `layouts/` (NOT in `themes/toha/`)
2. Follow same directory structure as theme
3. Hugo auto-detects and rebuilds

**For images**:
- Add to `static/images/` for direct serving
- Add to `assets/images/` for Hugo processing
- Reference in YAML as relative path from static: `images/site/filename.jpg`

### Deployment

**Automatic on push**:
```bash
git add .
git commit -m "Description"
git push origin source
```

Netlify automatically:
1. Detects push to `source` branch
2. Clones repo and initializes submodules
3. Runs build: `hugo mod tidy && hugo mod npm pack && npm install && hugo --minify`
4. Publishes `public/` folder
5. Build time: ~1-2 minutes

---

## ⚠️ Gotchas & Common Issues

### 1. Template Changes Not Appearing

**Symptom**: Modified template but changes don't show on site

**Causes**:
- Edited theme file instead of creating override
- Browser cache
- Fast render mode missed the change

**Solutions**:
- Ensure override is in `layouts/`, not `themes/toha/layouts/`
- Hard refresh browser (Cmd/Ctrl + Shift + R)
- Restart Hugo with `--disableFastRender`

### 2. Images Not Loading

**Symptom**: Image shows broken icon or doesn't display

**Causes**:
- Image in wrong folder (assets instead of static)
- Incorrect path in YAML
- File permissions

**Solutions**:
- Verify image is in `static/images/`
- Check path in YAML is relative to static: `images/site/picture.jpg` (no leading slash)
- Ensure file permissions are readable

### 3. Submodule Issues

**Symptom**: Theme not found or outdated

**Causes**:
- Submodule not initialized
- Submodule out of sync

**Solutions**:
```bash
# Initialize submodules
git submodule update --init --recursive

# Update submodule
cd themes/toha
git pull origin main
cd ../..
git add themes/toha
git commit -m "Update theme"
```

### 4. Netlify Build Failures

**Common causes**:
- Hugo version mismatch
- Node version mismatch
- Submodule not initialized
- Missing npm dependencies

**Check**:
- `netlify.toml` has correct versions
- Build logs show submodule initialization
- All theme npm packages present

---

## 🎨 Customization Guidelines

### Adding New Features to About Section

The about section template (`layouts/partials/sections/about.html`) now supports:

1. **Profile picture** via `image` field in `about.yaml`
2. **Custom tooltips** on buttons
3. **Modal zoom** for images

**To add more custom features**:

1. Add data fields to `data/en/sections/about.yaml`
2. Reference in template as `{{ .fieldName }}`
3. Use Bootstrap components (already available)
4. Test locally before pushing

### Bootstrap Components Available

The theme includes Bootstrap 4, so you can use:
- Modals
- Tooltips
- Popovers
- Accordions
- Cards
- And more...

**Documentation**: https://getbootstrap.com/docs/4.6/

### Font Awesome Icons

Theme includes Font Awesome, use in YAML:
```yaml
icon: "fas fa-envelope"  # Solid icons
icon: "fab fa-github"    # Brand icons
icon: "far fa-heart"     # Regular icons
```

**Browse icons**: https://fontawesome.com/v5/search

---

## 📝 Content Structure

### About Section YAML Schema

```yaml
section:
  name: About
  id: about
  enable: true
  weight: 1
  showOnNavbar: true
  template: sections/about.html

designation: "Job Title"

company:
  name: "Company Name"
  url: "https://company.com/"

resume: "files/resume.pdf"

# Custom addition - our enhancement
image: "images/site/picture.jpg"

summary: |
  Multi-line summary text
  Supports markdown

socialLinks:
  - name: Display Name
    icon: "fas fa-icon"
    url: "https://..."

badges:
  - type: certification
    name: "Cert Name"
    url: "https://..."
    badge: "https://image-url"

  - type: soft-skill-indicator
    name: "Skill Name"
    percentage: 85
    color: blue
```

---

## 🔮 Future Enhancements (Ideas)

### Potential Improvements

1. **Image Gallery** - Multiple images in about section with carousel
2. **Video Integration** - Embed intro video
3. **Interactive Timeline** - Enhanced experience/education timeline
4. **Dark Mode Toggle** - Manual theme switcher (theme has dark mode support)
5. **Print Styles** - Optimized print layout for sections
6. **Analytics** - Google Analytics or privacy-friendly alternative
7. **Contact Form** - Integrate with Netlify Forms or Formspree
8. **Blog Search** - Enhanced search for blog posts
9. **Tags/Categories** - Better blog post organization
10. **RSS Feed** - Already available at `/index.xml`, could be promoted

### Technical Debt

None currently. Project structure is clean and follows best practices.

---

## 🤖 Tips for AI Assistants

### When Modifying This Project

1. **Always read this file first** to understand the architecture
2. **Never modify theme files directly** - use layout overrides
3. **Check README.md** for general project info
4. **Test locally** before committing (user runs Hugo server)
5. **Explain trade-offs** when multiple solutions exist
6. **Document decisions** in this file if significant

### Common Tasks

**Adding a new section**:
1. Check Toha theme docs for available sections
2. Create YAML in `data/en/sections/`
3. Configure in `config.yaml` if needed

**Customizing existing section**:
1. Identify template file in `themes/toha/layouts/`
2. Copy to same path in `layouts/` folder
3. Modify the override copy
4. Test locally

**Adding images**:
1. Place in `static/images/` for direct serving
2. Reference in YAML as `images/folder/file.jpg`
3. Consider responsive sizing with `img-fluid` class

**Debugging**:
1. Check Hugo server output for errors
2. Verify file paths are correct
3. Check browser console for JavaScript errors
4. Review Netlify deploy logs if build fails

### Project Philosophy

This is a **personal portfolio site** focused on:
- Professional presentation
- Clean, maintainable code
- Fast loading times
- Accessibility
- Responsive design
- Easy content updates

**Avoid**:
- Over-engineering
- Unnecessary dependencies
- Breaking changes without user consultation
- Modifications that make updates difficult

---

## 📞 Questions?

For questions about this project or these notes, reference:
- **README.md** - General project documentation
- **Toha Theme Docs**: https://toha-guides.netlify.app/
- **Hugo Docs**: https://gohugo.io/documentation/

---

**Last Updated**: 2024-11-28
**Last Session**: Profile picture and UX enhancements
**Next Session**: TBD

*These notes are maintained to provide context for future AI-assisted development sessions.*
