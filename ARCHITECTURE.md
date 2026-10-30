# Architecture

A public-safe view of how Travelje is built. No internal URLs, credentials or infrastructure identifiers.

## One web app

Travelje is a single Next.js application (App Router, React, TypeScript, Tailwind CSS). Pages and API routes live in the same app and deploy together as serverless functions.

## Offline first

A trip lives on the device first, in browser storage, so the planner works with no account and no connection. When a user signs in, trips sync to the server. Each item in a trip (day, expense, document) carries its own change time, so edits made on two devices merge item by item instead of one device overwriting the other. Deleted items are remembered for a while, so a delete on one device is not undone by an older copy on another.

## Live prices with no paid APIs

Flight fares, hotel prices, exchange rates, weather and public holidays come from free public sources. Results are cached, and a scheduled job warms the fares travellers ask for most, so the planner answers quickly. Every price shows when it was checked.

## Accounts and teams

- Email and password accounts (new accounts confirm their email first) and Google sign-in.
- Optional two-factor login with an authenticator app.
- Workspaces: a personal one for every user, plus company workspaces with roles (owner, admin, editor, viewer) and an approval step for trips.
- Every server query is scoped to the caller's workspace, and admin actions are audited.

## Data

Postgres, with the schema in numbered SQL migrations applied in order. Optional Redis for shared rate limits. Email for account mail, trip reminders and price alerts.

## Quality gates

Type checks, a lint with zero warnings, small assert scripts for the pure logic (money, dates, merging, roles), and browser end-to-end tests that run before every deploy.
