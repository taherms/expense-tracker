# Expense Tracker

A simple single-page expense tracker built in plain HTML, CSS, and JavaScript with Firebase Authentication, Firestore, and Chart.js.

This app lets a user sign in with Google, save categorized expenses by date, review previous expenses from the calendar, and view category-based summaries in a chart and table.

## Features

- Google sign-in and sign-out with Firebase Authentication
- Add expenses with:
  - calendar-based date selection
  - amount input
  - category selection
- Create custom expense categories
- Calendar UI with:
  - selected date highlight
  - today highlight
  - visible markers for dates that already have expenses
- Expense history box that shows all saved expenses on or before the selected date
- Summary tab with:
  - start and end date filters
  - total by category
  - pie chart visualization using Chart.js
- Inline success and error notifications
- Fixed footer with developer credit

## Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript
- Firebase Auth
- Firebase Firestore
- Chart.js
- Google Fonts (`Inter`)

## Project Structure

This project is currently a single-file app:

```text
expense-tracker/
├── index.html
└── README.md
```

## How It Works

### Authentication

The app uses Firebase Google sign-in through:

- `firebase.auth.GoogleAuthProvider()`
- `auth.signInWithPopup(provider)`

When a user logs in:

- their email is displayed
- categories are loaded from Firestore
- expenses are loaded from Firestore

When a user logs out:

- the category dropdown resets
- loaded expenses are cleared from memory
- the calendar and expense history panel reset

### Expense Management

Each expense is stored in the Firestore `expenses` collection with:

- `date`
- `amount`
- `category`
- `user`
- `createdAt`

The Add Expense section allows the user to:

- choose a date from the custom calendar
- enter an amount
- select a category
- save the expense to Firestore

After saving:

- the amount field is cleared
- the category selection is reset
- expenses are reloaded
- the calendar refreshes
- the expense history box updates

### Category Management

Categories are stored in the Firestore `categories` collection with:

- `name`
- `user`
- `createdAt`

Users can add their own custom categories, and only categories for the logged-in user are loaded into the dropdown.

### Calendar Behavior

The custom calendar supports:

- previous and next month navigation
- selected-day highlighting
- today highlighting
- expense-date highlighting using the `has-expense` class

When a date is selected:

- the hidden `#date` input is updated
- the date label is shown in human-readable format
- the expense history panel updates

### Expense History Panel

The expense history panel appears below the `Save Expense` button.

It shows:

- all expenses saved on or before the selected date
- category name
- formatted date
- formatted amount
- total amount for the filtered list

If no date is selected, it prompts the user to select a date.

If no expenses exist before or on that date, it shows an empty-state message.

### Summary View

The Summary tab allows the user to filter expenses using:

- `startDate`
- `endDate`

It then:

- groups expenses by category
- calculates totals
- renders a table
- renders a pie chart

If no data matches the range, the table displays `No expenses found`.

## Firestore Collections

### `expenses`

Example document:

```json
{
  "date": "2026-04-20",
  "amount": 25.5,
  "category": "Food",
  "user": "firebase-user-uid",
  "createdAt": "Firestore Timestamp"
}
```

### `categories`

Example document:

```json
{
  "name": "Transport",
  "user": "firebase-user-uid",
  "createdAt": "Firestore Timestamp"
}
```

## Setup

### 1. Clone or download the project

Place the project in a local folder and open it in your editor.

### 2. Create a Firebase project

In Firebase:

- create a new project
- enable Google Authentication
- create a Firestore database

### 3. Update Firebase configuration

Inside `index.html`, update the `firebaseConfig` object with your Firebase project credentials if needed.

Current config is embedded directly in the page for local development.

## Running the App

Because this is a static single-page app, you can run it with any local web server.

Examples:

```bash
python3 -m http.server 8000
```

or

```bash
npx serve .
```

Then open:

```text
http://localhost:8000
```

## Important Notes

- Firebase configuration is currently hardcoded in `index.html`
- Firestore security rules should be configured to ensure users can only access their own data
- Input values are sanitized before being rendered in the UI
- The app is written in one HTML file, so scaling it further would benefit from splitting HTML, CSS, and JavaScript into separate files

## Suggested Firestore Security Direction

At a minimum, rules should ensure:

- authenticated users can only read their own documents
- authenticated users can only write documents where `user == request.auth.uid`

## UI Sections

The page contains:

- Authentication card
- Add Expense tab
- Calendar date picker
- Category management form
- Expense history panel
- Summary tab
- Fixed footer crediting the developer

## Developer Credit

Footer text:

```text
Copyright © Taher Shabbiri. Developed by Taher Shabbiri.
```

## Future Improvements

- Edit and delete expenses
- Better expense sorting and grouping
- Monthly and yearly summary views
- Export expenses to CSV
- Stronger Firestore query filtering for large datasets
- Move secrets/config to environment-based deployment setup
- Split code into reusable modules

## License

No license file is currently included in this repository. Add one if you want to define reuse permissions.
