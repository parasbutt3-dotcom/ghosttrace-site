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
