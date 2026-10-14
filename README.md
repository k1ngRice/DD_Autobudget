# DD_Autobudget

A lightweight personal budgeting app in a single `index.html` file. Track income and expenses across checking and savings accounts, and sync everything across devices through your own Firebase project.

## Features

- **Two accounts**: checking and savings, each with its own starting balance and running total.
- **Transactions**: log income or expenses with an amount, category, date, optional description, and payment method (debit or credit).
- **Categories**: Food, Housing, Transport, Utilities, Shopping, Salary, Savings deposit, Volleyball, YuGiOh, and Other.
- **Monthly breakdown**: spending grouped by month and category, shown as bar charts with each category's share of the total.
- **Gain/loss summary**: income compared with expenses at a glance.
- **Undo/redo**: reverse or reapply adds and deletes.
- **Filtering and pagination**: switch between All, Income, and Expenses, 10 transactions per page.
- **Responsive layout**: a desktop view, plus a mobile view with a bottom nav bar and a floating add button.
- **Cloud sync**: data syncs in real time through Firebase Realtime Database, and a status dot in the header shows the connection state.

## Getting started

There is no build step. Open `index.html` in a browser, or host it on any static host such as GitHub Pages.

### Using the existing project

To use the app's existing Firebase backend, enter these values on the setup screen:

| Field        | Value                                                  |
| ------------ | ------------------------------------------------------ |
| API Key      | `AIzaSyCOS8bpnHwn3HcznD1qinMb0--s17oE8Jc`              |
| Auth Domain  | `autobudget-1af58.firebaseapp.com`                     |
| Database URL | `https://autobudget-1af58-default-rtdb.firebaseio.com` |
| Project ID   | `autobudget-1af58`                                     |
| App ID       | `1:514206129408:web:8023273b1cb17e11cba7e5`            |

Then sign in with your email and password, or enter a new email and password to create an account. Each account only sees its own data.

To use your own backend instead, follow the steps below.

### 1. Create a Firebase project

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com).
2. Under **Build → Authentication**, turn on the **Email/Password** sign-in provider.
3. Under **Build → Realtime Database**, create a database.
4. Under **Project settings → Your apps**, add a web app and copy its config values.

### 2. Lock down the database

Each user's data is stored under `users/<uid>/`. These rules let a signed-in user read and write only their own data:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid"
      }
    }
  }
}
```

### 3. Connect the app

1. Open the app. The first time, it asks for your Firebase config: API key, auth domain, database URL, project ID, and app ID.
2. Click **Save & Connect**. The config is saved in your browser's `localStorage`, so you only enter it once per browser.
3. Sign in with an email and password. If no account exists for that email, one is created for you.
4. Set your starting checking and savings balances when prompted.

To change your Firebase project later, use **Update Firebase Config** (⚙ Config).

## Data model

```
users/
  <uid>/
    balances/      { checking, savings }
    transactions/
      <pushKey>/   { id, acct, type, name, amount, cat, date, pay }
```

| Field    | Values                                     |
| -------- | ------------------------------------------ |
| `acct`   | `checking` or `savings`                    |
| `type`   | `income` or `expense`                      |
| `pay`    | `debit` or `credit`                        |
| `id`     | Creation timestamp, used for sort order    |

## Tech stack

- Plain HTML, CSS, and JavaScript with no framework or build tools
- Firebase JS SDK 9.23 (compat): App, Auth, and Realtime Database
- DM Sans and DM Mono fonts from Google Fonts
