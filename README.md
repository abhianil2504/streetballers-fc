# Streetballers FC — Shared Website Setup

This version uses Supabase for shared data, image storage, and administrator login. Netlify continues to host the website.

## 1. Create Supabase project

1. Go to https://supabase.com and create a free project.
2. Open **SQL Editor**, create a new query, paste all of `supabase-setup.sql`, and click **Run**.

## 2. Create the shared administrator account

1. In Supabase, open **Authentication → Users**.
2. Click **Add user → Create new user**.
3. Use a shared email such as `admin@streetballersfc.com` and a strong password.
4. Enable **Auto Confirm User** when creating it.

The email acts as the common administrator username.

## 3. Connect the website

1. In Supabase, open **Project Settings → API**.
2. Copy the **Project URL** and the **anon/public key**.
3. Open `index.html` in a text editor.
4. Replace:

   `YOUR_SUPABASE_URL`

   `YOUR_SUPABASE_ANON_KEY`

Do not use or expose the `service_role` key.

## 4. Deploy to Netlify

1. Put `index.html` in a folder.
2. Drag that folder or this ZIP into **Netlify → Deploys**.
3. Open the live link and press `Ctrl + F5` once.

## 5. Test shared visibility

1. Log in as admin and publish a test article with a photo.
2. Open the website in an Incognito window or another phone.
3. The article and photo should appear there without logging in.

## Important

Anyone who knows the shared email and password has full admin access. Use a strong password and change it in Supabase Authentication whenever access should be revoked.

## Match goals and assists

Admins can open **Admin Dashboard → Match Statistics**, select a tournament and fixture, and enter each player's goals and assists. The active tournament page includes toggle buttons for **Standings**, **Top Scorers**, and **Top Assists**. These values are stored in the existing `app_data` table, so no additional SQL migration is required.

## Multiple photos in articles

The article publishing form now accepts up to 10 images per article. Each image must be under 6 MB. The first selected image is used as the article cover, and all selected images appear in the detailed article view without cropping.

## Tournament lifecycle update

Admins can now manage the tournament lifecycle from **Admin Dashboard → Tournaments**:

- **Start Tournament** makes an upcoming tournament the single current tournament, creates a fresh standings table for its teams, and resets award winners for the new competition.
- **Close Tournament** snapshots the current standings and award winners, marks the tournament as Past, and preserves it under Past Tournaments.
- The public Tournament page, homepage tournament labels, standings, leaders, honours, and fixture showcase now follow the active tournament automatically.
- **Upcoming Fixtures** only shows fixtures whose scheduled start time is still in the future. Fixture comparisons use India Standard Time (Asia/Kolkata) and refresh every minute while the site is open.
- A second tournament cannot be started while another tournament is active; close the current tournament first.

## Standings scoring
The tournament table uses: Win = 2 points, Draw = 1 point, Loss = 0 points. Played (P), Goal Difference (GD), and Points (PTS) are calculated automatically. GS means Goals Scored; GA means Goals Against; GD = GS - GA. Standings sort by PTS, then GD, then GS.
