# Thomson Car Rental Booking Service: setup guide

This takes about 30 minutes, once. Everything stays on free plans: GitHub Pages hosts the app, and Firebase (Spark plan, no card) stores the data and handles logins.

## Files in this package

| File | What it is |
|---|---|
| `index.html` | The whole app |
| `manifest.json`, `icon-192.png`, `icon-512.png` | Lets phones install it on the home screen |
| `firestore.rules` | Security rules that enforce admin, sales, and driver access |
| `SETUP.md` | This guide |

## Step 1: Create the Firebase project

1. Go to https://console.firebase.google.com and sign in with a Google account the company controls.
2. Click **Create a project**, name it `thomson-booking`, and turn Google Analytics **off**.
3. The project starts on the free **Spark** plan. Never click "Upgrade". On Spark, if a limit is ever reached, the service pauses until the next day. It never charges anything.

## Step 2: Turn on logins

1. In the left menu, open **Build → Authentication → Get started**.
2. Under **Sign-in method**, choose **Email/Password**, switch it on, and save.

Staff never see or use an email. The app turns a username such as `ahmed.driver` into an internal login automatically.

## Step 3: Create the database

1. Open **Build → Firestore Database → Create database**.
2. Choose a location close to Dubai if one is listed, for example a Middle East region. Otherwise choose a Europe region. The location can't be changed later.
3. Choose **Start in production mode**.
4. Open the **Rules** tab, delete everything there, paste the full contents of `firestore.rules`, and click **Publish**.

## Step 4: Connect the app to Firebase

1. Click the gear icon next to **Project Overview → Project settings**.
2. Under **Your apps**, click the web icon `</>`, name it `booking-web`, leave "Firebase Hosting" unticked, and click **Register app**.
3. Open `config.js` in a text editor and copy each value from Firebase's `firebaseConfig` block between the matching quotes.

`config.js` is the only file that holds your keys. When you receive an updated `index.html`, you replace `index.html` only and never touch `config.js` again. These values aren't passwords; the security rules from Step 3 protect the data.

## Step 5: Publish on GitHub Pages

1. On https://github.com, click **New repository**. Name it `thomson-booking` and choose **Public**. Free GitHub Pages needs a public repository; your data is not in the repository, only the app code.
2. Click **uploading an existing file** and upload `index.html`, `config.js`, `manifest.json`, `icon-192.png`, and `icon-512.png`. Click **Commit changes**.
3. Open **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After a minute or two, the page shows your link, for example `https://yourname.github.io/thomson-booking/`.
5. Back in Firebase, open **Authentication → Settings → Authorized domains → Add domain** and add `yourname.github.io`.

## Adding many cars at once

In **More → Cars and plates → Add many cars**, paste one car per line: Model, Plate, Color, Category. You can copy the four columns straight from Excel. Duplicate plates are skipped automatically.

## Step 6: First sign-in

1. Open your link. Because no admin exists yet, the app shows **First-time setup**.
2. Create the main admin account (your name, a username such as `sajid.admin`, and a strong password). This screen never appears again.
3. Open **More → Company and VAT settings** and fill in the company name, address, phone, TRN, VAT 5%, CC options `4,5`, default CC, and a delete PIN.
4. Open **More → Cars and plates** and add every car with its plate number.
5. Open **More → Users and roles** and create a login for each person:
   - **Driver**: sees tasks, marks deliveries and pickups done, records KM, fuel, and damage. Never sees prices.
   - **Sales**: creates and edits bookings, sees prices, settles returns. Keep "Also add as salesperson" selected so they get their reminders.
   - **Admin**: everything, including users, settings, and deleting bookings.
6. Open **More → Salespeople** to add any salesperson who doesn't have a login. They can be chosen on bookings but won't get reminders.

## Step 7: Install on phones

- **iPhone**: open the link in Safari, tap **Share → Add to Home Screen**.
- **Android**: open the link in Chrome, tap the menu **⋮ → Install app** or **Add to Home screen**.

It then opens full screen like a normal app.

## Adding your logo later

1. Upload your logo to the same GitHub repository and name it `logo.png`. A PNG with a transparent background works best.
2. In `config.js`, change `window.LOGO_URL = '';` to `window.LOGO_URL = 'logo.png';` and commit.

The logo then appears on the welcome page, the sign-in page, and every PDF.

## Updating the app

Upload the new `index.html` to the repository (drag it onto the repository page and commit). Phones pick up the update the next time the app is fully closed and reopened. GitHub Pages has no deploy credits or limits you need to watch for this.

## How reminders work

- While the app is open, a task due within 30 minutes pulses amber and plays an alert once for the assigned driver and the salesperson. Unassigned tasks alert the admin. Late tasks turn red.
- When the app is closed or the screen is off, nothing can alert. For those cases, use **Add to calendar** on a task. It creates a phone calendar event with its own 30-minute alert. If the task is rescheduled, tap it again to add the new time.
- Phones only allow sound after the screen has been tapped once since the app was opened.

## Everyday tips

- **Forgotten password**: an admin deactivates the old login under Users and creates a new one. Anyone can change their own password under **More → My account**.
- **PDFs**: every PDF button opens the phone's print screen. Choose **Save as PDF** (Android) or pinch out on the preview and tap Share (iPhone) to save or send it.
- **Times**: all dates and times are entered and shown as Dubai local time.
- **Free limits**: the Spark plan allows 50,000 reads and 20,000 writes per day, far above what a rental team uses. If it were ever reached, the app would pause until the next day. It never costs money.

## Troubleshooting

| What you see | What to do |
|---|---|
| "Connect Firebase first" | `config.js` is missing from the repository or still has `PASTE_` values. Complete Step 4 and upload it. |
| "api-key-not-valid" | The apiKey in `config.js` doesn't match Firebase. Copy it again from Project settings → Your apps → Config. |
| "You don't have permission for this" | The rules weren't published. Repeat Step 3.4. |
| "Account not active" | An admin needs to activate that user under **More → Users and roles**. |
| Changes don't show on a phone | Fully close the app and reopen it. |
