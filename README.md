# The Fit List — setup guide (no coding needed)

Everything is done in your web browser by clicking around and copy-pasting.
Total time: about 10 minutes, once. After that the app just works on both phones.

**The big picture:** the app is one file (`index.html`). To make it work on two
phones, two free things need to exist:

- **A database** — an online storage service where the photos and ratings are
  saved, so both phones read and write the same data. We use Google's **Firebase
  Realtime Database** (free tier), which also pushes changes to connected phones
  instantly — that's what makes the leaderboard update live.
- **Hosting** — a service that serves the app file at a public URL so both phones
  can open it like a normal website. We use **GitHub Pages** (free).

---

## Part 1 — Create the shared storage (on your computer, ~5 min)

1. Open **console.firebase.google.com** in your browser and sign in with your
   Google account.
2. Click **"Create a project"** (or "Add project"). Give it any name, like
   `fit-list`. If it asks about "Google Analytics", turn it **off**. Click through
   until the project is created.
3. In the menu on the left, click **Build**, then **Realtime Database**.
4. Click **"Create database"**. Pick any location. When it asks about security
   rules, choose **"locked mode"**. Click Enable.
5. You'll now see the database page. Near the top there are tabs: *Data, Rules,
   Backups…* Click **Rules**. Delete everything in the text box and paste exactly
   this instead:

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "rooms": {
      "$room": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

   Then click **Publish**. (These are the database's security rules — they block
   all access except to someone who knows your room's random ID, which only
   exists inside your invite link.)

6. One last thing to copy. Click the **gear icon ⚙** next to "Project Overview"
   (top-left) → **Project settings**. Scroll down to a section called
   **"Your apps"** and click the icon that looks like **`</>`**. Give the app any
   nickname, skip everything else, and click Register.
7. Firebase now shows you a block of text that starts with
   `const firebaseConfig = {` — this is the **config**: the settings (database
   URL, API key) the app needs to connect to *your* Firebase project.
   **Select and copy the whole thing.** Keep it somewhere handy (paste it into
   Notepad for now). You'll paste it into the app in Part 3.

---

## Part 2 — Put the app on the internet (~3 min)

1. Go to **github.com** and create a free account (or sign in).
2. Click the **+** at the top right → **"New repository"** (a repository is just
   a folder that lives online).
3. Name it `fit-list`, leave it on **Public**, and click **Create repository**.
4. On the next page, click the small link that says **"uploading an existing
   file"**. Drag the file `index.html` (from the `fit-list-sync` folder on this
   computer) into the box, then click **Commit changes** (that just means "save").
5. Now click **Settings** (top of the page) → **Pages** (left menu). Under
   "Branch", choose **main** and click **Save**.
6. Wait a minute or two, then refresh that page — it will show your app's web
   address, something like `https://yourname.github.io/fit-list/`.
   That's the link both of you will use. 🎉

Don't worry that the folder is "Public" — the file has no secrets in it. Your
photos and your database key never go into it.

---

## Part 3 — Pair the two phones (~2 min)

1. On **your** phone, open the app's URL from Part 2. You'll see a one-time
   setup screen. Paste the `firebaseConfig` block you saved in Part 1 into the
   box and tap **"Create our room 💘"**.
2. Go to the **Closet** tab and tap **"💌 Invite her phone"**. Send her the link
   it gives you — privately, like on WhatsApp. The link carries your config and
   room ID, so it works like a shared password: don't post it anywhere public.
3. She opens the link once on her phone. That's it — she's in, no setup at all.
4. On both phones: use the browser menu → **"Add to Home Screen"** so it feels
   like a real app.

Now she uploads outfits from the Closet tab, you swipe and judge in the Rate tab,
and the Leaderboard updates live on both phones.

---

## If something goes wrong

- **The app says "offline" forever** → in Firebase, check you clicked **Publish**
  after pasting the rules in Part 1, step 5.
- **The setup screen complains about your pasted text** → go back to Firebase
  Project settings and copy the *entire* `firebaseConfig` block again, from
  `const` to the final `};`.
- **Stuck anywhere** → come back to Claude and describe (or screenshot) what you
  see. If you paste your `firebaseConfig` block into the chat, Claude can build
  it into the file for you so you can skip the setup screen entirely.
