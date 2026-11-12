# GHOSTTRACE — Google OAuth verification demo video plan

Checklist + shot list for the YouTube demo required by Google's sensitive-scope
verification (`gmail.readonly`, `calendar.readonly`, `drive.readonly`).
Everything shown here must match [privacy.html](privacy.html) — reviewers cross-check
the video, the privacy policy and the scope justification text, and mismatches are the
#1 rejection cause.

## Setup (before recording)

- [ ] **Staging instance** (their option 1): separate deployment, e.g. a Vercel preview
      or `staging.ghosttrace.app`, with the Google connect flow behind a test route.
      Never demo unverified scopes on production traffic.
- [ ] **Test user**: your Google account added under OAuth consent screen → Test users.
      The unverified-app warning still appears for test accounts — that is required on video.
- [ ] **Consent screen branding**: logo = `logo.png` (512×512, 26 KB — well under the 2 MB cap),
      app domain verified (Search Console), Authorized domains include the staging domain.
- [ ] **Policy links on consent screen**:
      Privacy → `https://parasbutt3-dotcom.github.io/ghosttrace-site/privacy.html`
      Terms → `https://parasbutt3-dotcom.github.io/ghosttrace-site/terms.html`
- [ ] **All OAuth clients** in the project appear in the video (web + Android + iOS,
      whichever exist). Demoing only one client gets rejected.
- [ ] OBS (or similar) at 1080p, system audio muted, browser URL bar visible throughout.

## Shot list (~3–4 minutes, one take)

| # | Screen | Show / say |
|---|--------|-----------|
| 1 | Landing page, URL bar visible | "GHOSTTRACE is self-hosted open-source software; this is my staging instance." |
| 2 | Settings → Connect Google → OAuth consent screen **including the unverified-app warning** | Name each scope as it appears: `openid email profile`, then optional `gmail.readonly`, `calendar.readonly`, `drive.readonly` |
| 3 | Timeline after auth | Basic profile scope → account name + email shown in the header only |
| 4 | Gmail scope | Timeline entry = **subject + date** of a `no-reply@accounts.google.com` security alert. Open the stored record in the admin/DB view to prove no message body exists. |
| 5 | Calendar scope | Event **title, time, location** of a recent event on the timeline |
| 6 | Drive scope | **File name + modified time** entry only. State: "file contents are never fetched." |
| 7 | Companion app(s) | Android/iOS flow; show the OS **location permission prompt** (backs the "only with explicit permission" claim) |
| 8 | Sessions page | Revoke one session; show it dead immediately |
| 9 | Settings → Delete account | Confirm; show all collected events gone + Google token revoked |
| 10 | End card | Repo URL + privacy URL |

## Scope justification text (form field — keep policy-identical)

> `gmail.readonly` — GHOSTTRACE reads only security-alert emails from
> no-reply@accounts.google.com and stores subject line + date; no message bodies.
> `calendar.readonly` — title, time, location of recent events, for a personal activity
> timeline. `drive.readonly` — file names and modified times only, never contents.
> All processing occurs on the user's self-hosted instance; there is no central server.
> Account deletion revokes tokens and cascades event deletion (default retention 90 days).

## Gotchas

- Do not edit out or skip past the unverified-app warning screen.
- Do not hit production traffic with unverified scopes (quota + user disruption).
- Keep it under ~5 minutes; every scope requested must have a "used in the app" moment.
- Upload **unlisted** on YouTube, paste the link into the verification form.

## App audit — localhost:3001, 2026-11-12 (read-only walkthrough)

**Ready to demo (matches the policy as written):**

- `/integrations` — per-scope consent checkboxes ("Gmail security alerts", "Calendar events",
  "Recent Drive files") = the "optional, separately consented" claim, on screen.
- `/timeline` — search + source filter; a `token.scopes` event literally logs the granted
  scope list; `account.identity` event shows name/email handling.
- `/security` — audit log (`auth.login`, `auth.google_connected` with scopes array) and the
  detection-rule builder (event type / source / threshold / severity).
- `/insights` — hour-of-day + weekday heatmaps, streak, cross-account signals.
- `/settings` — delete-account copy matches the policy verbatim ("Connected Google tokens
  are revoked first", permanent, cascades); API keys stored hashed; password change signs
  out every session.

**Blockers before recording:**

1. **Per-session revocation missing** — policy promises it; the UI only has "Sign out
   everywhere". Add a revoke button per row in Security Center → Active sessions.
2. **Retention setting missing** — policy says "configurable, default 90 days"; no
   retention field exists in Settings. Add it (or amend the policy).
3. **Sensitive scopes not yet granted** — currently only `openid email profile`.
   Reconnect with the Gmail/Calendar/Drive checkboxes on, and have a
   `no-reply@accounts.google.com` security alert in the inbox beforehand (shot 4).
4. **No companion device paired** — Device Intelligence shows "No devices registered";
   shots 6–7 need one device sending battery/network/location telemetry.
5. **Running in dev mode** — the Next.js dev-tools overlay and issues badge are on
   screen. Record from a production build (`next build && next start`) or a Vercel
   preview, and ideally serve frontend + API from one origin (Reports currently links
   the API at `:8000` while the app is on `:3001`).
