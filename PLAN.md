# App Landing Page — Plan

A static, single-page introduction site for the iOS app developed in `/Users/sunny/code/bunengzhai`.
It also serves as the **Privacy Policy URL** and **Support URL** that App Store Connect requires at submission.

Design goal: **clean and minimal (清爽简洁)**.


## 0. Notes for the agent picking this up

- **Talk to the user in Chinese.** Code and comments in English.
- **Ask before committing** to git. The user reviews changes first, then says "提交吧".
- **Do not modify the app repo** (`/Users/sunny/code/bunengzhai`). Read it for facts:
  - `PLAN.md` there is the app's implementation plan, with status.
  - The SwiftUI views are under `DailyMetrics/Features/`.
- **Only advertise what the app actually does today** (section 2). iCloud sync is planned but **not built**. Don't claim it, or label it clearly as "coming later".
- **Resolve the open decisions (section 3) with the user before writing final copy.** The layout can be built with placeholders first.
- This directory is empty and not yet a git repo. Run `git init` when starting.


## 1. What this site is

- **`index.html`**: introduces the app. One page, a few sections, nothing extra.
- **`privacy.html`**: privacy policy. Required by App Store review.
- **Support**: a contact email on the page is enough for the Support URL.


## 2. Facts about the app (as of 2026-10-03)

Native iOS app: Swift, SwiftUI, SwiftData. Requires iOS 18+. iPhone and iPad.

**Implemented (Phase 1, local-only):**
- **User-defined daily metrics**, each with a name, unit and number of decimal places (0–4).
  - Defaults on first launch: Running (m), Pull-ups (reps), Dizi (min), Piano (min).
  - Examples users might add: Weight (kg, 1 decimal), Sleep (h), Reading (min).
- **Today screen**: one number field per metric, plus a free-text daily reflection (max 500 characters).
  - Autosaves; there is no Save button.
  - ‹ › arrows and a date picker let you fill in or fix **any past day**.
- **History**: all recorded days, newest first, each showing that day's values and the first line of its reflection. Tap a day to edit it.
- **Metrics management**: add, rename, reorder.
  - Deactivating a metric hides it from new entries but **keeps all its history**.
  - Reactivating returns it to its previous position.
- **"Not recorded" and "recorded 0" are different** — a small but honest detail.
- **No account, no server, no network required.** All data stays on the device.

**Not implemented — do not advertise:**
- iCloud sync (Phase 2, planned: the user's private iCloud database via CloudKit)
- Charts, statistics, streaks, goals
- Widgets, Apple Watch, HealthKit
- Notifications
- Export

**Other facts:**
- App UI language: English.
- Accent color: system default blue (no custom accent set yet).
- App icon: none yet.
- Not on the App Store yet.


## 3. Open decisions (ask the user)

1. **App name / brand.** The working name in code is "Daily Metrics". The repo folder is "bunengzhai" (不能斋). The site title, the App Store name and the in-app display name should match.
2. **Language:** Chinese, English, or bilingual (e.g. a language toggle, or `/en/` and `/zh/` pages)?
3. **Screenshots:** the user will take them on their iPhone (Today, History, Metrics), ideally with realistic sample data. Until then, use placeholder frames.
4. **Contact email** for Support and Privacy.
   - Use a placeholder `TODO@example.com` until the user provides one.
   - Do not guess or reuse any email from the environment.
5. **Hosting / domain:** GitHub Pages or Cloudflare Pages (both free). Custom domain or not.
6. **Accent color:** keep system blue, or pick a brand color. If a new color is chosen, the app's `AccentColor` should be updated to match (in the app repo, by the user / app agent).


## 4. Page structure (`index.html`)

1. **Hero**
   - App name and a one-line tagline in the spirit of 「每天记一点，看见自己的坚持」.
   - One iPhone screenshot (the Today screen).
   - An App Store link, shown as "Coming soon" until launch (see section 7).
2. **Three features**, each with a small icon and 1–2 sentences:
   - **你定义要记什么**: any metric, any unit (running, pull-ups, dizi, piano…).
   - **每一天都能回头改**: fill in a missed day or fix a mistake anytime.
   - **数据只属于你**: no account, no server, works offline.
3. **Screenshots**: Today / History / Metrics side by side on desktop, horizontally scrollable (scroll-snap) on mobile.
4. **Footer**: Privacy Policy link, contact email, © year and name.

Keep it to these sections. No testimonials, pricing, newsletter or FAQ unless the user asks.


## 5. Visual design

- **Fonts:** system font stack only: `-apple-system, BlinkMacSystemFont, "SF Pro Text", "PingFang SC", "Hiragino Sans GB", "Helvetica Neue", sans-serif`.
  No web fonts. This keeps the site fast and consistent with the "we load nothing third-party" privacy story.
- **Color:**
  - Mostly white space; text in near-black and grays.
  - One accent color.
  - Define all colors as CSS custom properties on `:root`.
- **Dark mode:** follow the system via `@media (prefers-color-scheme: dark)`.
- **Screenshots:**
  - Rounded corners (~40px at full size) and a soft, subtle shadow.
  - No heavy device-frame images.
  - Use `<img>` with explicit `width`/`height` to avoid layout shift, and `loading="lazy"` below the fold.
- **Layout:**
  - Max content width ~1000px, generous vertical spacing.
  - Mobile-first. Must look good at 375px wide with no horizontal page scroll.
- **Icons:** inline SVG (simple strokes), no icon font or CDN.
- **Motion:** none, or at most a subtle fade. Respect `prefers-reduced-motion`.


## 6. Technical requirements

- Plain HTML + one CSS file. **No framework, no build step, no JavaScript** unless genuinely needed (e.g. a language toggle).
- **No external requests:** no CDNs, no analytics, no cookies, no trackers, no embedded fonts.
  The privacy policy can then truthfully say the website collects nothing either.
- Semantic HTML (`header`, `main`, `section`, `footer`), correct `lang` attribute, alt text on every screenshot, sufficient contrast in both themes.
- `<meta name="viewport">`, `<meta name="description">`, a `<title>`, and Open Graph tags (`og:title`, `og:description`, `og:image`) for nice link previews.
- Favicon: a simple placeholder (e.g. an SVG letter mark) until the app icon exists.
- Optional later: `<meta name="apple-itunes-app" content="app-id=…">` (Smart App Banner) once the app has an App Store ID.

Suggested files:

```
index.html
privacy.html
styles.css
assets/
    screenshots/   today.png, history.png, metrics.png   (placeholders until provided)
    favicon.svg
    og-image.png   (optional)
README.md          (how to preview locally and deploy)
```

Preview locally with `python3 -m http.server` in this directory.


## 7. App Store link / badge

- Apple's official "Download on the App Store" badge has usage guidelines. Only use it once the app is live, linking to the real App Store URL.
- Before launch, show plain text or a neutral button ("即将上架 App Store / Coming soon to the App Store"), not the official badge.


## 8. Privacy policy content (`privacy.html`)

Write it plainly, matching the actual app behavior:

- The app **does not collect, transmit or sell any personal data**.
  - No analytics, no advertising, no third-party SDKs, no tracking.
  - No account.
- All data (metrics, values, reflections) is **stored only on the user's device**.
- The developer **has no access** to the data.
- Deleting the app deletes its data.
- The website: no cookies, no analytics, no third-party resources.
- Contact email for questions.
- Effective date.
- **Mark in an HTML comment that this section must be updated when iCloud sync ships.** It will then say: data syncs via the user's own private iCloud account (Apple CloudKit), and the developer still cannot access it.

This also matches the App Store "App Privacy" answer: **Data Not Collected**.


## 9. Deployment

- GitHub Pages (from the repo root) or Cloudflare Pages. No build command; the output is the repo root.
- Wait until the user chooses a host/domain (section 3) before setting up deployment.
- Note the resulting URLs for App Store Connect:
  - Marketing URL → `/`
  - Support URL → `/` (or `/#contact`)
  - Privacy Policy URL → `/privacy.html`


## 10. Definition of done

- `index.html` and `privacy.html` render well on iPhone-width and desktop, in light and dark mode.
- No external network requests (check the browser devtools Network tab).
- All copy matches section 2: nothing unimplemented is advertised.
- Placeholders (screenshots, email, name) are clearly marked with `TODO` and listed in `README.md`, until the user supplies them.
