# MongoDB Atlas Setup — Complete Guide

> Guide by **Synora 乂 Development** • Support Server: [dsc.gg/synoraxdev](https://dsc.gg/synoraxdev)

MongoDB Atlas is the cloud database platform used across all Synora 乂 bots and services. This guide walks you through creating a free cluster, configuring access, and connecting it to your Node.js/TypeScript project from scratch.

---

## At a Glance

| Step | What You Do | Why |
|------|-------------|-----|
| Create Account | Sign up on Atlas | Access the cloud dashboard |
| Create Cluster | Deploy `M0 Free` tier | Free hosted MongoDB instance |
| Database User | Add username + password | Auth for your connection |
| Network Access | Whitelist `0.0.0.0/0` | Allow external connections |
| Get URI | Copy connection string | Link your bot to the database |
| Install Package | `npm install mongoose` | Node.js MongoDB driver |
| Connect | `mongoose.connect(URI)` | Establish live connection |

---

## Table of Contents

| # | Section | Description |
|---|---------|-------------|
| 1 | [Create Account](#1-create-mongodb-atlas-account) | Sign up and log into Atlas |
| 2 | [Create Free Cluster](#2-create-free-cluster) | Deploy a free M0 cluster |
| 3 | [Create Database User](#3-create-database-user) | Set up auth credentials |
| 4 | [Allow Connections](#4-allow-connections) | Configure network access |
| 5 | [Get Connection URI](#5-get-connection-uri) | Copy your MongoDB URI |
| 6 | [Install Package](#6-install-mongodb-package) | Add Mongoose to your project |
| 7 | [Connect to MongoDB](#7-connect-mongodb) | Write the connection code |

---

## Quick Reference

```
What you need              How to get it
------------------------   --------------------------------------------------
Free Database              M0 tier on MongoDB Atlas (no credit card needed)
Auth Credentials           Security → Database Access → Add New User
Allow All IPs              Security → Network Access → Add 0.0.0.0/0
Connection String          Connect → Drivers → Node.js → Copy URI
Node.js Driver             npm install mongoose
Connect in Code            mongoose.connect("YOUR_MONGO_URI")
Confirm Connection         .then(() => console.log("MongoDB Connected"))
Handle Errors              .catch(console.error)
```

---

## 1. Create MongoDB Atlas Account

Go to the official Atlas platform:

```
https://www.mongodb.com/cloud/atlas
```

- Click **Try Free** or **Sign In**
- Register with email or Google
- Complete the onboarding survey (can skip)
- You will land on the **Atlas Dashboard**

---

## 2. Create Free Cluster

From the dashboard:

- Click **`+ Create`**
- Select tier: **`M0 Free`** *(no credit card required)*
- Choose a **Cloud Provider** — any of these work:

```
Provider     Recommended Region
---------    ------------------
AWS          us-east-1 (N. Virginia)
Google       us-central1 (Iowa)
Azure        eastus (Virginia)
```

- Set a **Cluster Name** (e.g. `synora-cluster`)
- Click **Create Deployment**

> ⏳ Deployment takes 1–3 minutes. Wait until the status shows **Active**.

---

## 3. Create Database User

Navigate to:

```
Security → Database Access
```

- Click **`+ Add New Database User`**
- Set **Authentication Method** to `Password`
- Enter a **Username** (e.g. `synora-bot`)
- Enter a strong **Password** — save it, you'll need it in the URI
- Set **Built-in Role** to:

```
Atlas admin   ← recommended for bot usage
```

- Click **Add User**

> ⚠️ Do not use special characters like `@`, `:`, `/` in your password — they break the URI format.

---

## 4. Allow Connections

Navigate to:

```
Security → Network Access
```

- Click **`+ Add IP Address`**
- Click **Allow Access from Anywhere**
- This inserts:

```txt
0.0.0.0/0
```

- Click **Confirm**

> This allows your VPS, local machine, and any hosting environment to connect without IP whitelisting issues.

---

## 5. Get Connection URI

From the Atlas dashboard:

- Click **`Connect`** on your cluster
- Select **`Drivers`**
- Set **Driver** to `Node.js` and **Version** to `5.5 or later`
- Copy the connection string:

```txt
mongodb+srv://USER:PASSWORD@cluster.mongodb.net/?retryWrites=true&w=majority
```

Replace the placeholders before using:

| Placeholder | Replace With |
|-------------|--------------|
| `USER` | Your database username |
| `PASSWORD` | Your database password |
| `cluster` | Your Atlas cluster hostname (auto-filled) |

**Example after replacing:**

```txt
mongodb+srv://synora-bot:MyPass123@synora-cluster.abc12.mongodb.net/?retryWrites=true&w=majority
```

> 🔒 Never commit your URI to GitHub. Store it in `.env` and add `.env` to `.gitignore`.

---

## 6. Install MongoDB Package

In your project root, run:

```bash
npm install mongoose
```

For TypeScript projects, types are bundled with Mongoose — no extra `@types` package needed.

Verify install:

```bash
npm list mongoose
```

Expected output:

```
your-project@1.0.0
└── mongoose@8.x.x
```

---

## 7. Connect MongoDB

### JavaScript (ESM)

```js
import mongoose from "mongoose";

mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log("MongoDB Connected"))
  .catch(console.error);
```

### TypeScript

```ts
import mongoose from "mongoose";

const connectDB = async (): Promise<void> => {
  try {
    await mongoose.connect(process.env.MONGO_URI as string);
    console.log("MongoDB Connected");
  } catch (error) {
    console.error("MongoDB Connection Failed:", error);
    process.exit(1);
  }
};

export default connectDB;
```

### `.env` File

```env
MONGO_URI=mongodb+srv://synora-bot:MyPass123@synora-cluster.abc12.mongodb.net/?retryWrites=true&w=majority
```

### Call on Bot Startup

```ts
import connectDB from "./lib/database.js";

await connectDB();
// then login your Discord client
await client.login(process.env.TOKEN);
```

---

## Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `Authentication failed` | Wrong username or password in URI | Re-check credentials in Database Access |
| `IP not whitelisted` | Your IP is blocked | Add `0.0.0.0/0` in Network Access |
| `ECONNREFUSED` | Wrong URI or cluster not running | Verify URI, check Atlas cluster status |
| `URI malformed` | Special chars in password | Avoid `@`, `:`, `/` in password |
| `MongooseError: buffering timed out` | `connect()` not called before queries | Always await `connectDB()` before bot login |

---

## Project Structure (Recommended)

```
your-bot/
├── app/
│   ├── commands/
│   └── events/
├── lib/
│   └── database.ts       ← mongoose connect logic
├── storage/
│   └── *.json
├── .env                  ← MONGO_URI goes here
├── .gitignore            ← must include .env
└── boot.ts
```

---

<div align="center">

**Synora 乂 Development**

[Support Server](https://dsc.gg/synoraxdev) • MongoDB Atlas is now fully configured and ready for production use.

</div>
