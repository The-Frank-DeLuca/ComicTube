# ComicTube Mobile App Prototype

A clickable, mobile-first prototype of the ComicTube app for client presentation. Every screen is a **standalone HTML file** — no shared CSS/JS files, no build step, no framework/CDN dependency. Each file can be pasted directly into a GoHighLevel Custom Code / Custom HTML element on its own page.

All data is mock data. There is no real backend — screen-to-screen state (login role, subscription tier, follows, saves, content mode, etc.) is kept in the browser's `localStorage` under the key `ct_state_v1`, so the demo feels persistent as you click through it on one device/browser.

### Images

Comedian avatars, the profile cover banner, content thumbnails, and the event hero image are pulled live from two free placeholder-image services: **`i.pravatar.cc`** (generic stock-photo faces, not real named individuals — standard for UI prototypes) and **`picsum.photos`** (generic stock photography, seeded by ID so the same "photo" shows consistently every time you view that comedian/content/event). This is the one place the prototype is *not* fully offline — it needs internet access to load these images, and won't work if GHL or the viewing network blocks external image hosts. Every image has a JS `onerror` fallback that hides it and reveals the colored initials/gradient underneath, so a blocked or slow image never breaks the layout. Swap in real photography before the actual client-facing launch — these are placeholders only.

## How to preview locally

Open `index.html` in any browser. On a desktop browser it renders inside a phone-shaped frame; on an actual phone (or narrow browser window) it fills the screen edge-to-edge, matching how it will look inside the GHL mobile app / mobile web.

## How to deploy into GoHighLevel

1. Create a GHL page (or Funnel step) for each screen you want live — one page per HTML file.
2. Add a **Custom Code / HTML** element to that page.
3. Open the corresponding `.html` file here, copy everything **inside `<body>...</body>`** (the `<style>` block in `<head>` needs to go into the element too — either paste the whole file into a "Custom HTML" element that accepts full documents, or move the `<style>` block into GHL's page-level Custom CSS field, whichever your GHL element supports).
4. Update any `location.href='other-screen.html'` references to the actual GHL page URLs/slugs you create, once your page structure is final. Until then, keep the filenames the same as the ones in this folder and the prototype's internal navigation works as-is if all files are hosted at the same path level.
5. No API keys, no build step, no npm install — it's plain HTML/CSS/JS.

## Screen map (37-screen spec → 26 prototype files)

Some spec screens were combined into one file where they're really one interaction (e.g. Age Gate Modal lives inside Mode Selection; Community boards + composer live in one file) to keep the file count manageable for a first client pass. Every one of the 10 MVP features from the spec is represented.

| File | Spec Screen(s) | Notes |
|---|---|---|
| `index.html` | S01 Splash | Also a "View as Fan / View as Comedian" role picker for demo purposes |
| `login.html` | S04 Login | Fan/Comedian tabs. Type `fail` as password to see the error state |
| `fan-register.html` | S02 Fan Registration | Try email `taken@example.com` to see the duplicate-account error |
| `mode-select.html` | S33 Mode Selection, S34 Age Gate | Age gate is previewable via a demo button |
| `discover.html` | S05 Discovery Browse, S06 Filter Panel | Trending row + ranked list + genre filters |
| `comedian-profile.html` | S07 Comedian Profile | Dynamic via `?id=c1`…`c6`. Follow/Save/Share/Tip all wired up |
| `content-player.html` | S10 Content Player, S11 Paywall Overlay | 2-minute preview simulated as 10s. Kids Mode blocks Adult content entirely |
| `tier-select.html` | S12 Tier Selection | Free vs Premium comparison |
| `checkout.html` | S13 Subscription Checkout | Mock Stripe form. Card `4000 0000 0000 0002` previews a decline |
| `premium-confirm.html` | S14 Premium Confirmation | Success receipt screen |
| `account.html` | S15 Fan Account, S35 Content Settings | Cancel-subscription flow, Kids/Adult toggle |
| `saved.html` | S16 Saved List | Remove-from-saved action |
| `events.html` | S30 Events Browse | Format filter chips |
| `event-detail.html` | S31 Event Detail | Attending toggle + reminder toast |
| `rooms.html` | S24 Room Listing | Live vs upcoming rooms |
| `live-room-fan.html` | S23 Live Room — Fan View | Simulated live reactions and viewer count |
| `comedian-register.html` | S03 Comedian Registration | 18+ gate, mock Stripe Connect, ToS + Revenue Share checkboxes |
| `comedian-dashboard.html` | S36 Comedian Dashboard Home | Nav hub + comedian bottom tab bar |
| `profile-edit.html` | S08 Comedian Profile Edit | Genre tags capped at 3, live bio counter |
| `content-upload.html` | S09 Content Upload | Simulated upload progress → Mux processing → publish |
| `analytics.html` | S19 Analytics Dashboard, S20 Content Detail | Pure-SVG subscriber growth chart, tap a content row for detail |
| `go-live.html` | S21 Go Live Setup | Solo Set vs Open Mic, start now vs schedule |
| `live-room-comedian.html` | S22 Live Room — Comedian View | End Room confirmation flow |
| `comedian-account.html` | Account/Payouts | $16 monetization unlock, Stripe Express connect |
| `community.html` | S26 Community Home, S27 Board Screen, S29 New Post | 4 boards, Platform News is read-only for comedians |
| `post-detail.html` | S28 Post Detail | Reply thread + composer |

## Demo tips baked into the prototype

- **Error states:** wrong password on Login, duplicate email on Fan Sign Up, declined card on Checkout, Kids Mode blocking Adult content in the Player.
- **Success states:** toast notifications appear for nearly every action (follow, save, tip, publish, subscribe, cancel, reply, etc.) so it reads as a live app rather than a static mockup.
- **Persistence:** actions taken (following a comedian, upgrading to Premium, switching Kids/Adult Mode) carry across screens because they're saved to `localStorage`. Clearing site data / using a private window resets the demo to its defaults.
- **Reset the demo:** clear `localStorage` for the file's origin (DevTools → Application → Local Storage → delete `ct_state_v1`), or open in a new private/incognito window.

## What this is not

This is a presentation prototype, not production code. There's no real authentication, no real Stripe/Mux/Daily.co integration, and no server. When the GHL build begins, use `ComicTube_MASTER_SPEC_v1.md` as the source of truth for actual behavior, data fields, and workflows — this prototype is only meant to let Abe see and click through what the finished experience will feel like.
