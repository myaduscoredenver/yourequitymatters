# Deployment Guide: GitHub → Vercel

Follow these steps to get YourEquityMatters.com live on the web.

## Prerequisites

- GitHub account (free at [github.com](https://github.com))
- Vercel account (free at [vercel.com](https://vercel.com))
- Git installed on your computer (or use GitHub web interface)
- Your domain name registered (or point a subdomain)

---

## Step 1: Create GitHub Repository

### Option A: GitHub Web Interface (Easiest)

1. Go to [github.com/new](https://github.com/new)
2. **Repository name:** `yourequitymatters`
3. **Description:** "Your Equity Matters - Longmont home equity calculator landing page"
4. **Visibility:** Public
5. Click **"Create repository"**

### Option B: Command Line

```bash
cd /path/to/your/project
git init
git add .
git commit -m "Initial commit: YourEquityMatters landing page"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/yourequitymatters.git
git push -u origin main
```

---

## Step 2: Upload Files to GitHub

### Via Web Interface:

1. Open your repo: `https://github.com/YOUR-USERNAME/yourequitymatters`
2. Click **"Add file"** → **"Upload files"**
3. Drag and drop:
   - `index.html`
   - `logo.png`
   - `james-taylor-professional.jpg`
   - `README.md`
4. Click **"Commit changes"**

### Via Git Command Line:

```bash
git add index.html logo.png james-taylor-professional.jpg README.md
git commit -m "Add landing page files"
git push origin main
```

---

## Step 3: Deploy to Vercel

### Option A: Quick Deploy (Recommended)

1. Go to [vercel.com/new](https://vercel.com/new)
2. Click **"Import Git Repository"**
3. Authenticate with GitHub (if not already)
4. Select `yourequitymatters` repo
5. **Framework Preset:** Select "Other" (no build step needed)
6. Click **"Deploy"**

**Vercel will generate a preview URL in ~1-2 minutes**

### Option B: Vercel CLI

```bash
npm i -g vercel
vercel
```

Follow the prompts. Vercel will assign you a `.vercel.app` domain.

---

## Step 4: Custom Domain Setup

### If you own yourequitymatters.com:

1. In Vercel dashboard, go to your project
2. Click **"Settings"** → **"Domains"**
3. Enter domain: `yourequitymatters.com`
4. Choose **"Nameserver"** or **"CNAME"** (Vercel will recommend)
5. Update your domain registrar's DNS settings with Vercel's nameservers
6. Wait 24-48 hours for DNS propagation

**Check if it worked:**
```bash
nslookup yourequitymatters.com
```

---

## Step 5: Enable HTTPS (Automatic)

Vercel automatically provisions **free SSL certificates** for all domains.

✅ Your site will be accessible at: `https://yourequitymatters.com`

---

## Step 6: Form Submission Routing

By default, form submissions log to browser console. To actually capture leads:

### Option A: Email Notifications with Formspree

1. Go to [formspree.io](https://formspree.io)
2. Create a new form, get your form endpoint: `https://formspree.io/f/YOUR_ID`
3. Update the form in `index.html`:

```html
<form action="https://formspree.io/f/YOUR_ID" method="POST" id="contactForm">
    <!-- form fields stay the same -->
</form>
```

Form submissions will email you directly.

### Option B: Zapier Integration

1. Create a Zap at [zapier.com](https://zapier.com)
   - **Trigger:** Catch Raw Hook
   - **Action:** Send Email / Add to Google Sheets / Create Airtable Record
2. Get your Zapier webhook URL
3. Update the form submission in `index.html`:

```javascript
document.getElementById('contactForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    const formData = new FormData(this);
    const data = {
        firstName: formData.get('firstName'),
        lastName: formData.get('lastName'),
        email: formData.get('email'),
        phone: formData.get('phone'),
        timeline: formData.get('timeline'),
        source: formData.get('source'),
        message: formData.get('message'),
        equityCalculatorUsed: currentCalculation ? true : false,
        estimatedEquity: currentCalculation ? currentCalculation.equity : null
    };

    fetch('YOUR_ZAPIER_WEBHOOK_URL', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data)
    }).then(() => {
        alert('Thanks! I\'ll be in touch within 24 hours.');
        this.reset();
    });
});
```

### Option C: Direct CRM Integration

If you use a specific CRM (HubSpot, Salesforce, etc.), update the form endpoint directly.

---

## Step 7: Testing

### Test on Different Devices:

- **Desktop:** Chrome, Safari, Firefox
- **Mobile:** iPhone Safari, Android Chrome
- **Tablet:** iPad, Android tablet

### Test Calculator:

Try these addresses (from sample CSV):
- `1345 Lincoln St` → $195k purchase, 2011
- `1136 Aspen St` → $208k purchase, 2014
- `1136 Judson St` → $215k purchase, 2009

### Test Form Submission:

Fill out the form and verify you receive the submission.

---

## Step 8: Analytics Setup (Optional)

### Add Google Analytics:

1. Go to [analytics.google.com](https://analytics.google.com)
2. Create a new property for yourequitymatters.com
3. Get your tracking ID (e.g., `G-XXXXXXXXXX`)
4. Add to `<head>` in `index.html`:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Add Facebook Pixel:

1. Go to [facebook.com/ads/manager](https://facebook.com/ads/manager)
2. Create a pixel
3. Add to `<head>`:

```html
<!-- Facebook Pixel -->
<script>
  !function(f,b,e,v,n,t,s)
  {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
  n.callMethod.apply(n,arguments):n.queue.push(arguments)};
  // ... Facebook pixel code
</script>
```

---

## Step 9: Update & Redeploy

After making changes:

```bash
git add .
git commit -m "Update: [describe change]"
git push origin main
```

Vercel automatically redeploys when you push to `main`. Changes go live in ~30 seconds.

---

## Common Issues & Fixes

### "404 Not Found" Error

- **Cause:** Files not pushed to GitHub
- **Fix:** Run `git push origin main` again

### Images not loading (logo, photo)

- **Cause:** Image filenames don't match HTML `src` attributes
- **Fix:** Ensure filenames are exactly: `logo.png`, `james-taylor-professional.jpg`
- **Verify:** Check Vercel deployment logs

### Form not submitting

- **Cause:** No endpoint configured or endpoint is broken
- **Fix:** Check browser console (F12 → Console tab) for errors
- **Verify:** Test with Formspree or Zapier integration

### HTTPS certificate not working

- **Cause:** Usually DNS not updated yet
- **Fix:** Wait 24-48 hours, then test with `https://yourequitymatters.com`

### Site is slow

- **Cause:** Usually not Vercel (it's fast)
- **Fix:** Clear browser cache, test from incognito window
- **Check:** Vercel analytics dashboard

---

## Monitoring & Maintenance

### Check Site Health:

1. **Vercel Dashboard:** [vercel.com/dashboard](https://vercel.com/dashboard)
   - View deployments, errors, analytics
   
2. **GitHub:** [github.com/YOUR-USERNAME/yourequitymatters](https://github.com/YOUR-USERNAME/yourequitymatters)
   - Check commit history, make updates

3. **Google Analytics:** See visitor traffic, form submissions, bounce rate

### Monthly Maintenance:

- Review form submissions
- Update "Track Record" numbers as needed
- Check calculator accuracy against recent comps
- Monitor 404 errors in Vercel logs
- Update appreciation rates annually

---

## Backup & Version Control

Your GitHub repo is your backup. To restore:

```bash
git clone https://github.com/YOUR-USERNAME/yourequitymatters.git
```

All versions are stored in Git history:

```bash
git log --oneline  # See all past commits
git checkout [commit-hash]  # Revert to any previous version
```

---

## Additional Resources

- [Vercel Docs](https://vercel.com/docs)
- [GitHub Guides](https://guides.github.com)
- [Formspree Docs](https://formspree.io/docs)
- [Zapier Setup Guide](https://zapier.com/help)

---

## Support

If you run into issues:

1. **Vercel Status:** [status.vercel.com](https://status.vercel.com)
2. **GitHub Status:** [githubstatus.com](https://githubstatus.com)
3. **Check browser console:** Press F12, look for error messages
4. **Vercel support:** [vercel.com/support](https://vercel.com/support)

---

**Your site should now be live at: `https://yourequitymatters.com` 🎉**

Next step: Create and mail out postcards with this URL!
