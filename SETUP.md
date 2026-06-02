# 🔧 Setup Guide — Profile v2 Integrations

This guide covers the manual setup required to enable all dynamic sections in your GitHub profile README.

---

## Table of Contents

1. [GitHub Secrets (Required for Workflows)](#1-github-secrets)
2. [📊 GitHub Metrics (lowlighter/metrics)](#2-github-metrics)
3. [⏱ WakaTime Stats](#3-wakatime-stats)
4. [📝 Blog Posts](#4-blog-posts)
5. [🎧 Spotify Now Playing](#5-spotify-now-playing)

---

## 1. GitHub Secrets

Three of the workflows need secrets stored in your repository. These are encrypted and safe.

### Where to add them

1. Go to your repo: https://github.com/AadityaBhuree/AadityaBhuree
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret** for each one below

### Secrets to create

| Secret Name | Value | Purpose |
|---|---|---|
| `WAKATIME_API_KEY` | Your WakaTime API key (see §3) | WakaTime coding stats |
| `METRICS_TOKEN` | GitHub classic PAT (see §2) | GitHub Metrics widget |

### Required workflow permission

1. Go to **Settings** → **Actions** → **General**
2. Under **Workflow permissions**, select **Read and write permissions**
3. Click **Save**

---

## 2. 📊 GitHub Metrics

Generates a rich metrics dashboard SVG every Sunday.

### Step 1: Create a classic PAT

1. Go to https://github.com/settings/tokens
2. Click **Generate new token** → **Generate new token (classic)**
3. Give it a name like `METRICS_TOKEN`
4. Set expiration to **No expiration** (so you don't have to redo this)
5. Scopes needed:
   - `repo` (Full control of private repositories)
   - `read:user` (Read user profile data)
   - `read:org` (Read organization data — optional)
6. Click **Generate token**
7. **Copy the token immediately** — you won't see it again

### Step 2: Add to GitHub Secrets

Add the token as `METRICS_TOKEN` in your repo secrets (see §1).

### Step 3: Trigger the workflow

1. Go to your repo → **Actions** tab
2. Find **📊 GitHub Metrics** in the left sidebar
3. Click **Run workflow** → **Run workflow**
4. Wait ~30 seconds, then refresh. It should generate `github-metrics.svg` in your repo root.
5. The README will pick it up automatically.

---

## 3. ⏱ WakaTime Stats

Shows your weekly coding breakdown (languages, editors, operating systems) directly in your README.

### Step 1: Sign up for WakaTime

1. Go to https://wakatime.com/signup and create a free account
2. Verify your email

### Step 2: Install the WakaTime plugin

1. Open VS Code
2. Go to Extensions (`Ctrl+Shift+X`)
3. Search for **WakaTime** and install it
4. You'll be prompted to enter your API key (get it in the next step)

### Step 3: Get your API key

1. Go to https://wakatime.com/settings/api-key
2. Copy your API key
3. If using VS Code, paste it when prompted (or press `Ctrl+Shift+P` → `WakaTime: API Key`)

### Step 4: Add to GitHub Secrets

Add the key as `WAKATIME_API_KEY` in your repo secrets (see §1).

### Step 5: Code for a few days

WakaTime needs data to show. Code normally — the workflow runs daily and will start showing stats once you have some coding time logged.

### Step 6: Trigger the workflow (optional)

If you want to test it immediately:
1. Go to your repo → **Actions** tab
2. Find **⏱ WakaTime Stats**
3. Click **Run workflow** → **Run workflow**
4. If you have logged time, it'll appear in the `~/.coding` section

---

## 4. 📝 Blog Posts

Auto-fetches your latest 5 blog posts from Medium or Dev.to.

### Option A: Medium

If you write on Medium:
1. Your feed URL is: `https://medium.com/feed/@your-username`
2. Already pre-configured as `https://medium.com/feed/@aadityabhure03`

### Option B: Dev.to

If you write on Dev.to:
1. Your feed URL is: `https://dev.to/feed/your-username`
2. Already pre-configured as `https://dev.to/feed/aadityabhure03`

### To customize the feeds

Edit `.github/workflows/blog-post-workflow.yml` and update:

```yaml
feed_list: "https://medium.com/feed/@your-username,https://dev.to/feed/your-username"
```

### To trigger

1. Go to your repo → **Actions** tab
2. Find **📝 Latest Blog Posts**
3. Click **Run workflow** → **Run workflow**
4. It will read your RSS feeds and inject your latest posts between the `<!-- BLOG-POST-LIST:START -->` tags in README.md

---

## 5. 🎧 Spotify Now Playing

Shows what you're currently listening to — updates in real time.

This requires deploying your own backend service. The recommended option is **novatorem**.

### Step 1: Fork and deploy novatorem

The easiest way is using the one-click deploy:

1. Go to https://github.com/novatorem/novatorem
2. Click the **Deploy to Vercel** button
3. You'll need:
   - A Vercel account (free — https://vercel.com)
   - A Spotify Developer account

### Step 2: Create a Spotify App

1. Go to https://developer.spotify.com/dashboard
2. Click **Create app**
3. Name it (e.g., "GitHub Profile")
4. Add `https://your-app.vercel.app` to **Redirect URIs** (use your actual Vercel URL)
5. Save your **Client ID** and **Client Secret**

### Step 3: Configure and deploy

Follow the novatorem README to:
1. Set up environment variables in Vercel (Client ID, Client Secret, etc.)
2. Generate a Refresh Token
3. Deploy

### Step 4: Update the embed URL

Once deployed, you'll get a URL like:
```
https://your-app.vercel.app/api/now-playing
```

Replace the `img src` URL in the `~/.now_playing` section of `README.md` with your deployed URL.

### Step 5: Find your Spotify UID

1. Open Spotify Desktop or Web
2. Click your profile → **Account**
3. Your username/UID is shown there
4. Replace `YOUR_SPOTIFY_UID` in the Spotify link in README.md

---

## Quick Reference — Workflow Triggers

| Workflow | Schedule | Manual Trigger |
|---|---|---|
| 📊 GitHub Metrics | Every Sunday at 00:00 UTC | Actions → Run workflow |
| ⏱ WakaTime Stats | Daily at 00:00 UTC | Actions → Run workflow |
| 📝 Latest Blog Posts | Daily at 00:00 UTC | Actions → Run workflow |
| 🐍 Snake Animation | Every 12 hours | Actions → Run workflow |
| 📊 Refresh Profile | Every Sunday at 00:00 UTC | Actions → Run workflow |

---

## Troubleshooting

### Metrics SVG shows as broken image
- Make sure `METRICS_TOKEN` secret is set correctly
- The workflow runs weekly — trigger it manually to generate immediately
- Check the workflow run logs for errors

### WakaTime shows no data
- WakaTime needs at least a few hours of coding tracked
- Make sure the VS Code plugin is connected
- Verify `WAKATIME_API_KEY` secret is correct
- The workflow updates daily — trigger it manually to test

### Blog posts not showing
- Verify your RSS feed URLs are correct
- Make sure you have published at least one post
- Check the workflow run logs for feed parsing errors

### Spotify card not rendering
- Make sure your novatorem instance is deployed and accessible
- Verify the URL in the `img src` points to your deployed instance
- Check that your Spotify account is currently playing something (it shows nothing when idle)

---

> **Need help?** Open an issue on this repo or reach out at aadityabhure03@gmail.com
