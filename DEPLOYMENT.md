# Touti Support Website - Azure Static Web Apps Deployment Guide

This guide will walk you through deploying your Touti support website to Azure Static Web Apps and configuring it with your custom domain `touti.meknassi.ca`.

## Prerequisites

- An Azure account (create one at [portal.azure.com](https://portal.azure.com) if you don't have one)
- A GitHub account (for automated deployments)
- Access to your domain DNS settings (for custom domain configuration)

## Project Structure

Your website is organized as follows:

```
Touti_web/
├── public/                    # Website files
│   ├── index.html            # Landing/marketing page
│   ├── support.html          # Support and FAQ page
│   ├── css/
│   │   └── styles.css        # Global styles
│   ├── js/
│   │   └── main.js           # JavaScript for navigation
│   ├── images/               # Card images and logo
│   └── pages/
│       ├── rules.html        # Game rules with card images
│       ├── privacy.html      # Privacy Policy
│       ├── terms.html        # Terms of Service
│       ├── email-verified.html   # Email verification success
│       └── email-error.html      # Email verification error
├── staticwebapp.config.json  # Azure SWA configuration
└── DEPLOYMENT.md            # This file
```

## Step 1: Prepare Your Repository

### Option A: Using GitHub (Recommended)

1. **Create a new GitHub repository:**
   - Go to [github.com](https://github.com) and create a new repository
   - Name it something like `touti-support-website`
   - Make it public or private (your choice)

2. **Push your code to GitHub:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Touti support website"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/touti-support-website.git
   git push -u origin main
   ```

### Option B: Manual Deployment (Alternative)

You can also deploy manually using Azure CLI, but GitHub integration provides automatic updates when you push changes.

## Step 2: Create Azure Static Web App

1. **Sign in to Azure Portal:**
   - Go to [portal.azure.com](https://portal.azure.com)
   - Sign in with your Azure account

2. **Create a new Static Web App:**
   - Click "Create a resource" (+ icon)
   - Search for "Static Web App"
   - Click "Create"

3. **Configure Basic Settings:**
   - **Subscription:** Select your Azure subscription
   - **Resource Group:** Create new or select existing (e.g., "touti-resources")
   - **Name:** Enter a name (e.g., "touti-support")
   - **Plan type:** Select "Free" (sufficient for most needs)
   - **Region:** Choose a region close to your users (e.g., "East US 2", "West Europe")

4. **Configure GitHub Integration:**
   - **Source:** Select "GitHub"
   - Click "Sign in with GitHub" and authorize Azure
   - **Organization:** Select your GitHub username
   - **Repository:** Select your repository
   - **Branch:** Select "main" (or your default branch)

5. **Build Details:**
   - **Build Presets:** Select "Custom"
   - **App location:** `/public`
   - **API location:** Leave empty (no API)
   - **Output location:** Leave empty

6. **Review and Create:**
   - Click "Review + create"
   - Review your settings
   - Click "Create"

7. **Wait for Deployment:**
   - Azure will create the resource and trigger a GitHub Actions workflow
   - This process takes 2-5 minutes
   - You can monitor progress in the "Deployment" section

## Step 3: Verify Your Deployment

1. **Get your Azure URL:**
   - Once deployment completes, go to your Static Web App resource
   - Find the URL (something like `https://blue-wave-123abc.azurestaticapps.net`)
   - Click the URL to verify your site is live

2. **Test all pages:**
   - Home page: `https://your-url.azurestaticapps.net/`
   - Support: `https://your-url.azurestaticapps.net/support.html`
   - Rules: `https://your-url.azurestaticapps.net/pages/rules.html`
   - Privacy: `https://your-url.azurestaticapps.net/pages/privacy.html`
   - Terms: `https://your-url.azurestaticapps.net/pages/terms.html`
   - Email verification pages

## Step 4: Configure Custom Domain (touti.meknassi.ca)

### 4.1 Add Custom Domain in Azure

1. **In Azure Portal:**
   - Go to your Static Web App resource
   - Click "Custom domains" in the left menu
   - Click "+ Add"
   - Select "On any other DNS"

2. **Enter your domain:**
   - Type: Select "CNAME"
   - Domain name: Enter `touti.meknassi.ca`
   - Click "Next"

3. **Note the validation details:**
   - Azure will show you DNS records you need to add
   - Keep this page open for reference

### 4.2 Configure DNS Settings in GoDaddy

You need to add DNS records for your `meknassi.ca` domain in GoDaddy to point the `touti` subdomain to Azure.

1. **Log in to GoDaddy:**
   - Go to [godaddy.com](https://godaddy.com)
   - Sign in to your account
   - Go to "My Products"
   - Find `meknassi.ca` and click "DNS" or "Manage DNS"

2. **Add a CNAME record for the subdomain:**
   - Scroll to the "Records" section
   - Click "Add" or "Add Record"
   - Configure the CNAME record:
     * **Type:** CNAME
     * **Name:** `touti` (GoDaddy will automatically append `.meknassi.ca`)
     * **Value:** Your Azure Static Web App URL (e.g., `blue-wave-123abc.azurestaticapps.net`)
     * **TTL:** 1 Hour (or 3600 seconds) - you can use default
   - Click "Save"

3. **Add TXT record for Azure validation:**
   - Click "Add" again
   - Configure the TXT record:
     * **Type:** TXT
     * **Name:** `_dnsauth.touti` (for validation)
     * **Value:** The validation token shown in Azure Portal (looks like a long random string)
     * **TTL:** 1 Hour
   - Click "Save"

4. **Wait for DNS propagation:**
   - GoDaddy DNS changes typically propagate in 5-30 minutes
   - Can take up to 24 hours in rare cases
   - You can check progress at [whatsmydns.net](https://whatsmydns.net)

**Important GoDaddy Notes:**
- Don't include the full domain in the "Name" field - just use `touti`, not `touti.meknassi.ca`
- Make sure to remove the trailing dot if GoDaddy adds one
- The CNAME value should NOT include `https://` - just the hostname

### 4.3 Validate Domain in Azure

1. **Return to Azure Portal:**
   - Go back to the "Add custom domain" page
   - Click "Add" or "Validate"
   - Azure will verify the DNS records

2. **Wait for validation:**
   - This can take a few minutes
   - Once validated, your domain will be listed as "Validating" → "Ready"

3. **SSL Certificate:**
   - Azure automatically provisions a free SSL certificate
   - This takes 5-10 minutes
   - Your site will be accessible via HTTPS

## Step 5: Configure App Store/Play Store URLs

Once your custom domain is configured, update your app store listings:

### Apple App Store
- **Support URL:** `https://touti.meknassi.ca/support.html`
- **Marketing URL:** `https://touti.meknassi.ca`
- **Privacy Policy URL:** `https://touti.meknassi.ca/pages/privacy.html`

### Google Play Store
- **Website:** `https://touti.meknassi.ca`
- **Email:** `support@touti.meknassi.ca`
- **Privacy Policy URL:** `https://touti.meknassi.ca/pages/privacy.html`

### Email Verification Links
Configure these URLs in your PlayFab/authentication system:
- **Success:** `https://touti.meknassi.ca/pages/email-verified.html`
- **Error:** `https://touti.meknassi.ca/pages/email-error.html`

## Step 6: Future Updates

### Automatic Updates via GitHub

Every time you push changes to your GitHub repository, Azure automatically rebuilds and deploys your site:

```bash
# Make changes to your files
git add .
git commit -m "Update support page"
git push origin main
```

Azure will automatically:
1. Detect the push
2. Build your site
3. Deploy the changes
4. Usually completes in 2-3 minutes

### Manual Updates (if not using GitHub)

If you're using Azure CLI:
```bash
az staticwebapp create \
  --name touti-support \
  --resource-group touti-resources \
  --source ./public \
  --location "eastus2" \
  --branch main
```

## Troubleshooting

### Domain Not Working After DNS Changes

**Problem:** `touti.meknassi.ca` doesn't load after configuring DNS

**Solutions:**
1. Wait longer - DNS can take up to 48 hours
2. Check DNS propagation: Use [dnschecker.org](https://dnschecker.org)
3. Verify CNAME record points to correct Azure URL
4. Clear your browser cache or try incognito mode

### SSL Certificate Not Provisioning

**Problem:** Domain shows "Not Secure" or certificate error

**Solutions:**
1. Wait 10-15 minutes after domain validation
2. Ensure DNS records are correctly configured
3. Try removing and re-adding the custom domain
4. Contact Azure support if issue persists

### Images Not Loading

**Problem:** Card images or logo not displaying

**Solutions:**
1. Check file paths are correct (case-sensitive)
2. Verify images are in `public/images/` folder
3. Check browser console for 404 errors
4. Ensure image file names match references in HTML

### GitHub Actions Build Failing

**Problem:** Deployment fails in GitHub Actions

**Solutions:**
1. Check the Actions tab in your GitHub repository
2. Review build logs for specific errors
3. Verify `staticwebapp.config.json` is in repository root
4. Ensure `public/` folder structure is correct
5. Try triggering a new deployment: Settings → Deployments → Re-run

### 404 Errors on Pages

**Problem:** Sub-pages show 404 errors

**Solutions:**
1. Verify `staticwebapp.config.json` is deployed
2. Check that HTML files are in correct locations
3. Ensure file extensions match links (.html)
4. Review Azure Static Web App logs in portal

## Cost Considerations

### Free Tier Includes:
- 100 GB bandwidth per month
- Custom domains with free SSL
- Global CDN distribution
- Unlimited static web apps (per subscription)
- CI/CD via GitHub Actions

### If You Exceed Free Tier:
- Additional bandwidth: ~$0.15/GB
- Most small support sites stay within free tier

### Monitoring Usage:
1. Go to your Static Web App in Azure Portal
2. Click "Metrics" to see bandwidth usage
3. Set up alerts if approaching limits

## Security Best Practices

Your site includes security headers in `staticwebapp.config.json`:
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `X-XSS-Protection: 1; mode=block`
- `Referrer-Policy: strict-origin-when-cross-origin`

These are automatically applied by Azure Static Web Apps.

## Support and Resources

### Azure Documentation
- [Azure Static Web Apps Docs](https://docs.microsoft.com/azure/static-web-apps/)
- [Custom Domains Guide](https://docs.microsoft.com/azure/static-web-apps/custom-domain)
- [Configuration Reference](https://docs.microsoft.com/azure/static-web-apps/configuration)

### Touti Support
- Email: support@touti.meknassi.ca
- Website: https://touti.meknassi.ca

### Azure Support
- [Azure Support Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
- Free tier includes community support
- Paid support plans available for production apps

## Maintenance Checklist

### Regular Updates
- [ ] Review and update FAQ based on user questions
- [ ] Keep privacy policy and terms current with app changes
- [ ] Update screenshots when UI changes significantly
- [ ] Monitor Azure metrics for unusual traffic patterns

### After Major App Updates
- [ ] Update game rules if gameplay changes
- [ ] Revise privacy policy if data collection changes
- [ ] Update terms if licensing or services change
- [ ] Test all email verification flows

### Periodic Reviews
- [ ] Test all pages on mobile and desktop
- [ ] Verify all links work correctly
- [ ] Check SSL certificate expiry (Azure auto-renews)
- [ ] Review Azure costs and bandwidth usage

## Success Indicators

You'll know deployment is successful when:
- ✅ All pages load correctly at your Azure URL
- ✅ Custom domain (touti.meknassi.ca) resolves and shows HTTPS
- ✅ Images and card graphics display properly
- ✅ Navigation works on mobile and desktop
- ✅ Email verification links can be accessed
- ✅ All footer links work correctly

---

## GoDaddy DNS Quick Reference

For quick copy/paste when adding DNS records in GoDaddy:

### CNAME Record (Subdomain)
```
Type: CNAME
Name: touti
Value: [YOUR-AZURE-URL].azurestaticapps.net
TTL: 1 Hour
```

### TXT Record (Validation)
```
Type: TXT
Name: _dnsauth.touti
Value: [VALIDATION-TOKEN-FROM-AZURE]
TTL: 1 Hour
```

**Where to find these values:**
- Azure URL: In Azure Portal → Your Static Web App → Overview → URL
- Validation Token: In Azure Portal → Custom Domains → Add → Validation section

**Verification Tools:**
- Check DNS propagation: https://whatsmydns.net
- Enter `touti.meknassi.ca` to see if CNAME is propagating globally

---

## Quick Reference

### URLs After Deployment
- **Marketing/Home:** https://touti.meknassi.ca
- **Support:** https://touti.meknassi.ca/support.html
- **Rules:** https://touti.meknassi.ca/pages/rules.html
- **Privacy:** https://touti.meknassi.ca/pages/privacy.html
- **Terms:** https://touti.meknassi.ca/pages/terms.html
- **Email Verified:** https://touti.meknassi.ca/pages/email-verified.html
- **Email Error:** https://touti.meknassi.ca/pages/email-error.html

### Contact Information
- **Support Email:** support@touti.meknassi.ca
- **Domain:** touti.meknassi.ca

---

**Questions?** If you run into any issues during deployment, feel free to reach out to Azure support or consult the Azure Static Web Apps documentation.

Good luck with your deployment! 🎴

