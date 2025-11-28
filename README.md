# Abhishek Bakshi - Portfolio Website

A professional portfolio website built with Hugo and the Toha theme, showcasing experience, skills, projects, and achievements.

## 🌐 Live Site

- **Production**: [abhishekbakshi.netlify.app](https://abhishekbakshi.netlify.app/)
- **GitHub**: [abhishekbakshi.github.io](https://github.com/bakshiabhi/abhishekbakshi.github.io)

## 🛠️ Technology Stack

- **Static Site Generator**: [Hugo](https://gohugo.io/) v0.111.3 (Extended)
- **Theme**: [Toha](https://github.com/hossainemruz/toha) (via git submodule)
- **Deployment**: [Netlify](https://www.netlify.com/)
- **Node.js**: v18
- **Frontend**: Bootstrap, Font Awesome, jQuery
- **Features**: Dark mode, multilingual support, blog posts, responsive design

## 📁 Project Structure

```
abhishekbakshi.github.io/
├── archetypes/          # Content templates
├── assets/              # Source images and SCSS
│   └── images/
│       ├── author/      # Author profile images
│       └── site/        # Site images (background, etc.)
├── content/             # Markdown content for blog posts
│   └── posts/
├── data/                # Site data and configuration
│   └── en/
│       ├── author.yaml           # Author information
│       ├── site.yaml             # Site metadata
│       └── sections/
│           ├── about.yaml        # About section config
│           ├── experiences.yaml  # Work experience
│           ├── education.yaml    # Education history
│           ├── skills.yaml       # Technical skills
│           ├── projects.yaml     # Portfolio projects
│           └── achievements.yaml # Certifications & achievements
├── layouts/             # Custom template overrides
│   └── partials/
│       └── sections/
│           └── about.html        # Customized about section template
├── static/              # Static files served as-is
│   ├── files/           # Downloadable files (resume PDF)
│   └── images/          # Public images
│       └── site/        # Site images including profile picture
├── themes/              # Hugo themes
│   └── toha/            # Toha theme (git submodule)
├── config.yaml          # Main Hugo configuration
├── netlify.toml         # Netlify build configuration
└── package.json         # Node.js dependencies
```

## 🔧 Customizations

### Layout Overrides

This site uses Hugo's template override system to customize the Toha theme without modifying the theme files directly:

- **`layouts/partials/sections/about.html`**: Custom about section template
  - Displays profile picture below resume button
  - Adds "← Click to download" tooltip with arrow
  - Supports image rendering in about section

### Custom Features Added

1. **Profile Picture in About Section**
   - Image configured in `data/en/sections/about.yaml`
   - Rendered below the resume button
   - Responsive sizing with Bootstrap classes

2. **Enhanced Resume Button**
   - Tooltip on hover: "Click to download"
   - Visible text hint with arrow: "← Click to download"
   - Points to the resume button for better UX

## 🚀 Local Development

### Prerequisites

- Hugo Extended v0.111.3 or later
- Node.js v18 or later
- Git

### Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/bakshiabhi/abhishekbakshi.github.io.git
   cd abhishekbakshi.github.io
   ```

2. **Initialize submodules**:
   ```bash
   git submodule update --init --recursive
   ```

3. **Install dependencies**:
   ```bash
   npm install
   ```

4. **Run the development server**:
   ```bash
   hugo server -D
   ```

5. **View the site**:
   - Open [http://localhost:1313/](http://localhost:1313/)
   - Changes auto-reload with hot reloading

### Development Tips

- **Draft posts**: Use `-D` flag to show draft content
- **Fast render**: Remove `--disableFastRender` for faster rebuilds (may miss some changes)
- **Full rebuild**: Use `hugo server -D --disableFastRender` for complete rebuilds

## 📤 Deployment

### Netlify Configuration

The site automatically deploys to Netlify when changes are pushed to the `source` branch.

**Build Settings** (defined in `netlify.toml`):
- **Build command**: `hugo mod tidy && hugo mod npm pack && npm install && hugo --minify`
- **Publish directory**: `public`
- **Hugo version**: 0.111.3
- **Node version**: 18

### Deployment Workflow

1. Make changes locally
2. Test with `hugo server -D`
3. Commit changes: `git commit -m "Description"`
4. Push to GitHub: `git push origin source`
5. Netlify automatically:
   - Detects the push
   - Clones the repository
   - Initializes submodules
   - Runs the build command
   - Publishes to production

**Build time**: Typically 1-2 minutes

## 📝 Content Management

### Adding Content

1. **Author Information**: Edit `data/en/author.yaml`
2. **About Section**: Edit `data/en/sections/about.yaml`
3. **Experience**: Edit `data/en/sections/experiences.yaml`
4. **Skills**: Edit `data/en/sections/skills.yaml`
5. **Projects**: Edit `data/en/sections/projects.yaml`
6. **Blog Posts**: Add markdown files to `content/posts/`

### Adding Images

- **Static images** (served as-is): Place in `static/images/`
- **Processed images** (Hugo resources): Place in `assets/images/`
- **Profile pictures**: `static/images/site/` or `assets/images/author/`

### Resume/CV

- Place PDF in `static/files/resume.pdf`
- Update path in `data/en/sections/about.yaml`

## ⚠️ Limitations & Known Issues

### Theme Customization

1. **Submodule Constraint**: The Toha theme is a git submodule pointing to the original repository
   - Cannot modify theme files directly and push them
   - **Solution**: Use Hugo's layout override system (files in `layouts/` override theme files)
   - Custom templates in `layouts/` take precedence over theme templates

2. **Theme Updates**: Updating the theme submodule may overwrite customizations
   - **Mitigation**: All customizations are in `layouts/` folder, which is preserved

### Image Handling

3. **Dual Image Locations**: Profile picture exists in both `assets/` and `static/`
   - `static/images/site/picture.jpg` - Used by the website (served directly)
   - `assets/images/site/picture.jpg` - Source/backup copy
   - **Reason**: Hugo's `relURL` function works with static files for direct serving

### Build Process

4. **Submodule Initialization**: Netlify must initialize submodules during build
   - Currently configured correctly
   - If issues occur, check Netlify build logs for submodule errors

5. **Node Dependencies**: Some npm packages are theme-specific
   - Don't remove packages marked with "toha" in comments
   - Theme requires specific versions for compatibility

### Browser Compatibility

6. **Modern Browsers Only**: Best viewed in recent versions of:
   - Chrome/Edge (latest)
   - Firefox (latest)
   - Safari (latest)

## 🔄 Update Guidelines

### Updating Theme

```bash
cd themes/toha
git pull origin main
cd ../..
git add themes/toha
git commit -m "Update Toha theme"
```

**Warning**: Review any breaking changes in theme updates.

### Updating Dependencies

```bash
npm update
git add package.json package-lock.json
git commit -m "Update npm dependencies"
```

### Adding New Sections

1. Check [Toha theme documentation](https://toha-guides.netlify.app/) for available sections
2. Add YAML configuration to `data/en/sections/`
3. Enable in `config.yaml` if needed

## 🤝 Contributing

This is a personal portfolio site. If you find issues or have suggestions:

1. Open an issue on GitHub
2. Describe the problem or enhancement
3. Include screenshots if relevant

## 📄 License

- **Site Content**: © 2024 Abhishek Bakshi. All rights reserved.
- **Toha Theme**: MIT License - [hossainemruz/toha](https://github.com/hossainemruz/toha)
- **Hugo**: Apache License 2.0

## 🙋 Author

**Abhishek Bakshi**
- Staff Technical Programme Manager @ Meta (Facebook)
- Director @ Alpha Lotus Ltd
- [LinkedIn](https://www.linkedin.com/in/bakshiabhi/)
- [GitHub](https://github.com/bakshiabhi)
- Email: bakshi.abhi@gmail.com

---

**Last Updated**: November 2024

*Built with ❤️ using Hugo and the Toha theme*
