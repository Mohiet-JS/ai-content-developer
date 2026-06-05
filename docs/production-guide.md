# AuraScribe AI - Production API Setup & Deployment Guide

To make the auto-poster actually connect to your channels and upload real video assets to **YouTube Shorts** and **Instagram Reels**, you need to replace the simulated database logic with real developer API integrations.

---

## 1. Setup YouTube Data API v3 (Google Cloud Console)

To post programmatically to YouTube, you must configure a Google Cloud Console application:

1. **Create Google Project**:
   - Visit the [Google Cloud Console](https://console.cloud.google.com/).
   - Click **Create Project** and name it `AuraScribe Auto-Poster`.
2. **Enable YouTube API**:
   - Go to **API & Services** > **Library**.
   - Search for **YouTube Data API v3** and click **Enable**.
3. **Configure OAuth Consent Screen**:
   - Go to **OAuth Consent Screen** (User Type: External).
   - Set app name, email, and add the scope: `.../auth/youtube.upload` (necessary to write/upload videos).
   - Add your Gmail account as a **Test User** (since the app will run in testing mode).
4. **Obtain Credentials**:
   - Go to **Credentials** > **Create Credentials** > **OAuth client ID**.
   - Application type: **Web Application** (or **Desktop Application** for simple local scripts).
   - Save the **Client ID** and **Client Secret**.
   - Set Authorized redirect URIs (e.g. `http://localhost:3000/oauth2callback`).

---

## 2. Setup Instagram Graph API (Meta Developers Portal)

Posting Reels programmatically requires an Instagram Creator/Business Account linked to a Facebook Page:

1. **Register Meta Developer App**:
   - Visit the [Meta for Developers Portal](https://developers.facebook.com/).
   - Create a **Business App**.
2. **Link accounts**:
   - Ensure your Instagram account is set to **Business** or **Creator** profile.
   - Link that Instagram profile to a Facebook Page which you admin.
3. **Add Permissions & Product**:
   - Add the **Instagram Graph API** product to your app.
   - Request permissions:
     - `instagram_basic`
     - `instagram_content_publish` (specifically required to upload Reels/posts)
     - `pages_show_list`
     - `pages_read_engagement`
4. **Obtain Access Tokens**:
   - Use the **Graph API Explorer** to generate a short-lived User Access Token.
   - Exchange it for a **Long-Lived User Access Token** (valid for 60 days) or a permanent **Page Access Token** via the Meta OAuth endpoints.

---

## 3. Programmatic Video Rendering (Optional)

In production, to turn scripts and visual assets into an actual `.mp4` video automatically, you can use:

1. **Remotion** (React & HTML5 Canvas to MP4):
   - A framework that allows you to write video files in React code.
   - Can render videos programmatically in the background using Puppeteer and FFmpeg.
   - Run `npx remotion render src/index.ts MyVideo out.mp4`.
2. **FFmpeg Shell Execution**:
   - Combine a static background image or template clips with an audio file (using Text-to-Speech engines like ElevenLabs or Google TTS) to compile standard MP4 packets.

---

## 4. Install Node Libraries

Install the actual integration SDKs in the AuraScribe project folder:

```bash
npm install googleapis axios dotenv fluent-ffmpeg
```

Create a `.env` file in the root of the project to store your secret tokens securely:

```env
# Google OAuth (YouTube)
YOUTUBE_CLIENT_ID=your_google_client_id.apps.googleusercontent.com
YOUTUBE_CLIENT_SECRET=your_google_client_secret
YOUTUBE_REFRESH_TOKEN=your_google_oauth_refresh_token

# Meta Graph API (Instagram)
INSTAGRAM_ACCOUNT_ID=your_instagram_business_account_id
INSTAGRAM_ACCESS_TOKEN=your_meta_long_lived_access_token
```
