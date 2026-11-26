# Ringer Fantasy Games: setup guide

A fantasy archery league for the 2026 Texas Ringer. The site is hosted free on **GitHub Pages**, and teams and scores are saved in **Firebase** (Google's free database). Setup takes about 30–45 minutes, and everything stays on the free tiers.

You'll need: a GitHub account, a Google account, and these four files: `index.html`, `config.js`, `firestore.rules`, `README.md`.

---

## Part A: Put the site on GitHub

**1. Create the repository**
1. Sign in at github.com and click **+ → New repository** (top right).
2. Name it `ringer-fantasy` and set it to **Public**. GitHub Pages is free for public repositories.
3. Click **Create repository**.
4. On the next page, click **uploading an existing file**. Drag in `index.html`, `config.js`, `firestore.rules` and `README.md`, then click **Commit changes**.

**2. Turn on GitHub Pages**
1. In the repository, go to **Settings → Pages**.
2. Under *Build and deployment*, set **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)**, then click **Save**.
3. Wait 1–2 minutes and refresh. Your address appears at the top, for example `https://YOUR-USERNAME.github.io/ringer-fantasy/`.

Open that address. The site works now, but only in **practice mode** (picks are saved in that browser only) until you finish Part B.

---

## Part B: Connect Firebase so everyone shares one league

**3. Create the Firebase project**
1. Go to console.firebase.google.com and click **Create a project**. Name it something like `ringer-fantasy`. You can turn Google Analytics off.
2. On the project home page, click the **web icon `</>`** ("Add app"). Give it any nickname and click **Register app**. You don't need Firebase Hosting.
3. Firebase shows a block of code containing `const firebaseConfig = { ... }`. Keep that page open, because you'll copy four values from it in step 6.

**4. Turn on sign-in**
1. In the left menu, go to **Build → Authentication → Get started**.
2. On the **Sign-in method** tab, enable **Google**: pick a support email and save. Then enable **Email/Password** (just the first switch) and save.
3. Go to **Settings → Authorized domains → Add domain** and enter `YOUR-USERNAME.github.io` (with no `https://` and nothing after `.io`).

**5. Create the database and add the security rules**
1. Go to **Build → Firestore Database → Create database**. Choose a location near you (for example `us-central1`), choose **Start in production mode**, and click **Create**.
2. Open the **Rules** tab, delete everything there, and paste in the full contents of `firestore.rules`.
3. In what you pasted, replace `you@example.com` with **your** Google email (the commissioner's). Then click **Publish**.

**6. Put your settings in `config.js`**
1. In GitHub, click `config.js`, then the **pencil icon** to edit it.
2. Fill in your values from Firebase (step 3) and your commissioner email:
   ```js
   firebaseConfig: {
     apiKey: "AIza...",
     authDomain: "ringer-fantasy.firebaseapp.com",
     projectId: "ringer-fantasy",
     appId: "1:1234:web:abcd"
   },
   commissionerEmails: ["yourname@gmail.com"]   // same email as in the rules
   ```
3. Click **Commit changes**. GitHub Pages updates within a minute or two.

Your settings live only in `config.js`. To update the site later, upload a new `index.html`; `config.js` stays as it is.

It's normal for these Firebase values to be visible to anyone. They only identify your project. What protects the data is the security rules from step 5.

---

## Part C: Test it, then invite people

1. Open your site and click **Continue with Google** using the commissioner email. A **Commissioner** tab should appear.
2. Build a test team, then open the site on your phone. Your team and the standings should match.
3. Text the link to your friends. They can sign in with Google or with an email and password.

**Running the league**
- **Lock time:** rosters lock automatically on **Fri Dec 4 at 2:00 PM Central**, and the database itself blocks changes after that. On the Commissioner tab you can choose **Locked now** or **Keep open** instead.
- **Automatic results (best):** on the **Commissioner** tab, set **Results page** to `https://texasringer.eyesonscore.com/events/2026/` (it's pre-filled) and click **Save link**. Then click **Pull results now**, or tick **Auto-pull every 5 minutes** and leave that tab open during the tournament. It brings in qualification ranks and scores, the official bracket seeds, and every match winner, shoot-offs included. Until the tournament starts, that page says "Scores coming soon", so a pull will report that nothing is posted yet.
- **Copy and paste (backup):** use this if the automatic page isn't updating during the event. on the **Commissioner** tab, paste the results link into **Results link** and click **Save link**. Everyone then sees an "Official live scores" link on the Results tab. To pull in scores, open that link on a computer, press **Ctrl+A** then **Ctrl+C** (Cmd on a Mac), paste into the import box, and click **Import results**. For eliminations, click the round's tab (1/8th, Quarter, Semi, Finals) in each division on Eyes on Score first, then copy and import again. Imports only add and update, so re-import as often as you like. Tied matches (shoot-offs) are flagged so you can click the winner yourself.
- **Test mode:** click **Switch to 2025 test data** to try everything with last year's real archers. Then either import from the 2025 link or click **Load all 2025 results**. Click **Back to the 2026 field** when you're done, and have players re-check their teams before the lock.
- **Qualification by hand:** on the **Commissioner** tab, pick a division, type each archer's score and X count (Xs break ties), then click **Save**. The **Results** tab shows the live rankings and the top-16 cut line.
- **Eliminations:** when qualification is final, click **Start bracket**. It seeds the top 16 (1 vs 16, 8 vs 9, …) and gives byes to the top seeds in divisions with fewer than 16 archers. Then click the winner of each match as it finishes. Click a winner again to undo.
- **Scoring:** qualification rank (1st = 16 points down to 16th = 1), +5 for making the top 16, then +10 for a Round of 16 win, +10 for a quarterfinal, +15 for a semifinal, +20 for gold and +10 for bronze. To change these numbers, edit the **Scoring** block near the top of `index.html`.
- **Signing in as commissioner:** use **Google**, because commissioner powers require a verified email and Google accounts already are.

## Fun extras

- **Share my team:** on My Team (and on your own row in Standings), players tap **Share my team** to get a 1080×1350 image of their team, sized for Instagram and Facebook. On a phone, Share sends it straight to apps; on a computer, it downloads.
- **Trash-talk wall:** beside Standings. Signed-in players can post (280 characters, one post every 15 seconds) and react with Bullseye or Ouch. Players can delete their own posts, and the commissioner can delete any post. **This needs the updated `firestore.rules`**: paste the whole file into Firebase → Firestore → Rules again (with your email in it) and click **Publish**.
- **Big Board (TV mode):** on the Commissioner tab, click **Open the Big Board**, or go to `your-site/#bigboard`. Put it on a TV at the venue and click **Full screen**. It shows the top 10 teams with up/down moves, the matches being shot, the hottest archers, a QR code to join, and a scrolling trash-talk ticker. It refreshes every minute.
- **Awards:** an **Awards** tab appears for everyone once the last medal match is posted. You see it earlier as a preview. It names the top 3 teams plus Sharpshooter (best captain), Sleeper Pick (picked archer who beat their projection by the most), Crystal Ball (team closest to its projection) and Underdog (lowest-projected team to finish top 3). Ties show every winner. **Share the awards** makes an image to post.

## Troubleshooting

| What you see | Fix |
|---|---|
| "Setup isn't finished… practice mode" | `config.js` is missing or still says `PASTE…`. Redo step 6. |
| "This web address isn't on Firebase's authorized domains list" | Step 4.3: add `YOUR-USERNAME.github.io`. |
| "That sign-in method isn't turned on" | Step 4.2: enable Google and Email/Password. |
| A team won't save ("Couldn't save… security rules") | Step 5: the rules weren't published, or the lineups are locked. |
| Posting on the wall fails ("update the Firestore rules") | Paste the new `firestore.rules` into Firebase and click Publish (step 5). |
| No Commissioner tab | The email must match in **both** `config.js` and the rules, and you must sign in with Google. |
| Changes on GitHub don't show up | Wait 2 minutes, then do a hard refresh (Ctrl+Shift+R, or Cmd+Shift+R on a Mac). |

Fan-made game. Not affiliated with the Texas Ringer or Eyes on Score.
