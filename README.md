# Poppy Playtime Unity — Play in Browser Kit

This kit takes your **Poppy-Playtime-Unity** project (Unity 2022.3.11f1) and turns it into a game that **runs in any desktop browser**, using GitHub's free servers to do the heavy compiling. No game engine, no paid software, nothing to install except accounts.

```
 Your 10 zips ──► GitHub Release ──► GitHub Actions (free build server)
                                         │  compiles Unity → WebGL
                                         ▼
                       https://YOURNAME.github.io/Poppy-Playtime-Unity
                              ◄── playable in any browser
```

**What's in this folder:**

| File | What it is |
|---|---|
| `README.md` | This guide |
| `.github/workflows/…` | Two recipes GitHub's build robots follow |
| `ci-kit/…` | A build script + the dark loading screen template that get injected into the game at build time |
| *(separate download)* `Poppy-Playtime-Unity-merged.zip` | Your 10 parts merged into one zip — ready to upload in Step 4 |

**Total cost: $0.** Public repos get free Actions minutes and free GitHub Pages hosting. The build takes roughly **30–60 minutes** (one-time; re-builds are faster thanks to caching).

---

## Step 1 — Create a GitHub account (2 min)

1. Go to **https://github.com/signup**
2. Enter an email, create a password, pick a username.
   → Whatever username you pick decides your game URL later: `https://<username>.github.io/...`
3. Verify your email.

## Step 2 — Create a Unity account (2 min)

1. Go to **https://id.unity.com/register**
2. Sign up (email or Google). Pick the **Personal** plan when asked — it is free and covers this project.
3. Verify your email. You will *not* install Unity on your PC; the account is only needed to generate a free build license in Step 5.

## Step 3 — Create the repo and upload this kit (5 min)

1. Go to **https://github.com/new**
2. Repository name: `Poppy-Playtime-Unity`
3. Visibility: **Public** ← required for free Pages hosting
4. Do **NOT** tick "Add a README" — leave everything empty, click **Create repository**.
5. On the empty repo page click **"uploading an existing file"**.
6. Drag the whole **`repo-upload-me`** folder's contents into the upload area:
   - Windows: the `.github` folder is visible normally.
   - macOS: press **Cmd + Shift + .** in the file picker to reveal hidden folders.
   - *If dragging fails*, alternative: in the repo click **Add file → Upload files** and drag the `.github` folder and the `ci-kit` folder.
7. Click **Commit changes**.
8. Check the **Actions** tab — you should see two workflows listed ("1 - Get Unity activation file" and "2 - Build WebGL…"). If you see them, Step 3 is done.

## Step 4 — Upload your game zips to a Release (5 min)

GitHub blocks uploads bigger than 25 MB through the normal file page — but **Releases** allow 2 GB per file in the browser. That's why the game travels as a Release.

1. On your repo page, on the right side click **Releases** → **Create a new release** (or "Draft a new release").
2. Tag: type `game-v1` and choose "Create new tag".
3. Release title: anything, e.g. `Game files`.
4. Drag in **`Poppy-Playtime-Unity-merged.zip`** (445 MB, the one you downloaded) — or drag all ten `poppy_part1..10.zip` files, both work.
5. Click **Publish release**.
   → The build workflow starts **automatically**, but it will pause with an error until the license secret exists (next step). That is expected.

## Step 5 — Generate the free Unity license secret (10 min, once)

Goal: put the text of a free **Personal license** (a `.ulf` file) into a GitHub secret called `UNITY_LICENSE`. Two routes — pick one:

### Route A — No install (recommended, ~10 min)

1. In your repo, open the **Actions** tab → left sidebar → **"1 - Get Unity activation file"** → button **Run workflow** → **Run workflow**.
2. Wait ~2 minutes, refresh. Click the finished run → under **Artifacts** download **`Unity_Activation_File`** → unzip it. You get a file like `Unity_v2022.3.11f1.alf`.
3. Go to **https://license.unity3d.com/manual** (Unity's official manual-activation page).
4. Log in with your Unity account → it asks "Upload activation file" → **Browse**, pick the `.alf` file → **Next**.
5. It asks what license you need → choose **Personal** → answer the questions (pick the free/revenue-under-$200k options) → **Next** → **Download license file**. You get a file like `Unity_v2022.x.ulf`.
6. Go to **Step 5.3 "Create the secrets"** below.

### Route B — With Unity Hub (officially documented, no editor install)

1. Install **Unity Hub** only (https://unity.com/download) — you do **not** install any editor version.
2. Open Hub → log in → gear icon ⚙ **Preferences → Licenses → Add → Get a free personal license**.
3. Now find the license file on disk:
   - Windows: `C:\ProgramData\Unity\Unity_lic.ulf` (ProgramData is hidden — enable "Hidden items" in Explorer's View menu)
   - macOS: `/Library/Application Support/Unity/Unity_lic.ulf`
   - Linux: `~/.local/share/unity3d/Unity/Unity_lic.ulf`

### 5.3 Create the secrets (both routes)

1. Open the `.ulf` file with Notepad (Windows) or TextEdit (Mac — Format > Make Plain Text if needed). **Select ALL text and copy it.**
2. Back on GitHub: repo → **Settings** → left sidebar **Secrets and variables** → **Actions** → button **New repository secret** — create these (Name must match exactly):
   - Name: `UNITY_LICENSE` → Secret: paste the whole `.ulf` text (keep the `<?xml ...>` and `</license>` lines!)
   - Name: `UNITY_EMAIL` → Secret: your Unity account email
   - Name: `UNITY_PASSWORD` → Secret: your Unity account password
   
   *(GitHub encrypts secrets — the email/password are only used by the build robot to activate Unity. GameCI explicitly does not store them.)*

## Step 6 — Build and go live (30–60 min)

1. Repo → **Actions** tab → **"2 - Build WebGL & deploy to GitHub Pages"** → **Run workflow** → **Run workflow**.
2. Grab a coffee ☕ — the robot downloads your zips, imports 430 MB of assets and compiles the game.
3. When the run turns **green ✓**, the game is live at:

   **`https://YOUR-USERNAME.github.io/Poppy-Playtime-Unity/`**

   (The exact link is also shown in the run summary → "Deploy to GitHub Pages" → `page_url`, and under your repo's **Deployments**.)

## Step 7 — Play 🎮

- **First load** downloads the whole game (several hundred MB) — the dark loading screen with the red bar is working as intended. Afterwards the browser caches it, so next launches are much faster.
- Click the game window once so keyboard/mouse are captured. **Fullscreen** button is bottom-right.
- Needs a desktop browser with WebGL2 (Chrome / Edge / Firefox).

---

## Can I play it inside this GLM chat?

Yes — after Step 6 finishes, come back to the chat, download the build from your repo (Actions → the finished run → **Artifacts** → `webgl-build`), upload the zip here, and the game can be served directly on the chat's preview link. The GitHub Pages link is the permanent home, though — it works even when this chat is closed.

## Updating the game later

Changed something in the project? Just re-upload a new zip:

1. Releases → **Draft a new release** → tag `game-v2` → drag the new zip → Publish.
2. The build runs automatically and your Pages link updates. Done.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Build fails: *"UNITY_LICENSE secret is not set"* | Step 5 wasn't done, or the secret name isn't exactly `UNITY_LICENSE` |
| Build fails at *"No .zip files found in any release"* | Step 4 wasn't done, or the release wasn't **Published** (drafts don't count) |
| Build fails: *"No valid Unity license"* / *"entitlement"* | The `.ulf` paste was incomplete — redo Step 5.3, copy **everything** including the `<?xml` and `</license>` lines |
| Build fails in Unity compile errors | Open the failed run, scroll the "Build WebGL with Unity" log — the first red block names the exact asset/problem |
| Pages shows 404 | Wait 2–3 min after the green ✓, hard-refresh (Ctrl+Shift+R); check Settings → Pages → Source is "GitHub Actions" |
| Loading bar stuck at 0% | Slow network with a big first download — give it a minute; check the browser console (F12) |
| Black screen after load | Enable hardware acceleration in browser settings; update your GPU driver |
| Actions minutes worry | Public repos = unlimited free minutes for standard runners. Nothing to pay. |

## How this kit works (for the curious)

- `ci-kit/Assets/Editor/PPUWebGLBuild.cs` — tells Unity, headlessly, to build all scenes for WebGL with **Gzip** compression and the custom template.
- `ci-kit/Assets/WebGLTemplates/PoppyTheme/` — the dark loading screen you saw described (red accent, progress phases, fullscreen button, desktop-only notice) plus a **fetch patch** that decompresses `.gz` files in-browser — that patch is what makes the game work on GitHub Pages, which can't send compressed-content headers itself.
- `.github/workflows/2-build-webgl.yml` — downloads your release zips, extracts the Unity project, injects `ci-kit/`, compiles with Unity 2022.3.11f1 via [game-ci](https://game.ci), publishes to Pages.
- The Unity project itself is [tayoky's Poppy-Playtime-Unity](https://github.com/tayoky/Poppy-Playtime-Unity) (MIT). Poppy Playtime is a trademark of Mob Entertainment — this is an unofficial fan project.
