# YourEquityMatters.com - Landing Page

## 📋 Overview

This is a lead generation landing page for Homestead Real Estate's "You Crushed It" equity-focused campaign targeting 500 Longmont homeowners who purchased between 2009-2021.

## 🎯 Campaign Purpose

**Segment:** "The Winners" (500 Longmont homeowners who bought bottom 2009-2012 + others through 2021)
**Message:** "You crushed it making money on your home. Now let's see what you can do with all that equity."
**Goal:** Drive postcard recipients to land on the site, enter their address, see their estimated equity, and convert to a qualified lead.

## 🔧 How It Works

### 1. **Address Lookup**
- Visitor enters their Longmont address
- System searches the embedded property database (from CSV)
- If found: Auto-populates purchase price, beds, baths, sq ft
- If not found: User manually enters their purchase details

### 2. **Equity Calculator**
Uses tiered appreciation rates based on purchase year:
- **2009-2013:** 5% annual appreciation
- **2014-2021:** 11.5% annual appreciation
- **2022:** 7% appreciation
- **2023-2026:** 1% appreciation (recent slowdown)

### 3. **Renovation Adjustments**
Users can select renovations to boost their estimate:
- Kitchen: +4.5%
- Bathroom: +2.5%
- Master Bath: +3%
- Roof: +2.5%
- HVAC: +2%
- Flooring: +3.5%
- Windows: +2%

### 4. **Lead Capture**
After calculator, visitor sees:
- Large equity number (the "wow" moment)
- Commission rates section (0.9% / 1.9%)
- "Remove the Friction" value props
- Track record section
- Personal intro from James
- **"Let's Make a Move" form** capturing:
  - Name, email, phone
  - Timeline to sell
  - How they heard about it
  - Their situation/message

### 5. **Data Tracking**
Form submission logs:
- Whether they used the calculator
- Their estimated equity
- Contact info & situation
- Source (postcard vs Facebook ad)

## 📁 Files Included

```
├── index.html                          # Main landing page (self-contained)
├── logo.png                            # Homestead Real Estate logo
├── james-taylor-professional.jpg       # Your professional headshot
├── README.md                           # This file
└── DEPLOYMENT.md                       # GitHub + Vercel setup guide
```

**Note:** The HTML is fully self-contained. It includes:
- Tailwind CSS (CDN)
- FormInit integration (for form handling)
- Property database embedded in JavaScript
- All styling and logic in one file

## 🚀 Deployment Steps

### Step 1: GitHub Setup
```bash
# Create a new GitHub repo named "yourequitymatters"
git init
git add .
git commit -m "Initial commit: YourEquityMatters landing page"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/yourequitymatters.git
git push -u origin main
```

### Step 2: Connect to Vercel
1. Go to [vercel.com](https://vercel.com)
2. Sign in with GitHub
3. Click "New Project"
4. Select the `yourequitymatters` repo
5. Framework: "Other" (no build required)
6. Deploy root directory: `/`
7. Click "Deploy"

### Step 3: Custom Domain
1. In Vercel project settings → Domains
2. Add your domain: `yourequitymatters.com`
3. Update DNS records (Vercel will provide instructions)

### Step 4: FormInit Setup (for form submissions)
The form currently logs to browser console. To capture leads:

**Option A: Connect to CRM/Email Service**
- Update the form submission handler in the JavaScript section
- Integrate with Zapier, Make.com, or direct API calls

**Example with Zapier Webhook:**
```javascript
// Replace the console.log with:
fetch('YOUR_ZAPIER_WEBHOOK_URL', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(formData)
});
```

**Option B: Use FormInit (already included)**
- Update the form with FormInit attributes
- Set up FormInit endpoint for email notifications

## 📊 CSV Integration (Future)

When you have the full 500-home CSV, here's how to integrate it:

1. Parse CSV into JSON property database
2. Replace the hardcoded `propertyDatabase` array
3. Keep the same object structure:
   ```javascript
   {
       address: "1345 Lincoln St",
       purchasePrice: 195000,
       purchaseDate: "2011",
       sqft: 1064,
       beds: 3,
       baths: 2,
       owner: "Name"
   }
   ```

## 📱 Marketing Integration

### Postcard CTA
```
You Crushed It! Now Let's See How Much.
See what your Longmont home equity is worth today.
→ YourEquityMatters.com ←
Enter your address. Takes 60 seconds.
```

### Facebook Ad Copy
```
Headline: "See What Your Home Equity Is Worth"
Description: "If you bought your Longmont home between 2009-2021, you've likely built significant equity. Check it now."
```

## 🔐 Privacy & Compliance

- Form data is collected locally (no external tracking by default)
- Recommend adding privacy policy link in footer
- Consider GDPR compliance if targeting any EU visitors
- Form submissions should be encrypted in production

## 🛠️ Customization

### Change Appreciation Rates
Edit the `getAppreciationRate()` function in the script section:
```javascript
if (year >= 2009 && year <= 2013) return 0.05;    // Change this
```

### Update Renovation Premiums
In the `calculateEquity()` function:
```javascript
if (reno === 'kitchen') renovationBoost += currentValue * 0.045; // Change multiplier
```

### Change Commission Rates
Search for "0.9%" and "1.9%" in the HTML and update.

### Update Track Record Numbers
Find the "My Track Record" section and update the stats.

## 📞 Support & Next Steps

1. **Test the calculator** with known properties
2. **Set up form submission** routing (Zapier/CRM)
3. **Create postcard design** linking to the domain
4. **A/B test** different postcard messaging
5. **Track lead quality** from calculator vs non-calculator submissions

## ✅ Checklist Before Launch

- [ ] Domain registered and DNS configured
- [ ] Logo and photo display correctly
- [ ] Calculator works with sample addresses
- [ ] Form submission routing configured
- [ ] Mobile responsiveness tested
- [ ] All links functional
- [ ] Privacy policy added
- [ ] Analytics/tracking pixels added (if desired)

---

**Built for:** Homestead Real Estate, LLC  
**Campaign:** "The Winners" - Longmont Equity Campaign  
**Last Updated:** June 2026
