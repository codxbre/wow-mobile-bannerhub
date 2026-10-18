# WoW on Android — BannerHub background build (unofficial)

An unofficial **BannerHub v3.8.1 (Normal)** build for playing **World of Warcraft 3.3.5a**
(Warmane / Icecrown) on Android. It adds two things over stock BannerHub:

- **Background keep-alive** — the Wine session is no longer suspended when you leave the app.
- **Background networking** — a foreground service in the same `:wine` process keeps the game
  connected when you switch apps or open BannerHub's settings/home page.

> This release does **not** contain World of Warcraft. You must use your own game files.

## Download

Grab the APK from the **Releases** page:

- https://github.com/codxbre/wow-mobile-bannerhub/releases/latest

File: `BannerHub-v3.8.1-Normal-BG2.apk`
SHA-256: `3a2d1f40c64d0b0bf1ab7d1ad6944577d04510cfb57ba0eef482ebd2d23d98cf`

## Install

1. Install the APK. It is signed with the same BannerHub testkey, so it can update over
   BannerHub **v3.8.1 Normal**. If you have a different variant/signature, uninstall first.
2. Open BannerHub and allow notifications when asked (the persistent "Game running"
   notification is what keeps the session alive in the background).
3. In your Android battery settings, set BannerHub to **No restrictions** and allow
   **background data**.

## Add your game

1. Put your WoW folder on the phone at:
   `/storage/emulated/0/Download/World of Warcraft 3.3.5a/`
2. In BannerHub, add the game and choose `Wow.exe` (or `wow-f7f8.exe` if you use the
   optional F7/F8 helper).
3. Launch.

## Fix: game won't start

Open the game's **PC Game Settings** and set **GPU Driver → System Driver**, then relaunch.
If it still fails, update the firmware/container from the app's component manager.

## Fix: sound but black screen

Edit `World of Warcraft 3.3.5a/WTF/Config.wtf` and make sure these lines are present:

```
SET gxWindow "1"
SET gxMaximize "1"
SET gxResolution "1280x720"
```

The container must be able to create the requested display mode. Windowed at the
container's real resolution is the reliable choice.

## Background play

While a game is running you should see a **"Game running"** notification. That is the
keep-alive service. Leaving it running is what keeps WoW connected when you switch to
another app or open BannerHub's own menus.

## If nothing works: Termux + opencode

If you've tried the fixes above and the game still won't run, you can let an AI agent
diagnose it on the phone.

1. Install **Termux** from [F-Droid](https://f-droid.org/packages/com.termux/) or the
   [GitHub releases](https://github.com/termux/termux-app/releases) — **not** the Play Store
   version (it is outdated).
2. Open Termux and run:

   ```sh
   pkg update -y && pkg upgrade -y
   pkg install -y curl git
   curl -fsSL https://opencode.ai/install | bash
   opencode
   ```

3. In opencode, run `/connect` and pick a provider with a free tier (for example
   **Google** with a free API key from Google AI Studio, or **OpenRouter** free models).
   Paste the key, then run `/models` and choose a model marked **free**.
4. Paste this prompt (fill in the brackets):

   ```text
   I'm on Android (Termux) trying to run World of Warcraft 3.3.5a (Warmane) through
   BannerHub v3.8.1 Normal (modified APK from
   https://github.com/codxbre/wow-mobile-bannerhub).

   My device: [phone model, Android version, GPU].
   Problem: [game won't start / black screen with sound / disconnects in background / other].
   What I already tried: [GPU Driver = System Driver, Config.wtf windowed 1280x720,
   installed the background APK, battery = No restrictions].

   Please:
   1. Find and read my logs (logcat, wine.log, f7f8-debug.log) under
      /storage/emulated/0 and /data/data/com.termux/files/home.
   2. Diagnose the actual cause.
   3. Fix it step by step. If the APK needs patching, build a new one in the cloud
      (e.g. GitHub Actions) and put it in /storage/emulated/0/Download.
   4. If you need more information, ask me.
   ```

5. Follow its steps and try the game again.

## Credits

- **BannerHub** — [The412Banner](https://github.com/The412Banner/bannerhub)
- **GameHub Lite / GameHub** — Python / [Producdevity](https://github.com/Producdevity/gamehub-lite)
- **Warmane** — the WoW 3.3.5a server this was tested with
- This is an **unofficial, unsigned-for-Play-Store** community build. All original code and
  assets belong to their respective authors. No game files are distributed here.
