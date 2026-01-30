# Slack app setup (current as of 2025)

Slack’s UI is manifest-based now. Use one of these approaches.

---

## Option A – Fix “no bot user” on an existing app (e.g. HumourBot)

1. Go to **https://api.slack.com/apps** and open your app.
2. In the left sidebar click **App Manifest** (not “Bot Users” — that’s legacy and may be gone).
3. The editor shows your app’s current manifest (often YAML or JSON).
4. Add a **bot user** by ensuring the manifest has a `features` block with `bot_user`:
   - In **JSON**, add something like:
     ```json
     "features": {
       "bot_user": {
         "display_name": "HumourBot",
         "always_online": true
       }
     }
     ```
   - If you already have `features` (e.g. `slash_commands`), add `bot_user` inside that same `features` object.
5. Click **Save** (and resolve any validation errors if Slack shows them).
6. Go to **Install App** (or **OAuth & Permissions** → Install to Workspace) and install/reinstall the app.

The “doesn’t have a bot user to install” error should go away once the manifest includes `bot_user` and you reinstall.

---

## Option B – Create a new app from the full manifest (recommended)

1. Go to **https://api.slack.com/apps** → **Create New App** → **From an app manifest**.
2. Choose **Your workspace** → **Next**.
3. Choose **JSON** and paste the **entire** contents of **`bot/slack-app-manifest.json`** from this repo.
4. **Next** → **Create**.
5. **Install to Workspace**.
6. Copy **Bot User OAuth Token** (OAuth & Permissions) and **Signing Secret** (Basic Information) into `bot/.env`.
7. Under **Slash Commands** → **/meme**, set **Request URL** to your app’s URL (e.g. `https://your-ngrok-url.ngrok.io/slack/events` when using Bolt).

This creates the app with bot user, `/meme` command, and scopes in one go.

---

## If you still see “Bot Users” in the sidebar

Some workspaces still show the legacy **Bot Users** item. Then:

1. Click **Bot Users** in the left sidebar.
2. Click **Add Legacy Bot User** (or **Add a Bot User**).
3. Set **Display name** and **Default username** → **Add** → **Save Changes**.
4. **Install App** (or Reinstall) to the workspace.

Note: Slack is deprecating legacy custom bots (see [changelog](https://api.slack.com/changelog/2024-09-legacy-custom-bots-classic-apps-deprecation)). Prefer **Option B** (create from full manifest) for a new app.
