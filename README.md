# Black Diamond Motors — Cinematic Netlify Edition

A responsive luxury automotive showroom with a custom interactive Three.js vehicle scene, cinematic motion, inventory cards, individual vehicle pages, an enquiry form, and a password-protected inventory console.

## Important before launch

- This is a deployable starter project, not a security audit or a managed hosting service.
- The 3D showroom vehicle is procedurally modelled from Three.js geometry. It is an original stylised grand-tourer visual, not a photorealistic manufacturer model. A licensed GLB/GLTF model can be substituted later.
- The page uses externally hosted Unsplash photography and Google Fonts. Replace these with images and fonts you have permission to use before commercial launch.
- The three sample vehicles are demonstration content. Replace them with real inventory and accurate details.
- Update the sample contact email in `public/index.html` before publishing.
- The inventory and enquiry features require a Supabase project and Netlify environment variables. The public site can render demo inventory without Supabase, but enquiry submission and admin inventory operations will not work until the database is connected.

## Deploy on Netlify — beginner steps

### 1. Create the database

1. Create a project at [Supabase](https://supabase.com/).
2. In the Supabase project, open **SQL Editor**.
3. Choose **New query**, open `supabase/schema.sql` from this project, copy all its contents into the editor, and click **Run**.
4. Open **Project Settings → API**. Copy the Project URL and the `service_role` key. Keep the service-role key private. Never put it in `public/` or commit it to GitHub.

### 2. Put the project on GitHub

1. Download and extract this ZIP on your computer.
2. Create a new, empty GitHub repository.
3. Upload the **contents** of the `black_diamond_motors_cinematic` folder to the repository root. Do not upload only the outer folder as a nested folder.
4. At the repository's top level you should see `netlify.toml`, `package.json`, `README.md`, `public`, `netlify`, and `supabase`.

### 3. Connect GitHub to Netlify

1. Sign in at [Netlify](https://app.netlify.com/).
2. Select **Add new project → Import an existing project**.
3. Choose GitHub and select your repository.
4. Netlify should read the included `netlify.toml`. Build command is `npm run build`; publish directory is `public`. Functions directory is `netlify/functions`.
5. Deploy the site.

**Do not use Render's Blueprint deployment for this project.** This project is configured for Netlify with `netlify.toml`; it does not need a `render.yaml` Blueprint file.

### 4. Add the private environment variables

In Netlify, open **Site configuration → Environment variables** (sometimes shown under **Build & deploy → Environment**). Add these variables:

| Variable | Value |
|---|---|
| `SUPABASE_URL` | Your Supabase Project URL |
| `SUPABASE_SERVICE_ROLE_KEY` | Your Supabase `service_role` key |
| `ADMIN_PASSWORD` | A long, unique password for the admin console |
| `JWT_SECRET` | A separate long, random secret used to sign admin sessions |

Do not use the same value for `ADMIN_PASSWORD` and `JWT_SECRET`. Do not paste either secret into frontend code. After saving the variables, trigger a fresh deploy so the Functions receive them.

### 5. Open and test the website

- Public website: your Netlify site URL.
- Admin console: `https://YOUR-SITE.netlify.app/admin.html`.
- Sign in with the `ADMIN_PASSWORD` value you configured.
- Add a test vehicle, then check that it appears in the public Collection.
- Submit a test enquiry and check that it appears in the admin Enquiries table.

## Admin console

The admin console supports:
- Password sign-in (session token expires after 8 hours).
- Add, edit and delete vehicles.
- Set year, price, mileage, transmission, fuel, category, image URL, description, featured and availability fields.
- Review customer enquiries.

## Before accepting real customer information

Use a strong unique admin password and JWT secret; restrict access to the Netlify and Supabase accounts; enable MFA; add spam protection/rate limiting and an email notification workflow; publish a privacy notice and terms; configure database backups; test the site on mobile and desktop; verify all images, vehicle information and contact details; and have the implementation reviewed for your local privacy and consumer-protection requirements.

## Local preview

Install Node.js, then from the project folder run:

```bash
npm install
npx netlify dev
```

The Netlify CLI runs the static site and Functions locally. Add local environment variables using a `.env` file (never commit it). Copy `.env.example` to `.env` and replace every placeholder with your own values.
