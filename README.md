# Touti Support Website

A complete static website for the Touti card game, including all pages required for App Store and Google Play Store submissions.

## What's Included

### Pages
- **Landing Page** (`index.html`) - Marketing page with features and download links
- **Support Page** (`support.html`) - FAQ and contact information
- **Game Rules** (`pages/rules.html`) - Comprehensive rules with card images
- **Privacy Policy** (`pages/privacy.html`) - Complete privacy policy for app stores
- **Terms of Service** (`pages/terms.html`) - License terms and EULA
- **Email Verification** - Success and error pages for email verification

### Design Features
- Clean, modern design with card game aesthetic
- Responsive layout (mobile-friendly)
- Card suit colors integrated throughout
- Easy navigation with sticky header
- Professional footer with all links

### Technical Features
- Pure HTML/CSS/JavaScript (no build process needed)
- Optimized for Azure Static Web Apps
- Security headers configured
- SEO-friendly with meta descriptions
- Fast loading with cached assets

## Deployment

See **DEPLOYMENT.md** for complete step-by-step instructions to deploy to Azure Static Web Apps with custom domain `touti.meknassi.ca`.

## URLs for App Stores

Once deployed to touti.meknassi.ca:

- **Support URL:** https://touti.meknassi.ca/support.html
- **Marketing URL:** https://touti.meknassi.ca
- **Privacy Policy:** https://touti.meknassi.ca/pages/privacy.html
- **Terms of Service:** https://touti.meknassi.ca/pages/terms.html

## Contact

**Email:** support@touti.meknassi.ca

## Local Development

To view the site locally:

1. Open the `public` folder in any web server
2. Or simply open `public/index.html` in your browser

For a local server:
```bash
cd public
python -m http.server 8000
# Then visit http://localhost:8000
```

## File Structure

```
public/
├── index.html              # Landing page
├── support.html            # Support and FAQ
├── css/
│   └── styles.css         # All styles
├── js/
│   └── main.js            # Navigation JavaScript
├── images/                # Card images and logo
└── pages/
    ├── rules.html         # Game rules
    ├── privacy.html       # Privacy policy
    ├── terms.html         # Terms of service
    ├── email-verified.html
    └── email-error.html
```

## Making Updates

1. Edit files in the `public/` folder
2. Commit changes to Git
3. Push to GitHub
4. Azure automatically rebuilds and deploys (2-3 minutes)

## License

All content © 2025 Touti. All rights reserved.

