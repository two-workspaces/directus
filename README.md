# Deploy and Host Directus on Railway

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new/template/directus-production?referralCode=sLjDXb&utm_medium=integration&utm_source=button&utm_campaign=directus-production)

[Directus](https://github.com/directus/directus) turns a SQL database into a headless CMS and backend: instant REST and GraphQL APIs, a no-code admin app for editors, roles and permissions, file management, flows and realtime updates. This template runs Directus 12 on Postgres with PostGIS, and stores uploads in a Railway bucket.

## About Hosting Directus

Three pieces: the Directus service, a PostGIS database with a volume, and a Railway bucket.

- **Admin ready at first boot.** The admin account is created from `ADMIN_EMAIL` and `ADMIN_PASSWORD`, both in the Variables tab, so you don't need to dig the first password out of the logs.
- **Files in object storage.** Uploads and marketplace extensions go to a Railway bucket, so the Directus service keeps no state and redeploys never lose files.
- **Geodata included.** The database is Postgres 17 with PostGIS, so Directus's map and geometry fields work out of the box.
- **No idle extras.** A single Directus instance doesn't need Redis, so the template leaves it out: three pieces to pay for instead of four. Add Redis only when you run several Directus replicas.

## Common Use Cases

- A headless CMS for Next.js, Nuxt, SvelteKit or Astro sites and mobile apps
- A backend for internal tools: data models, permissions and an admin UI without writing an API
- A no-code admin panel on top of data your team already has in Postgres

## Dependencies for Directus Hosting

- PostGIS (Postgres 17), included
- A Railway bucket for files, included

### Deployment Dependencies

- [Directus on GitHub](https://github.com/directus/directus)
- [Directus documentation](https://directus.io/docs)
- [Directus configuration reference](https://directus.io/docs/configuration/general)
- [Template source on GitHub](https://github.com/two-workspaces/directus)

### Implementation Details

**Sign in.** Open the Directus service's domain and sign in with `ADMIN_EMAIL` and `ADMIN_PASSWORD` from its Variables tab. Change both in your user profile afterwards; the variables only seed the first admin.

**Use the API.** Create a static token for your user (User Directory > your user > Token) and send it as a Bearer token:

```bash
curl https://<your-app>.up.railway.app/items/posts \
  -H "Authorization: Bearer <token>"
```

GraphQL is at `/graphql`. Apps in the same Railway project can call `http://${{Directus.RAILWAY_PRIVATE_DOMAIN}}:8055` instead; that traffic isn't billed as egress.

**Email.** Password resets and invites need SMTP. Add `EMAIL_TRANSPORT=smtp` and the `EMAIL_SMTP_*` variables from the [email docs](https://directus.io/docs/configuration/email). Railway allows outbound SMTP on the Pro plan; on Hobby, use an email API or create users from the admin app.

**Resources and cost.** Measured on Railway: Directus idles at about 245 MB of RAM and PostGIS at about 210 MB, so the stack fits the Hobby plan.

**Updates.** Change the Directus image tag to a newer `directus/directus:<version>` and redeploy; Directus runs its database migrations at start-up. Back up the database before major version jumps.

**License.** Directus 12 is released under the [Monospace Sustainable Core License](https://github.com/directus/directus/blob/main/license): you can use it for any purpose, commercial included, except offering Directus itself as a competing paid service.

## Why Deploy Directus on Railway?

Railway is a singular platform to deploy your infrastructure stack. Railway will host your infrastructure so you don't have to deal with configuration, while allowing you to vertically and horizontally scale it.

By deploying Directus on Railway, you are one step closer to supporting a complete full-stack application with minimal burden. Host your servers, databases, AI agents, and more on Railway.
