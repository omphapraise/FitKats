# Fit Kats Gym App

![CI](https://github.com/YOUR-USERNAME/FitKats/actions/workflows/ci.yml/badge.svg)

A premium gym membership mobile app prototype for **Fit Kats**, a tiered-membership gym concept in Sandton.
Built for **Module XISD6329 (Task 2)**.

The app is a **balanced MVP**. Core features work end to end (app, API and database), secondary features are
simplified, and advanced features are simulated, so the scope stays realistic.

## Screenshots

| Login | Home | Workouts |
|---|---|---|
| ![Login](docs/screenshots/login.png) | ![Home](docs/screenshots/home.png) | ![Workouts](docs/screenshots/workouts.png) |

| Gym | Account | Settings |
|---|---|---|
| ![Gym](docs/screenshots/gym.png) | ![Account](docs/screenshots/account.png) | ![Settings](docs/screenshots/settings.png) |

## Tech stack

| Layer | Technology |
|---|---|
| Mobile app | Kotlin, Android Studio, ViewBinding, Material 3, Retrofit, Coroutines |
| API | Node.js, Express, JWT login tokens, bcrypt password hashing |
| Database | SQLite (relational: primary keys, foreign keys, constraints) |
| Testing | 21 automated API tests, 15 Android unit tests, input validation in the app and API |
| DevOps | GitHub Actions (build, check, test, APK artifact) |

## Features

| # | Feature | Level |
|---|---|---|
| 1 | Registration and login | Full (API + database + UI) |
| 2 | Member dashboard | Full |
| 3 | Workout logging (sets, reps, weight) | Full |
| 4 | Progress tracking (history and stats) | Full |
| 5 | Membership information (tier, expiry) | Full |
| 6 | Settings (display name, notifications) | Full |
| 7 | Payment history | Full (seeded mock data) |
| 8 | Class booking | Simplified (list and book button) |
| 9 | Real-time capacity | Simplified (static Green / Orange / Red indicator) |
| 10 | Loyalty points | Simplified (+10 points for every logged workout) |
| 11 | Referral system | Simplified (input and confirmation) |
| 12 | Announcements feed | Simplified (static) |
| 13 | Equipment fault reporting | Simplified (form saved to the database) |
| 14 | Facial recognition check-in | Simulated |
| 15 | Biometric update | Simulated (UI only) |
| 16 | Tier perks dashboard | Simulated (static) |
| 17 | Off-peak incentives | Simulated |

## App navigation

The bottom bar has five tabs: **Home**, **Workouts**, **Gym**, **Account** and **Settings**.

## Project structure

```
FitKats/
├── android-app/          Kotlin Android app
│   └── app/src/
│       ├── main/         screens (ui), network (api), data classes (model), helpers (util), layouts (res)
│       └── test/         Android unit tests
├── backend/              Node.js + Express API and SQLite database
│   ├── src/
│   │   ├── routes/       auth, profile, workouts, payments, reports
│   │   ├── middleware/   login check (JWT)
│   │   ├── db.js         database connection and table creation
│   │   ├── seed.js       sample data (10+ records per table)
│   │   └── server.js     API entry point
│   ├── test-api.js       automated API tests
│   └── show-db.js        prints the database structure and sample rows
├── docs/                 user guide, DevOps notes, demo script
└── .github/workflows/    CI pipeline
```

## Database

| Table | Columns |
|---|---|
| Users | id (PK), email, password (hashed), display_name, membership_tier, membership_expiry, loyalty_points |
| Workouts | id (PK), user_id (FK), exercise_name, sets, reps, weight, date |
| Settings | user_id (PK, FK), notifications_enabled |
| Payments | id (PK), user_id (FK), amount, status, date |
| Reports | id (PK), user_id (FK), description, status |

Foreign keys link every table to Users. The tables use correct data types and check constraints (for example,
sets and reps must be above zero, and payment status must be Paid, Pending or Failed).

## API endpoints

| Method | Endpoint | Purpose | Login needed |
|---|---|---|---|
| POST | /register | Create an account | No |
| POST | /login | Log in and receive a token | No |
| GET | /profile | Profile, membership and settings | Yes |
| POST | /update-profile | Change display name or notifications | Yes |
| POST | /add-workout | Save a workout (+10 points) | Yes |
| GET | /workouts | Workout history | Yes |
| GET | /payments | Payment history | Yes |
| POST | /report-issue | Report broken equipment | Yes |

All replies are JSON with `success` and `message`. Errors return clear messages and the right status code.

## How to run it

### 1. Backend
```bash
cd backend
npm install
npm run seed     # creates the database with sample data
npm run dev      # starts the API on http://localhost:3000
```

### 2. Android app
1. Open the `android-app` folder in Android Studio.
2. Start an emulator.
3. Press Run. The emulator reaches the API at `http://10.0.2.2:3000/`.

For a real phone, change `BASE_URL` in `ApiClient.kt` to your computer's IP address.

### Demo account
Email: `demo@fitkats.com`
Password: `Password123`

Other sample accounts use the same password: `lerato@fitkats.com` (Platinum) and `zanele@fitkats.com` (expired membership).

## Testing

**API tests** (21 checks: login, registration, validation, profile, workouts, points, payments, reports, errors):
```bash
cd backend
npm run dev     # in one terminal
npm test        # in another terminal
```

**Android unit tests** (15 checks on the input rules and formatters): in Android Studio, right-click
`UtilTests` in the `test` folder and choose **Run 'UtilTests'**.

**Database viewer:**
```bash
cd backend
node show-db.js
```

Both sets of tests run automatically in GitHub Actions on every push.

## Simulated and simplified features

- Face check-in, face profile update, tier perks and off-peak points are simulated previews.
- Class bookings and referral counts are stored on the phone, not in the database.
- Loyalty points are real only for logged workouts (+10 each). Referral and off-peak points are simulated.
- The capacity indicator is static. Tapping the card previews the other levels.

## DevOps lifecycle

See [docs/DEVOPS.md](docs/DEVOPS.md). User instructions are in [docs/USER_GUIDE.md](docs/USER_GUIDE.md).

## Author

Your Name, Student number, Rosebank College
