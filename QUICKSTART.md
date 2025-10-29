# Quick Start Guide for Younes

This is your simplified guide to get `touti.meknassi.ca` live on Azure.

## Prerequisites Checklist
- [ ] Azure account (free tier is fine)
- [ ] GitHub account
- [ ] Access to GoDaddy account for meknassi.ca domain

## Step-by-Step Deployment

### 1. Push to GitHub (5 minutes)

```bash
# In your Touti_web folder
git init
git add .
git commit -m "Initial commit: Touti support website"
git branch -M main

# Create a new repository on GitHub first, then:
git remote add origin https://github.com/YOUR_USERNAME/touti-support.git
git push -u origin main
```

### 2. Create Azure Static Web App (10 minutes)

1. Go to https://portal.azure.com
2. Click "Create a resource" → Search "Static Web App" → Create
3. Fill in:
   - **Subscription:** Your Azure subscription
   - **Resource Group:** Create new "touti-resources"
   - **Name:** touti-support
   - **Plan:** Free
   - **Region:** East US 2 (or closest to you)
   - **Source:** GitHub
   - Sign in to GitHub and select your repository
   - **Branch:** main
   - **Build Presets:** Custom
   - **App location:** `/public`
   - **API location:** (leave empty)
   - **Output location:** (leave empty)
4. Click "Review + create" → "Create"
5. Wait 3-5 minutes for deployment

### 3. Get Your Azure URL

1. Once created, go to your Static Web App resource
2. Copy the URL (e.g., `happy-stone-0a1b2c3d4.azurestaticapps.net`)
3. Test it in your browser - your site should be live!

### 4. Add Custom Domain in Azure (5 minutes)

1. In your Static Web App → "Custom domains" → "+ Add"
2. Select "On any other DNS"
3. Domain type: CNAME
4. Enter: `touti.meknassi.ca`
5. Click "Next"
6. **IMPORTANT:** Keep this page open! You'll need the values shown

### 5. Configure GoDaddy DNS (5 minutes)

1. Log in to GoDaddy → My Products
2. Find `meknassi.ca` → Click "DNS"
3. In the Records section, click "Add"

**Add CNAME Record:**
```
Type: CNAME
Name: touti
Value: [paste your Azure URL from step 3, e.g., happy-stone-0a1b2c3d4.azurestaticapps.net]
TTL: 1 Hour
```

4. Click "Add" again

**Add TXT Record:**
```
Type: TXT  
Name: _dnsauth.touti
Value: [paste the validation token from Azure - shown in step 4]
TTL: 1 Hour
```

5. Click "Save" on both records

### 6. Validate Domain in Azure (2 minutes)

1. Go back to Azure Portal (the page from step 4)
2. Click "Add" or "Validate"
3. Wait 1-2 minutes
4. The domain should validate successfully

### 7. Wait for SSL Certificate (10-15 minutes)

- Azure automatically creates a free SSL certificate
- Your site will be accessible at https://touti.meknassi.ca
- Usually ready in 10-15 minutes

### 8. Verify Everything Works

Test all these URLs (replace with https://touti.meknassi.ca once DNS propagates):

- [ ] https://touti.meknassi.ca (home page)
- [ ] https://touti.meknassi.ca/support.html
- [ ] https://touti.meknassi.ca/pages/rules.html
- [ ] https://touti.meknassi.ca/pages/privacy.html
- [ ] https://touti.meknassi.ca/pages/terms.html
- [ ] https://touti.meknassi.ca/pages/email-verified.html
- [ ] https://touti.meknassi.ca/pages/email-error.html

## Troubleshooting

### Domain not working after 30 minutes?

Check DNS propagation:
1. Go to https://whatsmydns.net
2. Enter: `touti.meknassi.ca`
3. Select "CNAME" type
4. Should show your Azure URL globally

### Still not working?

1. Verify GoDaddy records:
   - CNAME: Name=`touti`, Value=`[your-azure-url].azurestaticapps.net`
   - TXT: Name=`_dnsauth.touti`, Value=`[validation-token]`
2. Make sure there's no trailing dot in GoDaddy values
3. Wait up to 24 hours for full DNS propagation
4. Try clearing browser cache or use incognito mode

## Future Updates

Every time you want to update the site:

```bash
# Make your changes to files in public/ folder
git add .
git commit -m "Description of changes"
git push origin main
```

Azure automatically rebuilds and deploys in 2-3 minutes!

## App Store URLs

Use these URLs when submitting to app stores:

**Apple App Store:**
- Support URL: `https://touti.meknassi.ca/support.html`
- Marketing URL: `https://touti.meknassi.ca`
- Privacy Policy: `https://touti.meknassi.ca/pages/privacy.html`

**Google Play Store:**
- Website: `https://touti.meknassi.ca`
- Privacy Policy: `https://touti.meknassi.ca/pages/privacy.html`

**Email Verification (PlayFab/Auth):**
- Success: `https://touti.meknassi.ca/pages/email-verified.html`
- Error: `https://touti.meknassi.ca/pages/email-error.html`

## Summary

Total time: ~30-45 minutes (mostly waiting for DNS and SSL)
Cost: $0 (Free tier)
Maintenance: Automatic deployments via GitHub

---

**Questions?** Check DEPLOYMENT.md for detailed troubleshooting, or email yourself at support@touti.meknassi.ca once live! 😄

