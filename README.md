# TerraGo — Eco Travel Planner

TerraGo is a responsive eco-tourism web application for discovering sustainable activities, accommodation, and transport around Kuala Lumpur. It combines account management, travel planning, weather forecasts, favourites, reviews, and a carbon footprint calculator in one Express application.

## Features

- **User accounts:** Registration and login with hashed passwords, HTTP-only JWT cookies, profile updates, password changes, and account deletion.
- **Travel planner:** Search and browse activities, eco-friendly accommodation, and public or private transport options.
- **Favourite places:** Save and remove favourites by category and image details.
- **Itinerary builder:** Add activities to day cards and manage a trip directly on the travel planner page.
- **Weather forecasts:** Search by city or use browser geolocation to view current weather and a five-day forecast through OpenWeatherMap.
- **Carbon footprint calculator:** Estimate transport and optional accommodation emissions in kilograms of CO₂ equivalent.
- **Profile management:** Update profile information and upload a profile picture.
- **Reviews:** Submit and view account-linked reviews.
- **Responsive navigation:** Hamburger/sidebar navigation and Bootstrap-based layouts across desktop and mobile screens.

## Technology Stack

- **Server:** Node.js and Express 5
- **Database:** MongoDB with Mongoose
- **Views:** EJS templates
- **Authentication:** JSON Web Tokens and bcrypt/bcryptjs
- **Uploads:** Multer
- **Frontend:** HTML5, CSS3, vanilla JavaScript, and Bootstrap 5
- **External data:** OpenWeatherMap and OpenTripMap APIs

## Project Structure

```text
G8_WP/
├── app.js                  # Express server, routes, models, and middleware
├── backend/
│   ├── config.js           # Optional/backend configuration reference
│   └── models/             # Accommodation, favourite, review, and transport schemas
├── views/                 # EJS pages and shared partials
├── public/
│   ├── css/               # Page and component styles
│   ├── js/                # Browser-side application logic
│   ├── assets/images/     # Images and transportation assets
│   ├── icons/             # Navigation and interface icons
│   └── uploads/           # User-uploaded profile images
├── js/                    # Additional frontend configuration and scripts
└── package.json
```

## Prerequisites

Before running TerraGo, install:

- [Node.js](https://nodejs.org/) 18 or newer
- npm (included with Node.js)
- A [MongoDB Atlas](https://www.mongodb.com/atlas) account or a local MongoDB server
- An OpenWeatherMap API key for weather features
- An OpenTripMap API key for activity search

## Setup Instructions

### 1. Clone and enter the repository

```bash
git clone https://github.com/1syahirah/G8_WP.git
cd G8_WP
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure MongoDB

Create a database in MongoDB Atlas or start a local MongoDB instance. The application currently sets its MongoDB connection string in [`app.js`](/Users/amnasuraya/VS%20Code%20files/G8_WP/app.js).

Replace the `MONGODB_URI` value near the MongoDB connection section with your own connection string, for example:

```text
mongodb+srv://<username>:<password>@<cluster>/<database>?retryWrites=true&w=majority
```

If the password contains URL-reserved characters, URL-encode it before adding it to the connection string.

> For a production deployment, move the database URI, JWT secret, and third-party API keys out of source code and into environment variables. Never commit credentials to the repository.

### 4. Configure authentication

The application currently signs and verifies JWT cookies with the literal value `secret_key` in [`app.js`](/Users/amnasuraya/VS%20Code%20files/G8_WP/app.js). Replace both uses with one strong, unique secret before deployment.

### 5. Configure API keys

The weather and activity features use keys stored in browser-side JavaScript:

- Weather API configuration: [`public/js/weather.js`](/Users/amnasuraya/VS%20Code%20files/G8_WP/public/js/weather.js)
- Activity API configuration: [`public/js/travelActivities.js`](/Users/amnasuraya/VS%20Code%20files/G8_WP/public/js/travelActivities.js)
- Activity search configuration: [`public/js/travelFeatures.js`](/Users/amnasuraya/VS%20Code%20files/G8_WP/public/js/travelFeatures.js)

Register for your own OpenWeatherMap and OpenTripMap keys, then replace the existing placeholders. Browser keys are visible to users, so use provider-recommended restrictions where available and rotate any keys previously exposed in the repository.

### 6. Start the server

```bash
node app.js
```

For automatic restarts during development, use the installed Nodemon package:

```bash
npx nodemon app.js
```

By default, the application starts on `http://localhost:3000`. The port can be changed with the `PORT` environment variable:

```bash
PORT=4000 node app.js
```

## Usage

### Create an account

1. Open `http://localhost:3000/register`.
2. Enter a name, email address, password, phone number, and optional profile picture.
3. Submit the form and log in at `/login`.

### Use the main modules

- **Home:** Visit `/` to see the TerraGo dashboard and featured scenery.
- **Profile:** Visit `/profileManagement` after logging in to update your name, email, phone number, password, or profile picture.
- **Travel Planner:** Visit `/travelPlanner` to search activities, accommodation, and transport. Build a day-based itinerary and save favourites.
- **Weather:** Visit `/weatherModule`, enter a city, or allow browser location access for a local forecast.
- **Carbon Calculator:** Visit `/carbonFootprint`, choose a transport mode, enter distance and travellers, and optionally include the number of stay days.
- **Reviews and favourites:** View or submit reviews from the profile interface and save travel options from the planner cards.
- **Logout:** Use the sidebar or navigation link to clear the JWT cookie and return to login.

## Main Routes

| Route | Method | Access | Purpose |
|---|---|---|---|
| `/` | GET | Public | Dashboard |
| `/register` | GET/POST | Public | Registration form and account creation |
| `/login` | GET | Public | Login page |
| `/api/login` | POST | Public | Authenticate user and set JWT cookie |
| `/profileManagement` | GET | Authenticated | Profile page |
| `/updateProfilePic` | POST | Authenticated | Upload a profile picture |
| `/updateName` | POST | Authenticated | Update the account name |
| `/updateEmail` | POST | Authenticated | Update the account email |
| `/updatePhoneNumber` | POST | Authenticated | Update the account phone number |
| `/changePassword` | POST | Authenticated | Change the account password |
| `/deleteProfile` | POST | Authenticated | Delete the account and profile picture |
| `/travelPlanner` | GET | Public | Travel planner interface |
| `/weatherModule` | GET | Public | Weather interface |
| `/carbonFootprint` | GET | Public | Carbon footprint calculator |
| `/api/accomodation` | GET | Public | List accommodation data |
| `/api/transports` | GET | Public | List transport data |
| `/api/favourites` | GET/POST | Public | List or save favourites |
| `/api/favourites/:name` | DELETE | Public | Remove a favourite |
| `/api/reviews` | GET/POST | POST: authenticated | List or submit reviews |
| `/logout` | GET | Public | Clear the JWT cookie |

## Browser Permissions

The weather module requests access to your location only through the browser geolocation API. Location access is optional; location-based weather can be replaced by a manual city search.

## Data and Upload Notes

- Profile images are stored in `public/uploads/` and only image uploads are accepted by Multer.
- Travel data is retrieved from MongoDB and the configured activity provider APIs.
- The carbon footprint estimates use simplified emission factors and are intended as indicative planning values, not official lifecycle assessments.
- Uploaded files are removed when the associated profile or profile account is deleted.

## Troubleshooting

### MongoDB connection fails

- Confirm the username, password, database name, and connection string are correct.
- Allow your current IP address in MongoDB Atlas network access settings.
- Check that the database service is running if you use a local MongoDB instance.

### Port 3000 is already in use

Start the server on another port:

```bash
PORT=4000 node app.js
```

### Weather or activity data does not load

- Confirm that the relevant API key is valid and has provider access.
- Check browser developer tools for blocked requests, invalid keys, quota limits, or CORS errors.
- Ensure location permissions are enabled if automatic weather lookup is selected.

### Login succeeds but the profile cannot be opened

- Check that cookies are enabled in the browser.
- Confirm the same JWT secret is used for signing and verification.
- Log in again after restarting the server because JWT cookies have a one-hour lifetime.

## Tests

No automated test suite is currently configured. The `npm test` script reports that no tests are specified. For manual verification, start the server and test registration, login, profile updates, travel planning, favourites, weather search, and carbon calculations in the browser.

## Security

This project is a development prototype. Before production use:

- Move database credentials, JWT secrets, and API keys to environment variables.
- Rotate credentials that have been committed to source control.
- Add server-side session/token validation and stronger input validation.
- Restrict CORS, enforce password rules, add rate limiting, and enable HTTPS.
- Use secure cookie settings, including `secure: true` on HTTPS deployments.

## License

This repository declares the **ISC License** in `package.json`. Review the repository terms before redistribution.

## Repository

- **GitHub:** https://github.com/1syahirah/G8_WP
