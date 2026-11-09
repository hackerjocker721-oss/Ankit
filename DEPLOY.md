# AnkitHack — Live Karne Ki Poori Guide

Ye website **AnkitHack** ke naam se banayi gayi hai. Is file mein step-by-step
puri guide hai ki isko internet par live (public) kaise karna hai.

---

## ⚠️ Pehle ye samajh lo

Main (AI) domain **khareed nahi sakta**, kyunki domain ke liye:

- Payment karni padti hai (₹400 – ₹1,200 / saal)
- Aapki KYC / account chahiye (Namecheap, GoDaddy, Hostinger, Vercel)

Isliye domain aapko khud lena hoga. Baaki **sab kuch main ready kar diya hai** —
SEO, sitemap, robots.txt, PWA manifest, structured data, favicon, social share
cards. Aap bas domain point kara do, site 2 minute mein public ho jayegi.

---

## 🚀 STEP 1: Free hosting par deploy karo (0 rupaye)

### Sabse aasan — Vercel (recommended)

1. Poora project **GitHub** par upload karo
2. [vercel.com](https://vercel.com) par jao → **Sign up with GitHub**
3. **Add New → Project** → apna repo select karo → **Deploy**
4. Deploy hote hi aapko free URL milega, jaise:
   ```
   https://ankithack.vercel.app
   ```
   Ye URL turant public hai — koi bhi world mein khol sakta hai. ✅

### Database (free) — Neon ya Supabase

Website ko PostgreSQL chahiye:

1. [neon.tech](https://neon.tech) par free account banao
2. Ek project banao → **Connection string** copy karo
   (dikhega: `postgresql://user:pass@ep-xxx.neon.tech/neondb`)
3. Vercel mein **Settings → Environment Variables** mein add karo:

   | Variable | Value |
   |---|---|
   | `DATABASE_URL` | Neon se mila connection string |
   | `ADMIN_PASSWORD` | aapka naya admin password (strong rakho) |
   | `NEXT_PUBLIC_SITE_URL` | `https://ankithack.vercel.app` |
   | `GOOGLE_SITE_VERIFICATION` | (optional) Google Search Console ka code |

4. **Redeploy** karo, phir ye chalao:
   ```bash
   npx drizzle-kit push    # tables ban jayengi
   psql "$DATABASE_URL" -f seed.sql   # demo apps
   ```

> ⚠️ **Important:** Abhi app icons / APK `uploads/` folder mein save hote hain
> jo Vercel par permanent **nahi** rehte. Production ke liye
> [Vercel Blob](https://vercel.com/storage/blob) ya Cloudflare R2 use karo —
> `src/app/api/upload/route.ts` mein sirf `writeFile` ka hissa badalna hai.

---

## 🌐 STEP 2: Apna domain lo — `ankithack.com`

### Domain kharidne ke options

| Registrar | Price (approx) | Link |
|---|---|---|
| Hostinger India | ₹899 – ₹1,199/saal | hostinger.in |
| GoDaddy India | ₹549 – ₹999/saal | godaddy.com |
| Namecheap | $9 – $12/saal | namecheap.com |
| Cloudflare Registrar | at-cost (sabse sasta) | cloudflare.com |

**Check karo available hai ya nahi:** [namecheap.com/domains](https://www.namecheap.com/domains/registration/results/?domain=ankithack)

Agar `.com` nahi mila to ye lo (sasta / free):
- `ankithack.in` — India ke liye
- `ankithack.app`, `ankithack.dev`, `ankithack.io` — tech feel
- `ankithack.eu.org` — **bilkul free**
- `ankithack.vercel.app` — free subdomain (upar se already mil jata hai)

### Domain ko Vercel se jodo

1. Vercel → apna project → **Settings → Domains**
2. `ankithack.com` type karo → **Add**
3. Vercel 2 DNS records dikhayega, jaise:
   ```
   A     @        76.76.21.21
   CNAME www       cname.vercel-dns.com
   ```
4. Apne registrar (jahan domain liya) → **DNS settings** → ye records paste karo
5. 5 minute se 24 ghante tak DNS activate ho jayega (SSL certificate automatic)
6. **Settings → Environment Variables** mein jaake `NEXT_PUBLIC_SITE_URL`
   ko badal do:
   ```
   https://ankithack.com
   ```
7. **Redeploy** karo — ab ye values sitemap, robots, aur social cards
   mein sahi URL banaengi

---

## 🔍 STEP 3: Google par list karao (SEO ready hai)

Website ka sab SEO kaam **maine already kar diya hai**:

| Cheez | Status |
|---|---|
| `sitemap.xml` (auto, saare apps ke saath) | ✅ `/sitemap.xml` |
| `robots.txt` (admin/api blocked) | ✅ `/robots.txt` |
| Google Structured Data (Organization, WebSite) | ✅ |
| `SoftwareApplication` schema har app par | ✅ |
| Open Graph + Twitter cards | ✅ |
| PWA `manifest.webmanifest` | ✅ |
| Favicon + apple icon | ✅ |
| Canonical URLs | ✅ |
| Mobile-friendly responsive | ✅ |

### Ab bas ye karo

1. [search.google.com/search-console](https://search.google.com/search-console) kholo
2. **Add Property** → `https://ankithack.com` daalo
3. Verification ke liye **HTML tag** method choose karo — ek meta tag milega
   jisme `content="abc123..."` hoga
4. Us code ko Vercel ke env mein daalo:
   ```
   GOOGLE_SITE_VERIFICATION=abc123...
   ```
5. Redeploy karo → Search Console mein **Verify** dabao
6. **Sitemaps** section mein jaake submit karo:
   ```
   https://ankithack.com/sitemap.xml
   ```
7. **URL Inspection → Request Indexing** karo `/`, `/apps` aur har app page ka

Google 2 din se 4 hafte mein site ko index kar leta hai. Search karoge
`ankithack` to site dikhne lagegi.

> 💡 **Jaldi rank karne ke liye:** Har app ka description achi likho (3-4 line),
> screenshots upload karo, aur site ka link apne social media / YouTube /
> WhatsApp par share karo. Backlinks aane se ranking tez hoti hai.

---

## 🔐 Admin Panel

```
https://ankithack.com/admin
```

Password env variable `ADMIN_PASSWORD` se aata hai. **Live karne se pehle
ise zaroor badal do** aur strong rakho (12+ characters).

Default dev password: `ankit@2024`

---

## 📁 Project Structure

```
src/
├── app/
│   ├── page.tsx              Home (3D hero)
│   ├── apps/                 Library + detail pages
│   ├── admin/                Admin panel
│   ├── api/                  REST endpoints
│   ├── robots.ts             robots.txt
│   ├── sitemap.ts            sitemap.xml
│   ├── manifest.ts           PWA manifest
│   └── icon.png              Favicon
├── components/               3D + UI components
├── db/                       Drizzle schema + client
└── lib/                      site config, auth, constants
```

---

## ✅ Checklist (live karne se pehle)

- [ ] `ADMIN_PASSWORD` strong password se badla
- [ ] `NEXT_PUBLIC_SITE_URL` apne asli domain se badla
- [ ] Database (Neon/Supabase) connect kiya + `drizzle-kit push` chalaya
- [ ] Uploads ke liye Blob storage set kiya (Vercel Blob / R2)
- [ ] Domain kharida aur DNS point kiya
- [ ] Google Search Console verify + sitemap submit
- [ ] Apni real apps upload ki (admin panel se)
- [ ] Demo apps hata di (jo seed se aayi thi)
