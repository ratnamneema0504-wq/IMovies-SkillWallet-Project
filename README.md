# iMovies — Movie Ticket Booking Platform

A full-stack movie discovery and ticket booking platform built with **React 18, Vite, Node.js, Express, and MongoDB**. iMovies offers an interactive booking experience with real-time seat availability, dynamic city and cinema selection, movie trailers, user reviews, and a streamlined checkout process. It also supports an in-memory data fallback when MongoDB is unavailable.

![Node.js](https://img.shields.io/badge/Node.js-v18%2B-green?logo=node.js)
![React](https://img.shields.io/badge/React-18-blue?logo=react)
![Vite](https://img.shields.io/badge/Vite-5.x-646CFF?logo=vite)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.x-38B2AC?logo=tailwind-css)
![MongoDB](https://img.shields.io/badge/MongoDB-Ready-47A248?logo=mongodb)
![Security Tests](https://img.shields.io/badge/Security_Tests-6%2F6_Passing-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## Table of Contents

* [Features](#features)
* [Technology Stack](#technology-stack)
* [Getting Started](#getting-started)
* [Environment Variables](#environment-variables)
* [Available Scripts](#available-scripts)
* [Testing and Security](#testing-and-security)
* [API Reference](#api-reference)
* [License](#license)

---

## Features

### Movie Discovery

* **Extensive Movie Catalog:** Browse 80+ curated movie titles with posters, genres, cast information, ratings, and runtime details.
* **Trailer Previews:** Watch movie trailers through interactive modal windows.
* **Movie Search and Filtering:** Discover movies using search queries, genres, and location-based filters.

### Booking and Reservations

* **Interactive Seat Selection:** Explore a visual seating layout featuring VIP, Premium, and Standard seating categories.
* **Live Seat Availability:** View occupied seats for specific movies, screening dates, and showtimes.
* **Atomic Seat Reservations:** Database-level uniqueness constraints help prevent duplicate bookings and race conditions.
* **Server-Side Price Validation:** Ticket prices are calculated and verified by the server to prevent client-side price manipulation.
* **Conflict Handling:** Booking conflicts return an HTTP `409 Conflict` response.

### Location and Cinema Discovery

* **City Selection:** Choose from supported metropolitan locations.
* **Cinema Locator:** Browse available cinemas and their locations.
* **Location-Aware Discovery:** Explore movie and cinema options based on supported city selections.

### Authentication and Security

* **JWT Authentication:** Secure authenticated requests using JSON Web Tokens.
* **Password Hashing:** Protect stored passwords with `bcryptjs`.
* **Role Protection:** Public registration cannot assign administrative privileges through client-supplied role values.
* **Protected Routes:** Restrict access to authenticated user profiles and booking operations.
* **Input Validation:** Validate booking dates and required screening information.

### Reliability and User Experience

* **In-Memory Fallback:** Continue using supported application features when MongoDB is unavailable, with pre-seeded movie and cinema data.
* **Image Proxy and Caching:** Cache movie posters in memory and retrieve remote images through a proxy.
* **Responsive Interface:** Use a mobile-friendly interface with a warm cream-and-crimson cinema-inspired design.
* **Smooth Interactions:** Enjoy animations and streamlined navigation throughout the booking experience.

---

## Technology Stack

### Frontend

| Technology         | Purpose                                            |
| ------------------ | -------------------------------------------------- |
| React 18           | Component-based user interface                     |
| Vite 5             | Development server and frontend build tool         |
| React Router DOM 7 | Client-side routing                                |
| Tailwind CSS 3     | Utility-first styling                              |
| PostCSS            | CSS processing                                     |
| Lucide React       | Interface icons                                    |
| Axios              | HTTP requests, token interception, and retry logic |

### Backend

| Technology          | Purpose                     |
| ------------------- | --------------------------- |
| Node.js 18+         | JavaScript runtime          |
| Express 4           | Backend server and REST API |
| MongoDB             | Persistent database         |
| Mongoose 8          | MongoDB object modeling     |
| JSON Web Tokens     | Authentication              |
| bcryptjs            | Password hashing            |
| In-memory datastore | Fallback data storage       |

---

## Getting Started

Follow these steps to run iMovies locally.

### 1. Prerequisites

Ensure the following are installed:

* **Node.js:** Version 18.0.0 or later
* **npm:** Version 9.0.0 or later
* **MongoDB:** Optional for local development when the in-memory fallback is supported by the application

### 2. Clone the Repository

```bash
git clone https://github.com/tanishqpardeshi/I-MOVIES-.git
cd I-MOVIES-
```

### 3. Install Dependencies

Install the project dependencies:

```bash
npm install
```

### 4. Configure Environment Variables

Copy the example environment file:

```bash
cp .env.example .env
```

If you're using Windows Command Prompt, you can run:

```cmd
copy .env.example .env
```

Update the variables in `.env` according to your local configuration.

### 5. Start the Development Server

Run:

```bash
npm run dev
```

Open your browser and navigate to:

**http://localhost:3000**

The application uses a unified Express server with Vite middleware in development mode.

---

## Environment Variables

The following environment variables configure the application.

| Variable                 | Description                                                | Example or Default                                |
| ------------------------ | ---------------------------------------------------------- | ------------------------------------------------- |
| `PORT`                   | Port used by the server                                    | `3000`                                            |
| `NODE_ENV`               | Application environment                                    | `development`                                     |
| `JWT_SECRET`             | Secret used to sign JWTs                                   | A strong, unique secret of at least 32 characters |
| `MONGO_URI`              | MongoDB connection string                                  | `mongodb://localhost:27017/imovies`               |
| `SEED_ADMIN_PASSWORD`    | Password used when seeding an administrator account        | Configure a strong password                       |
| `SEED_USER_PASSWORD`     | Password used when seeding a standard user account         | Configure a strong password                       |
| `SEED_ALLOW_DESTRUCTIVE` | Controls whether destructive database seeding is permitted | `false`                                           |

**Security recommendations:**

* Generate a strong, unique `JWT_SECRET`.
* Never commit your `.env` file or production credentials to version control.
* Use strong passwords for seeded accounts.
* Keep `SEED_ALLOW_DESTRUCTIVE=false` unless destructive seeding is explicitly required.
* Verify the database configuration before running seed commands.

---

## Available Scripts

| Command                        | Description                                                                   |
| ------------------------------ | ----------------------------------------------------------------------------- |
| `npm run dev`                  | Starts the development server with Vite middleware                            |
| `npm run build`                | Builds the production frontend into `client/dist`                             |
| `npm start`                    | Starts the production server                                                  |
| `npm run lint`                 | Runs syntax checks on the configured server files                             |
| `npm test`                     | Runs the automated security and regression test suite                         |
| `npm --prefix server run seed` | Runs the database seed script, subject to its configuration and safety checks |

**Note:** Build the frontend before starting the production server. The production server is expected to serve the generated frontend assets.

---

## Testing and Security

iMovies includes an automated security and regression test suite located at:

`server/test/security.test.js`

The documented test coverage includes the following areas:

| Test                            | Purpose                                                                                |
| ------------------------------- | -------------------------------------------------------------------------------------- |
| Privilege Escalation Protection | Ensures public registration cannot assign the `admin` role through user-supplied input |
| Server-Side Price Calculation   | Prevents clients from manipulating booking totals                                      |
| Atomic Seat Reservation         | Checks database uniqueness constraints for seat reservations                           |
| Concurrency Conflict Handling   | Verifies that conflicting bookings return HTTP `409 Conflict`                          |
| Date Validation                 | Rejects invalid calendar dates, such as `2026-02-31`                                   |
| Occupied-Seat Filter Validation | Requires screening dates and showtimes when retrieving occupied seats                  |

### Run the Tests

Execute:

```bash
npm test
```

The security badge at the top reflects the reported test status and should be updated whenever the test suite's results change.

---

## API Reference

The backend exposes REST API endpoints for movie discovery, authentication, cinema listings, bookings, and image delivery.

### General

| Method | Endpoint      | Description                                       | Authentication |
| ------ | ------------- | ------------------------------------------------- | -------------- |
| `GET`  | `/api/health` | Returns server health information and a timestamp | Not required   |

### Movies

| Method | Endpoint          | Description                                                     | Authentication |
| ------ | ----------------- | --------------------------------------------------------------- | -------------- |
| `GET`  | `/api/movies`     | Retrieves movies with supported genre, city, and search filters | Not required   |
| `GET`  | `/api/movies/:id` | Retrieves a movie by ID, including showtime information         | Not required   |

### Authentication

| Method | Endpoint             | Description                                | Authentication |
| ------ | -------------------- | ------------------------------------------ | -------------- |
| `POST` | `/api/auth/register` | Registers a new user account               | Not required   |
| `POST` | `/api/auth/login`    | Authenticates a user and returns a JWT     | Not required   |
| `GET`  | `/api/auth/me`       | Retrieves the authenticated user's profile | Required       |

For protected endpoints, include the JWT in the request header:

```http
Authorization: Bearer <token>
```

### Cinemas

| Method | Endpoint       | Description                                       | Authentication |
| ------ | -------------- | ------------------------------------------------- | -------------- |
| `GET`  | `/api/cinemas` | Retrieves supported cinema listings and locations | Not required   |

### Bookings

| Method | Endpoint                 | Description                                                                  | Authentication |
| ------ | ------------------------ | ---------------------------------------------------------------------------- | -------------- |
| `GET`  | `/api/bookings/occupied` | Retrieves occupied seats for a specified movie, screening date, and showtime | Not required   |
| `POST` | `/api/bookings`          | Creates a booking after server-side validation                               | Required       |
| `GET`  | `/api/bookings/my`       | Retrieves the authenticated user's bookings and digital passes               | Required       |

### Images

| Method | Endpoint            | Description                                     | Authentication |
| ------ | ------------------- | ----------------------------------------------- | -------------- |
| `GET`  | `/api/images/proxy` | Retrieves remote images through a caching proxy | Not required   |

**API note:** The exact request parameters, payload schemas, response formats, and error codes should be verified against the implementation in the repository.

---

## Project Structure

The application uses a full-stack structure with a frontend client and a Node.js backend.

```text
I-MOVIES-/
├── client/
│   └── dist/              # Production frontend build
├── server/
│   └── test/
│       └── security.test.js
├── server.js              # Unified development/server entry point
├── .env.example           # Example environment configuration
├── package.json           # Project scripts and dependencies
└── README.md
```

*This is a high-level overview. Additional directories and files may exist in the repository.*

---

## License

This project is licensed under the **MIT License**.

See the [`LICENSE`](LICENSE) file for the complete license terms.
