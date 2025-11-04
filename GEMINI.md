# Project Overview

This is a personal portfolio and blog website built with [Hugo](https://gohugo.io/) and the [Toha](https://github.com/hugo-toha/toha) theme. The content is written in Markdown and the site is configured using YAML files. The website is deployed to GitHub Pages using a GitHub Actions workflow.

**Key Technologies:**

*   **Hugo:** A static site generator written in Go.
*   **Toha Theme:** A feature-rich Hugo theme for personal websites and portfolios.
*   **Markdown:** Used for writing content (blog posts, notes, etc.).
*   **YAML:** Used for configuration and data files.
*   **GitHub Actions:** Used for continuous integration and deployment.
*   **npm:** Used for managing development dependencies.

**Architecture:**

*   **`content/`:** Contains the Markdown files for the website's content, such as blog posts and notes.
*   **`data/`:** Contains YAML files with data used throughout the site, such as author information, site metadata, and section content.
*   **`themes/toha/`:** Contains the Hugo theme used for the website.
*   **`config.yaml`:** The main configuration file for the Hugo site.
*   **`.github/workflows/deploy-site.yaml`:** The GitHub Actions workflow for deploying the site to GitHub Pages.
*   **`package.json`:** Defines the development dependencies for the project.

# Building and Running

**Prerequisites:**

*   [Hugo](https://gohugo.io/getting-started/installing/)
*   [Node.js and npm](https://nodejs.org/en/download/)

**Installation:**

1.  Clone the repository:
    ```bash
    git clone --recurse-submodules https://github.com/bakshiabhi/abhishekbakshi.github.io.git
    ```
2.  Install the dependencies:
    ```bash
    npm install
    ```

**Running in Development:**

To run the website locally for development, use the following command:

```bash
hugo server
```

This will start a local server at `http://localhost:1313/`.

**Building for Production:**

To build the website for production, use the following command:

```bash
hugo --minify
```

This will generate the static files in the `public/` directory.

# Development Conventions

*   **Content:** All content is written in Markdown and located in the `content/` directory.
*   **Data:** Site-wide data is stored in YAML files in the `data/en/` directory.
*   **Theme:** The website uses the "toha" theme, which is included as a submodule in the `themes/` directory.
*   **Deployment:** The website is automatically deployed to GitHub Pages when changes are pushed to the `source` branch.
