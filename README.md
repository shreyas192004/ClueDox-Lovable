# Welcome to your Lovable project

## Project info

**URL**: https://lovable.dev/projects/REPLACE_WITH_PROJECT_ID

## How can I edit this code?

There are several ways of editing your application.

**Use Lovable**

Simply visit the [Lovable Project](https://lovable.dev/projects/REPLACE_WITH_PROJECT_ID) and start prompting.

Changes made via Lovable will be committed automatically to this repo.

**Use your preferred IDE**

If you want to work locally using your own IDE, you can clone this repo and push changes. Pushed changes will also be reflected in Lovable.

The only requirement is having Node.js & npm installed - [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating)

Follow these steps:

```sh
# Step 1: Clone the repository using the project's Git URL.
git clone <YOUR_GIT_URL>

# Step 2: Navigate to the project directory.
cd <YOUR_PROJECT_NAME>

# Step 3: Install the necessary dependencies.
npm i

# Step 4: Start the development server with auto-reloading and an instant preview.
npm run dev
```

**Edit a file directly in GitHub**

- Navigate to the desired file(s).
- Click the "Edit" button (pencil icon) at the top right of the file view.
- Make your changes and commit the changes.

**Use GitHub Codespaces**

- Navigate to the main page of your repository.
- Click on the "Code" button (green button) near the top right.
- Select the "Codespaces" tab.
- Click on "New codespace" to launch a new Codespace environment.
- Edit files directly within the Codespace and commit and push your changes once you're done.

## What technologies are used for this project?

This project is built with:

- Vite
- TypeScript
- React
- shadcn-ui
- Tailwind CSS

## How can I deploy this project?

Simply open [Lovable](https://lovable.dev/projects/REPLACE_WITH_PROJECT_ID) and click on Share -> Publish.

## Can I connect a custom domain to my Lovable project?

Yes, you can!

To connect a domain, navigate to Project > Settings > Domains and click Connect Domain.

Read more here: [Setting up a custom domain](https://docs.lovable.dev/features/custom-domain#custom-domain)

## Backup Supabase (Dual Write)

This project now supports optional backup mirroring from the primary project into a second Supabase project.

- Primary flow remains default (upload + AI extraction in primary project).
- Backup project receives:
	- file binary in storage bucket (default: `files`)
	- `files` row
	- extracted metadata (`ai_summary`, `entities`, `semantic_keywords`, etc.)
	- `tags` + `file_tags`
- AI extraction does **not** run in backup.

### 1) CLI auth / init / link

Run these in your project folder:

```powershell
supabase login
supabase init
supabase link --project-ref pnwvmepfhteejjpcffos
```

Use your backup project ref only for backup credentials (below): `qsulolldyzdlklhsasza`.

### 2) Set edge-function secrets (primary project)

You need backup project URL + service role key from your backup Supabase project settings.

```powershell
supabase secrets set BACKUP_SUPABASE_URL="https://qsulolldyzdlklhsasza.supabase.co"
supabase secrets set BACKUP_SUPABASE_SERVICE_ROLE_KEY="<YOUR_BACKUP_SERVICE_ROLE_KEY>"
supabase secrets set BACKUP_SUPABASE_STORAGE_BUCKET="files"
```

### 3) Deploy updated function

```powershell
supabase functions deploy analyze-file
```

### 4) Important schema note

Ensure backup project has matching schema/tables/bucket used here:

- `files`
- `tags`
- `file_tags`
- storage bucket `files`

If schema differs, backup writes will be logged but primary uploads will continue.
