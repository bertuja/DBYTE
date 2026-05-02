# DBYTE SEO Setup Guide — Google Search Console + Business Profile

## Summary of Recent Performance Improvements (2026-05-02)

**Performance optimizations deployed:**
- ✅ Fixed hero logo LCP bug (loading=lazy → eager, fetchpriority=high)
- ✅ Added resource hints for external CDNs (preconnect + dns-prefetch)
- ✅ Converted PNG logo to WebP (102KB → 44KB, 57% savings)
- ✅ Minified CSS/JS (9% total size reduction: 75.3KB → 68.4KB)

**Estimated Core Web Vitals improvements:**
- LCP: ~200-400ms faster (fixed above-fold image priority)
- FID: ~30-50ms faster (reduced JS parsing)
- CLS: Maintained (no layout shifts)

---

## Part 1: Google Search Console Setup

### 1.1 Create a Google Account (if needed)
- Go to https://accounts.google.com
- Sign up or sign in with existing account
- This will be your GSC admin account

### 1.2 Add Your Website Property
1. Go to **https://search.google.com/search-console**
2. Click **"Add property"** (top left)
3. Select **"URL prefix"** (not "Domain")
4. Enter: `https://www.d-byte.com.ar`
5. Click **Continue**

### 1.3 Verify Ownership (HTML Meta Tag Method — Fastest)
1. You'll see a verification options dialog
2. Select the **"HTML tag"** tab
3. Copy the entire meta tag (looks like):
   ```html
   <meta name="google-site-verification" content="XXXXX...">
   ```
4. Add this tag to your `public/index.html` in the `<head>` section (after line ~13, after the canonical link)
5. Save and deploy: 
   ```bash
   git add public/index.html
   git commit -m "Add Google Search Console verification meta tag"
   git push origin master
   vercel deploy --prod
   ```
6. Return to GSC and click **"Verify"**
7. Wait 1-2 minutes for verification to complete

### 1.4 Submit Your Sitemap
1. In GSC left sidebar, go to **Sitemaps** (under "Index")
2. Click **"New sitemap"**
3. Enter: `https://www.d-byte.com.ar/sitemap.xml`
4. Click **Submit**
5. Monitor the status — should show "Success" within 1 hour

### 1.5 Monitor Key Metrics
- **Coverage** — How many pages Google found and indexed
  - Go to **Coverage** in left sidebar
  - Should show `index.html` indexed (1 page total)
  - Watch for errors (none expected)

- **Performance** — Click rates, impressions, position
  - Go to **Performance** in left sidebar
  - Shows search traffic from Google (takes 1-2 weeks to populate)
  - Monitor which keywords drive traffic

- **Core Web Vitals** — Performance report
  - Go to **Core Web Vitals** in left sidebar
  - Shows LCP, FID, CLS metrics from real users
  - Updates daily (takes 1-2 weeks of traffic data)
  - Target: "Good" status (green)

- **Mobile Usability** — Mobile-specific issues
  - Go to **Mobile Usability** in left sidebar
  - Should show "No issues" (already passing)

### 1.6 Check Indexation Status (Optional)
In GSC, use the **URL Inspection** tool:
1. Paste: `https://www.d-byte.com.ar`
2. Shows: Last crawl date, rendering, indexation status
3. Click **"Request indexing"** if needed (usually not necessary)

---

## Part 2: Google Business Profile Setup

### 2.1 Search for Existing DBYTE Listing
1. Go to **https://business.google.com**
2. Click **"Sign in"** (top right) — use same account as GSC
3. Click **"Manage your business"** or search for "DBYTE"
4. Check if a listing already exists:
   - **If YES** → Click "Claim this business" (step 2.2)
   - **If NO** → Click "Create a business" (step 2.3)

### 2.2 Claim Existing Business (If Found)
1. Click on the DBYTE listing
2. Click **"Claim this business"**
3. Verify via one of these methods:
   - **Phone** (fastest) — Google calls with a verification code
   - **Email** — Receive code via email
   - **Postcard** — Mailed to your address (1-2 weeks)
4. Enter the verification code
5. Follow **Step 2.4: Complete Your Profile** below

### 2.3 Create New Business Listing
1. Click **"Create a new business"**
2. Fill in the form:
   - **Business name**: `DBYTE`
   - **Business address**: `Gavilán 1207, Entrepiso AB, CP 1407, CABA, Argentina`
     - (Use exact address from your contact section)
   - **Phone**: `+54 9 11 31040042`
   - **Service area**: Select `Argentina` or list specific provinces
   - **Website**: `https://www.d-byte.com.ar`
3. Click **Create**
4. Choose verification method (same as step 2.2)
5. Enter verification code
6. Follow **Step 2.4: Complete Your Profile** below

### 2.4 Complete Your Business Profile
Once verified, complete all fields:

**Key Fields to Fill:**
- **Business name**: DBYTE (or DBYTE SRL)
- **Phone**: +54 9 11 31040042
- **Website**: https://www.d-byte.com.ar
- **Address**: Gavilán 1207, CP 1407, CABA (already filled)
- **Category**: Select "IT Services" or "Computer Support Services"
  - Add more: "Cloud Computing", "Cybersecurity", "Infrastructure Management"
- **Service area**: Argentina (mark "serves areas in")
- **Business hours**: Mon-Fri 9:00–18:00, Support 24/7
- **Description**: 
  ```
  Soluciones integrales de infraestructura IT, ciberseguridad y continuidad 
  de negocio para PyMEs y empresas en Argentina y Latinoamérica. Servicios 
  especializados en cloud, AWS, seguridad informática, RMM, MDR y IA.
  ```

**Add Media:**
- Click **"Add photos"** and upload:
  - Office/Team photo
  - Logo (iso-gradient-blanca.png)
  - Service/infrastructure photo if available

**Other Important Fields:**
- **Attributes**: Check relevant ones (e.g., "Remote support", "Custom solutions")
- **Questions & Answers**: Encourage customer Q&A
- **Posts**: Create posts about services (similar to the blog content you considered)

### 2.5 Link to Website
Your business is now linked to `https://www.d-byte.com.ar` in Google Business Profile.
- Google Maps will show your listing
- Customers can find you via Google search + Maps
- Reviews/ratings appear on your profile and search results

---

## Part 3: Monitor & Maintain

### Weekly Tasks
- **GSC**: Check Performance tab for new search queries
- **GB Profile**: Monitor "Insights" tab for customer searches and views

### Monthly Tasks
- **PageSpeed Insights**: Run audit to track Core Web Vitals trend
  ```
  https://pagespeed.web.dev/?url=https%3A%2F%2Fwww.d-byte.com.ar
  ```
- **Search Console**: Review "Coverage" for any indexation issues
- **Mobile Usability**: Check for any new warnings

### When to Update Profiles
- **Contact info changes** → Update in both GSC and GB Profile
- **Service changes** → Update service list in GB Profile
- **Hours change** → Update business hours in GB Profile
- **Site maintenance** → Let GSC know via "Request a recrawl"

---

## Part 4: Rich Results Testing (Optional)

Test your schema markup in Google's Rich Results Test:

1. Go to **https://search.google.com/test/rich-results**
2. Enter: `https://www.d-byte.com.ar`
3. Click **Test URL**
4. Review results:
   - **Organization** — Company info (✅ should pass)
   - **LocalBusiness** — Address, phone, services (✅ should pass)
   - **FAQPage** — Q&A section (✅ should pass)
5. All should show "✓ Valid" in green

---

## Part 5: Expected Timeline

- **Verification (GSC/GB)**: 1–5 minutes (HTML tag method)
- **First indexation**: 1–3 days (your pages in Google's index)
- **First search traffic**: 1–2 weeks (data starts appearing in GSC Performance)
- **Core Web Vitals data**: 1–2 weeks (real user metrics)
- **Ranking improvements**: 2–6 weeks (depending on competition)

---

## Troubleshooting

**GSC shows "Not indexed"?**
- Wait 3 days (indexing can be slow)
- Go to **URL Inspection** → **Request indexing**
- Check **Coverage** tab for any errors

**Google Business Profile stuck on "Verification pending"?**
- Check spam/trash emails for verification code
- Try phone verification (faster than postcard)
- Ensure you're using the same Google account

**Search results don't show my business info?**
- May take 3–7 days for rich results to appear
- Check Google rich results test (Part 4) for validation errors
- Ensure schema markup is present (should be already)

---

## Next Steps

1. ✅ Add GSC meta tag to index.html (above)
2. ✅ Deploy to production
3. ✅ Verify GSC ownership
4. ✅ Submit sitemap in GSC
5. ✅ Create/Claim Google Business Profile
6. ✅ Complete all business profile fields
7. Monitor performance over 2 weeks

**Expected Results:**
- Pages indexed in Google (1–3 days)
- Appears in Google Maps (1–5 days after GB Profile setup)
- Search traffic data in GSC (1–2 weeks)
- Ranking improvements (2–6 weeks for new sites)

---

## Questions?

- **GSC Help**: https://support.google.com/webmasters
- **Business Profile Help**: https://support.google.com/business
- **Schema Markup**: https://schema.org/LocalBusiness

Good luck! 🚀
