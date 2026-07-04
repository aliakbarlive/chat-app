# Chat App

A real-time one-to-one and group chat application built with the MERN stack and Socket.IO.

## Overview

This project is a full-stack chat application with a Node.js/Express/MongoDB backend and a React/TypeScript frontend. It supports user authentication, direct messaging, group chat rooms, and live updates such as presence, typing indicators, and read receipts.

## Problem It Solves

Building a real-time chat feature requires coordinating authentication, a persistent message store, and a live communication layer (WebSockets) that stays in sync with the REST API. This project demonstrates that full flow end to end, from login to live message delivery.

## Features

- User registration and login with JWT access and refresh tokens
- Refresh token stored in an httpOnly, signed cookie
- Password hashing with bcrypt and request validation with express-validator
- Protected API routes via token-verification middleware
- One-to-one direct messaging and group chat rooms
- Real-time message delivery, online/offline presence, typing indicators, and read receipts via Socket.IO
- Automatic access-token refresh on the frontend when a request receives a 403
- Toast notifications for errors and events

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| Frontend tooling | React Router, Axios, Socket.IO client, react-toastify |
| Backend | Node.js, Express |
| Database | MongoDB (Mongoose) |
| Real-time | Socket.IO |
| Auth | JWT (`jsonwebtoken`), bcryptjs, express-validator |

## Architecture / Project Structure

```text
chat-app/
  index.js                    # Express app entry point, DB connection, socket init
  controllers/
    auth.js                   # Register, login, refresh, logout
    user.js                   # Contacts, messages, rooms
  middleware/
    authenticateToken.js      # JWT access token verification
  model/
    User.js
    Message.js
    Room.js                   # Group chat room schema
  routes/
    auth.js
    user.js
  socket/
    index.js                  # Presence, messaging, typing, and room events
  client/                     # React + TypeScript frontend (Vite)
    src/
      pages/Auth/             # Login, SignUp
      pages/Home/             # Main chat interface
      context/                # Auth, Chat, and Socket context providers
      services/api/           # REST API calls
      services/socket/        # Socket.IO client wiring
      hooks/                  # useAxios (auto token refresh), useLocalStorage
```

## Getting Started

### Prerequisites

- Node.js
- Yarn
- MongoDB (local instance or Atlas)

### Installation

Backend:

```
git clone https://github.com/aliakbarlive/chat-app.git
cd chat-app
yarn install
```

Frontend:

```
cd client
yarn install
```

## Environment Variables

Backend `.env` (project root):

```
PORT=5000
CLIENT_URL=http://localhost:3000
MONGO_URI=your_mongodb_connection_string
ACCESS_TOKEN_SECRET=your_access_token_secret
REFRESH_TOKEN_SECERT=your_refresh_token_secret
COOKIE_SIGNATURE=your_cookie_signing_secret
```

> Note: `REFRESH_TOKEN_SECERT` is spelled this way in the source code — use this exact variable name for the token refresh flow to work.

Frontend `client/.env`:

```
VITE_SERVER_URL=http://localhost:5000
```

## Run Locally

Backend:

```
yarn dev
```

Frontend (in a separate terminal):

```
cd client
yarn dev
```

## Available Scripts

Backend:

| Command | Purpose |
|---|---|
| `yarn dev` | Start the backend with nodemon |
| `yarn start` | Run the backend with node |
| `yarn test` | Placeholder — no tests configured yet |

Frontend:

| Command | Purpose |
|---|---|
| `yarn dev` | Start the Vite development server |
| `yarn build` | Type-check and build for production |
| `yarn lint` | Run ESLint |
| `yarn preview` | Preview the production build locally |

## API / App Flow

A user registers or logs in through `/api/auth`, receiving an access token and a signed refresh-token cookie. Authenticated requests to `/api/user/*` (contacts, messages, rooms) go through the token-verification middleware. Once connected, the frontend opens a Socket.IO connection to receive real-time events: online/offline presence, incoming messages, typing notifications, and read-receipt updates. Users can message each other directly or create/join a group chat room.

## Screenshots

Screenshots can be added later after running the application locally.

## Roadmap

- Add automated tests for API routes and socket events
- Add message pagination for larger chat histories
- Add file or image sharing in messages
- Add a Docker setup for local development
- Rename `REFRESH_TOKEN_SECERT` to `REFRESH_TOKEN_SECRET` in a future code cleanup

## License

This project is licensed under the MIT License.
