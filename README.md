# 🐉 Dragon Nest Merge

A casual HTML5 merge-and-evolve game built for **YouTube Playables**. Merge matching pets on a 5×6 board to evolve them through 12 levels, from an egg to the legendary final creature, then unlock the next kingdom and start again.

> Single-file game: plain HTML, CSS and JavaScript. No frameworks, no build step, no external assets.

---

## ✨ Features

- **7 kingdoms**, each with 12 evolution levels: Birds 🐦, Amphibians 🐸, Fishes 🐠, Animals 🦁, Humans 👑, Bugs 🐞 and Plants 🌳
- **5×6 merge board**: tap a pet, then tap an identical pet to merge them into the next level
- **Kingdom progression**: reach Level 12 to complete a kingdom and unlock the next one, with animated transitions
- **Energy system**: every spawn costs 1⚡. Energy recharges 1⚡ every 5 seconds, and the maximum grows by 1 for each level you reach in the current kingdom
- **Economy**: earn coins by merging and selling pets, and spend them on Nest upgrades that spawn higher-level pets
- **Daily rewards**: 7-day streak with a triple reward on day 7
- **Guided tutorial**: highlights and explains every button for first-time players (replay it from Settings → How to play)
- **Player profile**: first launch asks for a player name, and the game adds a random tag to keep it unique
- **Leaderboard**: shows your kingdom, score and progress, with fixed fictional rival characters to compete against. Tap any row for details
- **Music and sound effects**: generated in code with the Web Audio API (no audio files), with on/off toggles
- **Autosave**: saves every 60 seconds and after important actions, so players resume exactly where they left off
- **Ads (YouTube Playables SDK)**: optional rewarded ads and interstitial ads at level-ups (see below)

## 🎮 How to play

1. Press **Spawn** (costs 1⚡) to place a new pet on the board.
2. Tap a pet, then tap another pet of the **same level** to merge them.
3. Tap an empty cell to move a selected pet. Use **Sell** to turn a selected pet into coins and free up space.
4. Spend coins on **Nest** upgrades so spawns start at a higher level.
5. Reach **Level 12** to complete the kingdom and unlock the next one.

## 💰 Ads

The game uses the YouTube Playables SDK ads API (public preview, so check the official docs for changes).

| Where | Type | Reward ID |
|---|---|---|
| 📺 +6⚡ button and the out-of-energy popup | Rewarded | `energy-6` |
| Double reward on a new level | Rewarded | `level-2x` |
| Claim 2x on the daily gift | Rewarded | `daily-2x` |
| New level 4 to 12 reached for the first time | Interstitial | none |

Ad calls are wrapped in `try/catch`, only one ad request runs at a time (`adBusy`), and rewards are given only when the SDK reports the reward as earned.

### Test mode

Near the top of the script:

```js
const TEST=false; // true ONLY to test fake ads in a normal browser. MUST be false in the file you upload.
```

- `TEST=true`: outside YouTube, ad buttons play a 2-second fake ad and give the reward. Use this for local testing and for a public demo link.
- `TEST=false`: outside YouTube, no ads are shown and no rewards are given. **Use this for the build you submit to YouTube Playables.**

## 🚀 Run locally

No installation needed.

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
# open index.html in your browser, or serve it:
python3 -m http.server 8000
# then visit http://localhost:8000
```

## 🌐 Host a demo on GitHub Pages

1. Set `TEST=true` in a demo copy of `index.html` so reviewers can see the ad flow.
2. Push the repo to GitHub.
3. Go to **Settings → Pages**, choose your branch and the root folder, then save.
4. Your game will be live at `https://<your-username>.github.io/<your-repo>/`.

## 📦 Submit to YouTube Playables

1. Make sure `TEST=false` and `const API=""` (Playables does not allow external network calls).
2. Zip the game so `index.html` is at the **root** of the zip.
3. Apply through the YouTube Playables interest form (see the [official Playables documentation](https://developers.google.com/youtube/gaming/playables)).
4. After approval, run YouTube's test suite and fix anything it reports.
5. Submit the zip for certification.

## 🧩 Customization

Everything is in `index.html`.

| What | Where |
|---|---|
| Kingdoms, pet names and emojis | the `K` array |
| Energy recharge time | `REG` (milliseconds, currently `5000`) |
| Energy capacity | `cap()` (5 + best level in the current kingdom) |
| Nest upgrade prices | `NC` array |
| Sell price | the `sell` button handler |
| Fictional leaderboard rivals | the `RIV` array |
| Rewarded ad IDs | the `ad('...')` calls |
| Online leaderboard server | `const API=""` (see below) |

### Optional online leaderboard

Outside YouTube Playables, you can connect your own server by setting `const API="https://your-server"`. It needs two endpoints:

- `GET /board` returns a JSON list of `{name, k, sc, mx, done, un, seen}`
- `POST /score` receives the same object for the current player

Inside Playables, leave `API` empty, because external network calls are not allowed. The game still submits the score to YouTube through the SDK's `sendScore`.

## 💾 Save data

Progress is stored with the Playables SDK (`saveData` / `loadData`) and mirrored to `localStorage` (key `nest2`) outside YouTube. Saves from older versions are kept, and new kingdoms are added automatically.

## 🛠️ Tech

- HTML, CSS, vanilla JavaScript (single file)
- Web Audio API for music and sound effects
- [YouTube Playables SDK](https://developers.google.com/youtube/gaming/playables) for saving, audio and pause handling, and ads

## 📄 License

Add your preferred license here (for example, MIT). Pet emojis are standard Unicode emoji and display using each device's emoji font.

## 🙌 Credits

Made by **<your name>**. Feedback and ideas are welcome. Open an issue or a pull request.
