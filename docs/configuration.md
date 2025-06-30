# 🔧 Configuration Reference

This document provides a comprehensive reference for all configuration files and settings used in the Open Guide to Kanban project.

## Hugo Configuration Files

The project uses environment-specific Hugo configuration files to manage different deployment scenarios.

### Main Configuration (`hugo.yaml`)

```yaml
# Base URL for the production site
baseURL: "https://kanbanguides.org"

# Default language
languageCode: "en-us"
defaultContentLanguage: "en"

# Site title
title: "Open Guide to Kanban"

# Theme (empty for custom theme)
theme: ""

# Hugo version compatibility
module:
  hugoVersion:
    extended: true
    min: "0.120.0"

# Language configuration
languages:
  en:
    languageName: "English"
    weight: 1
    title: "Open Guide to Kanban"
    description: "The Open Guide to Kanban is a free, community-driven reference for applying Kanban in knowledge work."
  de:
    languageName: "Deutsch"
    weight: 2
    title: "Offener Leitfaden für Kanban"
    description: "Der Offene Leitfaden für Kanban ist eine kostenlose, von der Community getragene Referenz für die Anwendung von Kanban in der Wissensarbeit."
  es:
    languageName: "Español"
    weight: 3
    title: "Guía Abierta de Kanban"
    description: "La Guía Abierta de Kanban es una referencia gratuita impulsada por la comunidad para aplicar Kanban en el trabajo del conocimiento."
  fr:
    languageName: "Français"
    weight: 4
    title: "Guide Ouvert de Kanban"
    description: "Le Guide Ouvert de Kanban est une référence gratuite pilotée par la communauté pour appliquer Kanban dans le travail de la connaissance."

# Menu configuration
menu:
  main:
    - name: "Guide"
      url: "/guide/"
      weight: 10
    - name: "Creators"
      url: "/creators/"
      weight: 20
    - name: "Download"
      url: "/download/"
      weight: 30

# Parameters for custom variables
params:
  # Site metadata
  description: "An expanded interpretation of the 2020 Scrum Guide"
  author: "Ralph Jocham, John Coleman, Jeff Sutherland"

  # Social media
  github: "https://github.com/KanbanGuides/OpenGuideToKanban"

  # Analytics
  googleAnalytics: "G-XXXXXXXXXX"

  # Features
  enableSearch: true
  enableComments: false
  enableToc: true

  # PDF settings
  enablePdfDownload: true
  pdfPath: "/pdf/"

# Markup configuration
markup:
  goldmark:
    renderer:
      unsafe: true
      hardWraps: false
    parser:
      autoHeadingID: true
      autoHeadingIDType: "github"
  highlight:
    style: "github"
    noClasses: false
    codeFences: true
    guessSyntax: true
    lineNos: true

# Build configuration
build:
  writeStats: true
  noJSConfigInAssets: true

# Security configuration
security:
  enableInlineShortcodes: false
  exec:
    allow: ["^dart-sass-embedded$", "^go$", "^npx$", "^postcss$"]

# Output formats
outputs:
  home: ["HTML", "RSS", "JSON"]
  page: ["HTML"]
  section: ["HTML", "RSS"]

# Minification (production only)
minify:
  disableXML: true
  minifyOutput: true

# SEO and performance
enableRobotsTXT: true
enableGitInfo: true
enableEmoji: true

# Related content
related:
  includeNewer: true
  indices:
    - name: "keywords"
      weight: 100
    - name: "tags"
      weight: 80
```

### Environment-Specific Configurations

#### Local Development (`hugo.local.yaml`)

```yaml
baseURL: "http://localhost:1313"
title: "Open Guide to Kanban - Local"

# Development settings
buildDrafts: true
buildFuture: true
buildExpired: true

# Disable external services in local
params:
  googleAnalytics: ""
  enableComments: false

# Fast rebuilds
enableGitInfo: false
minify:
  minifyOutput: false

# Debug settings
log: true
verbose: true
```

#### Preview Environment (`hugo.preview.yaml`)

```yaml
baseURL: "https://agreeable-island-0c966e810-preview.centralus.6.azurestaticapps.net"
title: "Open Guide to Kanban - Preview"

# Preview settings
buildDrafts: true
buildFuture: true

# Different analytics for preview
params:
  googleAnalytics: "G-PREVIEW-ID"

# Disable robots in preview
enableRobotsTXT: false
```

#### Canary Environment (`hugo.canary.yaml`)

```yaml
baseURL: 'https://agreeable-island-0c966e810-{PullRequestId}.centralus.6.azurestaticapps.net'
title: 'Open Guide to Kanban - Canary'

# Canary settings
buildDrafts: true
buildFuture: true

# Separate analytics for canary
params:
  googleAnalytics: 'G-CANARY-ID'

# Enable additional debugging
params:
  enableDebug: true
  showVersionInfo: true

# Disable robots in canary
enableRobotsTXT: false
```

## Azure Static Web Apps Configuration

### Production Configuration (`staticwebapp.config.json`)

```json
{
  "routes": [
    {
      "route": "/",
      "serve": "/index.html",
      "statusCode": 200
    },
    {
      "route": "/guide",
      "serve": "/guide/index.html",
      "statusCode": 200
    },
    {
      "route": "/creators",
      "serve": "/creators/index.html",
      "statusCode": 200
    },
    {
      "route": "/download",
      "serve": "/download/index.html",
      "statusCode": 200
    },
    {
      "route": "/en/*",
      "serve": "/en/index.html",
      "statusCode": 200
    },
    {
      "route": "/de/*",
      "serve": "/de/index.html",
      "statusCode": 200
    },
    {
      "route": "/es/*",
      "serve": "/es/index.html",
      "statusCode": 200
    },
    {
      "route": "/fr/*",
      "serve": "/fr/index.html",
      "statusCode": 200
    }
  ],
  "responseOverrides": {
    "404": {
      "serve": "/404.html",
      "statusCode": 404
    },
    "403": {
      "serve": "/403.html",
      "statusCode": 403
    }
  },
  "mimeTypes": {
    ".pdf": "application/pdf",
    ".json": "application/json",
    ".xml": "application/xml"
  },
  "globalHeaders": {
    "X-Frame-Options": "DENY",
    "X-Content-Type-Options": "nosniff",
    "Referrer-Policy": "strict-origin-when-cross-origin",
    "Cache-Control": "public, max-age=31536000, immutable"
  },
  "navigationFallback": {
    "rewrite": "/index.html",
    "exclude": ["/pdf/*", "/images/*", "/css/*", "/js/*", "*.{css,js,jpg,png,gif,svg,pdf,xml,json}"]
  },
  "trailingSlash": "auto"
}
```

### Preview Configuration (`staticwebapp.config.preview.json`)

```json
{
  "routes": [
    {
      "route": "/*",
      "headers": {
        "X-Robots-Tag": "noindex, nofollow"
      }
    }
  ],
  "responseOverrides": {
    "404": {
      "serve": "/404.html",
      "statusCode": 404
    }
  },
  "mimeTypes": {
    ".pdf": "application/pdf"
  },
  "globalHeaders": {
    "X-Frame-Options": "DENY",
    "X-Content-Type-Options": "nosniff",
    "Cache-Control": "no-cache, no-store, must-revalidate"
  }
}
```

### Canary Configuration (`staticwebapp.config.canary.json`)

```json
{
  "routes": [
    {
      "route": "/*",
      "headers": {
        "X-Robots-Tag": "noindex, nofollow",
        "X-Environment": "canary"
      }
    }
  ],
  "responseOverrides": {
    "404": {
      "serve": "/404.html",
      "statusCode": 404
    }
  },
  "mimeTypes": {
    ".pdf": "application/pdf"
  },
  "globalHeaders": {
    "X-Frame-Options": "DENY",
    "X-Content-Type-Options": "nosniff",
    "Cache-Control": "no-cache, no-store, must-revalidate"
  }
}
```

## Internationalization Configuration

### Translation Files

Translation files are stored in the `i18n/` directory:

#### English (`i18n/en.yaml`)

```yaml
# Navigation
- id: "nav_home"
  translation: "Home"
- id: "nav_guide"
  translation: "Guide"
- id: "nav_creators"
  translation: "Creators"
- id: "nav_download"
  translation: "Download"

# Common elements
- id: "read_more"
  translation: "Read More"
- id: "download_pdf"
  translation: "Download PDF"
- id: "share"
  translation: "Share"
- id: "print"
  translation: "Print"

# Guide sections
- id: "table_of_contents"
  translation: "Table of Contents"
- id: "previous_section"
  translation: "Previous Section"
- id: "next_section"
  translation: "Next Section"

# Footer
- id: "footer_copyright"
  translation: "© 2025 Open Guide to Kanban"
- id: "footer_license"
  translation: "Licensed under CC BY-SA 4.0"
```

#### German (`i18n/de.yaml`)

```yaml
# Navigation
- id: "nav_home"
  translation: "Start"
- id: "nav_guide"
  translation: "Leitfaden"
- id: "nav_creators"
  translation: "Autoren"
- id: "nav_download"
  translation: "Download"

# Common elements
- id: "read_more"
  translation: "Weiterlesen"
- id: "download_pdf"
  translation: "PDF herunterladen"
- id: "share"
  translation: "Teilen"
- id: "print"
  translation: "Drucken"

# Guide sections
- id: "table_of_contents"
  translation: "Inhaltsverzeichnis"
- id: "previous_section"
  translation: "Vorheriger Abschnitt"
- id: "next_section"
  translation: "Nächster Abschnitt"

# Footer
- id: "footer_copyright"
  translation: "© 2025 Scrum Guide Erweiterungspaket"
- id: "footer_license"
  translation: "Lizenziert unter CC BY-SA 4.0"
```

## Content Front Matter Configuration

### Attribution Fields

The guide supports three types of attribution through content front matter: **creators**, **contributors**, and **translators**. These fields are used to display recognition for different types of contributions to the guide.

#### Loading Behavior

- **Creators & Contributors**: Only loaded from the default language (English) page (`site/content/guide/index.md`)
- **Translators**: Only loaded from language-specific pages (`site/content/guide/index.es.md`, `site/content/guide/index.de.md`, etc.)

This design ensures consistent attribution across all languages while allowing language-specific translator recognition.

#### Field Schema

All attribution types support the same field structure:

```yaml
# Required field
name: "Full Name"

# Optional image fields (priority order)
image: "https://direct-url-to-image.jpg" # Highest priority
gravatarHash: "sha256-hash-of-email" # Medium priority
githubUsername: "github-username" # Lowest priority

# Optional profile link
url: "https://profile-or-website-url.com"

# Translator-specific field
language: "es" # Only used in translators section
```

#### Field Priority for Images

The system attempts to load images in this priority order:

1. **`image`** - Direct URL to profile image (highest priority)
2. **`gravatarHash`** - SHA256 hash for Gravatar service
3. **`githubUsername`** - GitHub username for GitHub avatar API
4. **Default avatar** - System fallback if none available

#### Example Configuration

**Default Language (`site/content/guide/index.md`)**:

```yaml
---
title: Open Guide to Kanban
# ... other front matter fields ...

creators:
  - name: John Coleman
    image: https://media.linkedin.com/dms/image/v2/D4E03AQGlxycsyUPltg/profile-displayphoto-shrink_800_800/profile-displayphoto-shrink_800_800/0/1676027893027
    url: https://www.linkedin.com/in/johnanthonycoleman/

contributors:
  - name: Martin Hinshelwood
    gravatarHash: a9a55b4384e0420e376f441384d0c13fdadb9d39e72892ac60c3e89c3079d10d
    githubUsername: mrhinsh
    url: https://www.linkedin.com/in/martinhinshelwood/
  - name: Jim Benson
    url: https://www.linkedin.com/in/jimbenson/
  - name: Magdalena Firlit
---
```

**Translated Language (`site/content/guide/index.es.md`)**:

```yaml
---
title: Guía Abierta de Kanban
# ... other localized front matter fields ...

# Do NOT include creators/contributors - loaded from default language

translators:
  - name: María García
    language: es
    gravatarHash: def789ghi012jkl345mno678pqr901stu234vwx567yz
    url: https://www.linkedin.com/in/mariagarcia/
  - name: Carlos Rodriguez
    language: es
    githubUsername: carlosrod
---
```

### Technical Implementation Notes

- **Gravatar Hash Generation**: Use SHA256 hash of lowercase, trimmed email address
- **GitHub Username**: Must match exact GitHub username (case-sensitive)
- **URL Validation**: All URLs should be well-formed and preferably HTTPS
- **Language Consistency**: Translator `language` field should match file's language code

## Git Configuration

### `.gitignore`

```gitignore
# Hugo
/public/
/resources/_gen/
/site/.hugo_build.lock

# OS generated files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# IDE files
.vscode/
.idea/
*.swp
*.swo
*~

# Logs
*.log

# Temporary files
*.tmp
*.temp

# Node.js (if using)
node_modules/
npm-debug.log
yarn-error.log

# Environment files
.env
.env.local
.env.production
```

### `.gitattributes`

```gitattributes
# Handle line endings automatically
* text=auto

# Ensure shell scripts use LF
*.sh text eol=lf

# Ensure Windows scripts use CRLF
*.bat text eol=crlf
*.cmd text eol=crlf
*.ps1 text eol=crlf

# Binary files
*.png binary
*.jpg binary
*.jpeg binary
*.gif binary
*.pdf binary
*.woff binary
*.woff2 binary
*.ttf binary
*.eot binary

# Large files
*.pdf filter=lfs diff=lfs merge=lfs -text
```

## GitHub Actions Configuration

### Main Workflow (`.github/workflows/deploy.yml`)

```yaml
name: Deploy to Azure Static Web Apps

on:
  push:
    branches: [main, preview, canary]
  pull_request:
    branches: [main]
  workflow_dispatch:

env:
  HUGO_VERSION: "0.120.4"

jobs:
  build_and_deploy:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job

    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          lfs: false

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: ${{ env.HUGO_VERSION }}
          extended: true

      - name: Determine Environment
        id: env
        run: |
          if [[ "${{ github.ref }}" == "refs/heads/main" ]]; then
            echo "config=hugo.yaml" >> $GITHUB_OUTPUT
            echo "environment=production" >> $GITHUB_OUTPUT
          elif [[ "${{ github.ref }}" == "refs/heads/preview" ]]; then
            echo "config=hugo.preview.yaml" >> $GITHUB_OUTPUT
            echo "environment=preview" >> $GITHUB_OUTPUT
          elif [[ "${{ github.ref }}" == "refs/heads/canary" ]]; then
            echo "config=hugo.canary.yaml" >> $GITHUB_OUTPUT
            echo "environment=canary" >> $GITHUB_OUTPUT
          else
            echo "config=hugo.yaml" >> $GITHUB_OUTPUT
            echo "environment=production" >> $GITHUB_OUTPUT
          fi

      - name: Build Hugo Site
        run: |
          cd site
          hugo --config ${{ steps.env.outputs.config }} --minify --environment ${{ steps.env.outputs.environment }}

      - name: Deploy to Azure Static Web Apps
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "public"
          api_location: ""
          output_location: ""
          config_file_location: "staticwebapp.config.${{ steps.env.outputs.environment }}.json"

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          action: "close"
```

## Development Configuration

### VS Code Settings (`.vscode/settings.json`)

```json
{
  "files.associations": {
    "*.html": "html",
    "*.yaml": "yaml",
    "*.yml": "yaml"
  },
  "emmet.includeLanguages": {
    "html": "html"
  },
  "hugo.server.renderStaticToDisk": true,
  "markdown.preview.fontSize": 14,
  "markdown.preview.lineHeight": 1.6,
  "files.exclude": {
    "public/": true,
    "resources/": true,
    "site/.hugo_build.lock": true
  }
}
```

### VS Code Extensions (`.vscode/extensions.json`)

```json
{
  "recommendations": ["budparr.language-hugo-vscode", "yzhang.markdown-all-in-one", "redhat.vscode-yaml", "ms-vscode.vscode-json", "esbenp.prettier-vscode", "streetsidesoftware.code-spell-checker"]
}
```

## Configuration Best Practices

### Security Considerations

- **Never commit secrets** to version control
- **Use environment variables** for sensitive data
- **Enable security headers** in Static Web Apps config
- **Regular updates** of dependencies and Hugo version

### Performance Optimization

- **Enable minification** in production
- **Use CDN caching** through Azure Static Web Apps
- **Optimize images** before committing
- **Monitor bundle sizes** and build times

### Maintenance

- **Regular config reviews** for outdated settings
- **Test configurations** in different environments
- **Document changes** when updating configs
- **Backup configurations** before major changes

---

🔙 **Back to**: [Documentation Home](./README.md)  
➡️ **Next**: [Troubleshooting Guide](./troubleshooting.md)
