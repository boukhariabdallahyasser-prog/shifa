# SHIFAA — Vanilla JavaScript SPA

A responsive single-page frontend built with plain HTML, CSS and JavaScript. No React, Tailwind or UI kits. Lucide is loaded as the single icon library through its pinned browser CDN build.

## Run locally

Open `index.html` in a browser. For a local workspace preview, use:

- `index.html?preview=doctor`
- `index.html?preview=patient`
- `index.html?preview=1` to choose a workspace from the landing page

The preview uses empty data collections. Records created in preview mode exist only in memory and disappear on refresh. This avoids shipping fabricated patient examples or treating demo records as real clinical data.

## Visual system

- Lucide icons are used consistently for navigation, actions and patient record categories. The browser needs an internet connection to load the pinned Lucide script from unpkg.
- The Patient workspace includes a care overview, account-linked summaries, health-file categories, medical information states, referral tracking, visits and a care timeline.
- Patient records are filtered by the authenticated account identifier in the frontend. This is a presentation safeguard only; the Laravel API must enforce authorization on every endpoint.

## Files

- `index.html` — app entry point and Lucide browser script
- `styles.css` — responsive design system and layouts
- `app.js` — SPA navigation, screen rendering, forms and preview interactions

## Backend integration

The frontend expects a Laravel API at `/api` by default. Set `window.SHIFAA_API_BASE` before loading `app.js` if the API uses another base URL.

Expected authentication endpoints used by this frontend:

- `POST /api/auth/login` — `{ identifier, password }`; response includes `{ user: { role, name, id } }`
- `POST /api/auth/activate` — `{ identifier, temporaryPassword, newPassword }`
- `POST /api/auth/password/forgot` — `{ identifier }`
- `POST /api/auth/password/change` — `{ currentPassword, newPassword }`

For production, patient creation, referral creation, encounters, follow-ups, notifications and document uploads should be persisted through authenticated Laravel endpoints. The current in-browser create/edit interactions are preview-only; they are not a secure database or clinical record system.

## SHIFAA workflow constraints represented

- One shared sign-in screen; role comes from the authenticated account response.
- Doctor accounts are provisioned by an administrator outside this application scope.
- Doctors create patient accounts using the patient's identification number; a temporary password is generated in the preview flow.
- Patients activate their account by setting a private password.
- Clinical encounters represent documented visits, not appointments.
- Referrals are care handoffs tracked through SHIFAA Loop.
- No doctor directory, doctor discovery, maps, appointment booking or admin dashboard.

## Important security note

This is a frontend prototype. Do not use it with real patient information until backend authorization, server-side validation, audit logging, encryption, secure file storage, session management and applicable healthcare privacy requirements are implemented and reviewed.
