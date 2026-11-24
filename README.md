# TableFlow

Multi-restaurant QR ordering. An owner registers their restaurant, creates logins for waiters and kitchen staff, and prints table QR codes. Customers scan, order, track, tip and pay. Every screen updates live.

## How it works

| Page | Who | Access |
|---|---|---|
| `manager.html` | Owner / manager | Register a restaurant (name, mobile, Gmail, password) or sign in. Tables, QR codes, orders, menu, staff logins, cashier report, settings |
| `waiter.html` | Waiter (or manager) | Staff login. Accept orders with an ETA, serve, confirm payments |
| `kitchen.html` | Kitchen (or manager) | Staff login. Start preparing, delayed, ready |
| `customer.html` | Customer | No login. Opened from the table QR (`customer.html?r=RESTAURANT_ID&table=N`) |
| `index.html` | Launcher | Links for owners and staff |

Flow: QR scan (table occupied, waiter alerted) → order → waiter accepts + ETA → kitchen → ready → waiter serves → bill with tip on customer's phone → pay → waiter confirms → table cleared.

The restaurant's name replaces "TableFlow" in the header of every screen and on the QR cards.

## Setup (Firebase, free Spark plan)

1. https://console.firebase.google.com → **Add project**.
2. **Build → Authentication → Get started → Sign-in method**: enable **Email/Password** and **Anonymous**.
3. **Build → Realtime Database → Create database** (any region, locked mode is fine).
4. Open the **Rules** tab, paste the contents of `database.rules.json`, **Publish**.
5. **Project settings → Your apps → Web (`</>`)**: register an app, copy the config values into `config.js`, commit.
6. Host the repo on GitHub Pages (Settings → Pages → `main` / root). If Firebase reports an unauthorized domain, add `YOUR-USERNAME.github.io` under Authentication → Settings → Authorized domains.
7. Open `manager.html`, choose **Register a new restaurant**, then create staff logins on the **Staff** page and print QR codes from **QR Codes**.

## Security notes

- Passwords are handled by Firebase Authentication (hashed and managed by Google). They are never stored in this repo or in plain text.
- The mobile number is stored but **not verified** (SMS verification needs a paid plan).
- Staff roles are enforced in the app and by the `users` database rules. Order data is shared by one database node per restaurant, so a technically skilled customer could tamper with their restaurant's data. For a production launch, split the data and move order writes into Cloud Functions.
- Payments are recorded manually by the waiter. UPI opens a pay link and nothing is verified.
- GST and service charge are configurable inputs, not tax advice.

## License

MIT
