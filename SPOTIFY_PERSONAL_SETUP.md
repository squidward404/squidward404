# Personal Live Spotify Widget Setup (MyPlaying)

This guide makes the Spotify card in your profile use your own playback, not a shared public instance.

## Architecture

You need 2 running services:
1. spotify-websocket-client (your private Spotify listener)
2. MyPlaying (renders the SVG widget for README)

MyPlaying reads live track updates from spotify-websocket-client.

## Step 1: Create Spotify App

1. Open Spotify Developer Dashboard.
2. Create an app.
3. Add redirect URI:
   - http://localhost:35679/callback
4. Save these values:
   - SPOTIFY_CLIENT_ID
   - SPOTIFY_CLIENT_SECRET

## Step 2: Run spotify-websocket-client

Use this repo:
- https://github.com/busybox11/spotify-websocket-client

Required env vars:
- SPOTIFY_REDIRECT_URI=http://localhost:35679/callback
- SPOTIFY_CLIENT_ID=...
- SPOTIFY_CLIENT_SECRET=...
- REFRESH_TOKEN=... (generated after login)

Flow:
1. Install dependencies
2. Run login.ts once to generate REFRESH_TOKEN
3. Start websocket server
4. Expose this websocket service publicly

You will get a public websocket host, for example:
- your-ws-host.example.com:80

## Step 3: Deploy MyPlaying

Use this repo:
- https://github.com/busybox11/MyPlaying

Required env vars from example.env:
- SERVER_PORT=7089
- SPTWSS_URL=your-ws-host.example.com:80
- LASTFM_USER=optional
- LASTFM_API_KEY=optional
- LASTFM_SECRET=optional
- SPOTIFY_PROFILE_URL=https://open.spotify.com/user/31hgawktqptlttvxyb2ke3yzyz4e

After deployment, your SVG endpoint is:
- https://your-myplaying-domain/playing/img?hideGithubLogo

## Step 4: Update Profile README

Replace Spotify image source in README with your own domain:
- https://your-myplaying-domain/playing/img?hideGithubLogo

Current profile link for your account:
- https://open.spotify.com/user/31hgawktqptlttvxyb2ke3yzyz4e

## Quick Verify

Open these URLs in browser:
1. https://your-myplaying-domain/
2. https://your-myplaying-domain/playing
3. https://your-myplaying-domain/playing/img?hideGithubLogo

If #3 returns SVG, it is ready for GitHub README.
