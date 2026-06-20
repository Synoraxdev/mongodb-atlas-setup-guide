# 🍃 MongoDB Atlas Setup Guide
 
> Guide by **Synora 乂 Development** — Support Server: [dsc.gg/synoraxdev](https://dsc.gg/synoraxdev)
 
Complete step-by-step guide to set up MongoDB Atlas for your Node.js or TypeScript project. Every step is explained in full detail so even a complete beginner can follow along without getting stuck.
 
---
 
## 📋 Table of Contents
 
| N | Section | Description |
|---|---------|-------------|
| 1 | [What is MongoDB Atlas?](#-what-is-mongodb-atlas) | Understanding what you're setting up |
| 2 | [Prerequisites](#-prerequisites) | What you need before starting |
| 3 | [Create Account](#-step-1--create-mongodb-atlas-account) | Sign up on the Atlas platform |
| 4 | [Create Free Cluster](#-step-2--create-a-free-cluster) | Deploy your free M0 database |
| 5 | [Create Database User](#-step-3--create-a-database-user) | Set up login credentials |
| 6 | [Allow Connections](#-step-4--allow-network-connections) | Open network access |
| 7 | [Get Connection URI](#-step-5--get-your-connection-uri) | Copy your connection string |
| 8 | [Install Mongoose](#-step-6--install-mongoose-package) | Add the Node.js driver |
| 9 | [Connect in Code](#-step-7--connect-mongodb-in-your-project) | Write the connection logic |
| 10 | [Environment Setup](#-step-8--environment-variable-setup) | Secure your credentials |
| 11 | [Verify Connection](#-step-9--verify-your-connection) | Test that everything works |
| 12 | [Common Errors](#-common-errors--fixes) | Troubleshooting guide |
| 13 | [Project Structure](#-recommended-project-structure) | Where to put your files |
 
---
 
## 🍃 What is MongoDB Atlas?
 
**MongoDB** is a **NoSQL database** — unlike traditional SQL databases (like MySQL), MongoDB stores data as **JSON-like documents** instead of rows and columns in a table.
 
**MongoDB Atlas** is the **cloud-hosted version** of MongoDB. Instead of installing a database server on your own machine, Atlas hosts it for you in the cloud. You just connect to it from your code.
 
### Why use it for Discord bots?
 
| Feature | Benefit |
|---------|---------|
| Free M0 tier | 512MB storage, no credit card needed |
| Always online | Your data persists even when your bot restarts |
| No local setup | No need to install MongoDB on your server/VPS |
| Scalable | Upgrade plan later if your bot grows |
| JSON documents | Perfect match for JavaScript/TypeScript objects |
 
### MongoDB vs SQL — Quick Comparison
 
```
SQL (MySQL/PostgreSQL)          MongoDB Atlas
------------------------------  ------------------------------
Tables                          Collections
Rows                            Documents
Columns                         Fields
JOIN queries                    Nested documents / references
Strict schema required          Flexible schema (no rigid rules)
```
 
---
 
## ✅ Prerequisites
 
Before starting, make sure you have:
 
- [ ] A working **Node.js** project (v18+ recommended)
- [ ] **npm** installed (`npm -v` to check)
- [ ] A valid **email address** to register on Atlas
- [ ] Basic knowledge of **JavaScript or TypeScript**
- [ ] A `.env` file setup in your project *(covered in Step 8)*
---
 
## 🚀 Step 1 — Create MongoDB Atlas Account
 
### 1.1 — Go to the Atlas Website
 
Open your browser and go to:
 
```
https://www.mongodb.com/cloud/atlas
```
 
### 1.2 — Register
 
- Click the **"Try Free"** button (top right corner)
- You can sign up with:
  - **Email + Password** — fill in the form
  - **Google Account** — click "Sign up with Google" for faster setup
- Accept the Terms of Service
- Click **"Create your Atlas account"**
### 1.3 — Complete Onboarding
 
After registration, Atlas shows a quick survey:
 
```
"What is your goal today?"     → Select anything, or click "Finish" to skip
"What type of application?"    → Skip
"Preferred language?"          → Select JavaScript or TypeScript
```
 
> You can skip all of this — it does not affect your database setup.
 
### 1.4 — You're In
 
You will land on the **Atlas Dashboard**. This is your control panel for everything — clusters, users, network access, and more.
 
---
 
## 🖥️ Step 2 — Create a Free Cluster
 
A **cluster** is your actual database server. The **M0 Free tier** gives you a permanently free shared cluster with 512MB storage — more than enough for most bots.
 
### 2.1 — Start Creation
 
On the Atlas Dashboard:
 
- Click the green **`+ Create`** button
- You will see a tier selection screen
### 2.2 — Select the Free Tier
 
```
M0    →  Free Forever   ✅ SELECT THIS
M2    →  $9/month
M5    →  $25/month
```
 
> **Important:** Make sure you select `M0` — it clearly says **"Free"** with no credit card required.
 
### 2.3 — Choose Cloud Provider & Region
 
Pick any provider — all work the same for M0:
 
```
Provider    Best Region (pick closest to your users/VPS)
---------   -----------------------------------------------
AWS         us-east-1  (N. Virginia)   ← most popular
Google      us-central1 (Iowa)
Azure       eastus (Virginia)
```
 
> 💡 Tip: Choose a region **closest to your VPS or hosting location** for lower latency.
 
### 2.4 — Name Your Cluster
 
In the **Cluster Name** field, enter something like:
 
```
synora-cluster
```
 
Keep it lowercase, no spaces.
 
### 2.5 — Create the Cluster
 
- Click **"Create Deployment"**
- Atlas will start building your cluster
> ⏳ This takes **1 to 3 minutes**. Wait until the status changes to **"Active"** (shown in green).
 
---
 
## 👤 Step 3 — Create a Database User
 
A **Database User** is the login credentials your code uses to authenticate with MongoDB. This is **not** your Atlas account login — it's a separate user specifically for database access.
 
### 3.1 — Open Database Access
 
In the left sidebar, click:
 
```
Security → Database Access
```
 
### 3.2 — Add New User
 
- Click the **`+ Add New Database User`** button (top right)
- A modal/panel will appear
### 3.3 — Fill in the Details
 
**Authentication Method:** Select `Password`
 
**Username:**
```
synora-bot
```
 
**Password:**
```
MyStrongPass2024
```
 
> ⚠️ **Password Rules — IMPORTANT:**
> - Do **NOT** use these characters: `@` `:` `/` `#` `?` `=` `&`
> - These characters **break the URI format** and cause connection errors
> - Use only letters, numbers, underscores `_`, and hyphens `-`
> - Safe example: `Synora_Bot_2024`
> - Unsafe example: `pass@word:123` ← will break URI
 
**Built-in Role:** Click the dropdown and select:
 
```
Atlas admin
```
 
*(This gives full read/write access — recommended for bot usage)*
 
### 3.4 — Save the User
 
- Click **"Add User"**
- You will see the user appear in the list
> 📝 **Save your username and password right now.** You will need them in Step 5 to build your connection string.
 
---
 
## 🌐 Step 4 — Allow Network Connections
 
By default, Atlas **blocks all external connections** for security. You need to whitelist IP addresses that are allowed to connect to your database.
 
### 4.1 — Open Network Access
 
In the left sidebar, click:
 
```
Security → Network Access
```
 
### 4.2 — Add IP Address
 
- Click **`+ Add IP Address`** button
- A modal will appear with options
### 4.3 — Allow All IPs
 
- Click the **"Allow Access from Anywhere"** button
- This automatically fills in:
```
0.0.0.0/0
```
 
> **What does `0.0.0.0/0` mean?**
> This is CIDR notation meaning "all IP addresses". It allows connections from anywhere — your local machine, VPS, cloud hosting, etc.
 
### 4.4 — Add a Comment (Optional)
 
In the **Comment** field you can write:
 
```
Allow all - bot hosting
```
 
### 4.5 — Confirm
 
- Click **"Confirm"**
- The IP rule will appear in the list with status **"Active"**
> 🔐 **Security Note:** For production environments, you can restrict this to your VPS IP only. But for most bot hosting setups, `0.0.0.0/0` is the standard approach.
 
---
 
## 🔗 Step 5 — Get Your Connection URI
 
The **Connection URI** is a single URL that contains everything MongoDB needs to connect — the host, credentials, and options.
 
### 5.1 — Open Connect Dialog
 
Go back to your cluster on the dashboard and click the **`Connect`** button.
 
### 5.2 — Select Connection Method
 
A modal appears with options:
 
```
Shell              ← for command line access
Drivers            ← for code (SELECT THIS)
MongoDB Compass    ← for GUI
Atlas SQL          ← for SQL-style queries
```
 
Click **"Drivers"**.
 
### 5.3 — Select Driver Settings
 
- **Driver:** `Node.js`
- **Version:** `5.5 or later`
### 5.4 — Copy the URI
 
You will see a connection string like this:
 
```txt
mongodb+srv://USER:PASSWORD@synora-cluster.abc12.mongodb.net/?retryWrites=true&w=majority
```
 
Click the **copy icon** next to it.
 
### 5.5 — Replace the Placeholders
 
| Placeholder | Replace With | Example |
|-------------|--------------|---------|
| `USER` | Your database username from Step 3 | `synora-bot` |
| `PASSWORD` | Your database password from Step 3 | `Synora_Bot_2024` |
 
**Before:**
```txt
mongodb+srv://USER:PASSWORD@synora-cluster.abc12.mongodb.net/?retryWrites=true&w=majority
```
 
**After:**
```txt
mongodb+srv://synora-bot:Synora_Bot_2024@synora-cluster.abc12.mongodb.net/?retryWrites=true&w=majority
```
 
> 🔒 **NEVER paste this URI directly into your code files.** Always use environment variables (`.env`). See Step 8.
 
---
 
## 📦 Step 6 — Install Mongoose Package
 
**Mongoose** is the most popular Node.js library for working with MongoDB. It provides a clean API for connecting, querying, and defining data schemas.
 
### 6.1 — Open Your Terminal
 
```bash
cd your-bot-folder
```
 
### 6.2 — Install Mongoose
 
```bash
npm install mongoose
```
 
### 6.3 — Verify Installation
 
```bash
npm list mongoose
```
 
Expected output:
 
```
your-project@1.0.0
└── mongoose@8.x.x
```
 
### 6.4 — TypeScript Note
 
Mongoose **includes its own TypeScript types** built-in. Do **not** install `@types/mongoose` — it's outdated and causes conflicts.
 
```bash
# ✅ Correct
npm install mongoose
 
# ❌ Wrong — do NOT do this
npm install mongoose @types/mongoose
```
 
---
 
## 💻 Step 7 — Connect MongoDB in Your Project
 
### 7.1 — Create the Database File
 
Create `lib/database.ts` (or `lib/database.js` for plain JS):
 
**JavaScript (ESM)**
 
```js
import mongoose from "mongoose";
 
const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("✅ MongoDB Connected Successfully");
  } catch (error) {
    console.error("❌ MongoDB Connection Failed:", error);
    process.exit(1);
  }
};
 
export default connectDB;
```
 
**TypeScript**
 
```ts
import mongoose from "mongoose";
 
const connectDB = async (): Promise<void> => {
  try {
    await mongoose.connect(process.env.MONGO_URI as string);
    console.log("✅ MongoDB Connected Successfully");
  } catch (error) {
    console.error("❌ MongoDB Connection Failed:", error);
    process.exit(1);
  }
};
 
export default connectDB;
```
 
### 7.2 — Call connectDB on Startup
 
In your main entry file (`boot.ts`, `index.ts`, etc.):
 
```ts
import connectDB from "./lib/database.js";
import { Client, GatewayIntentBits } from "discord.js";
 
const client = new Client({ intents: [GatewayIntentBits.Guilds] });
 
// 1. Connect DB first
await connectDB();
 
// 2. Then login bot
await client.login(process.env.TOKEN);
```
 
> ⚠️ **Always connect to MongoDB BEFORE logging in your bot.** If your commands try to access the database before it's connected, they will fail or hang.
 
### 7.3 — One-liner Version (Simple Projects)
 
```js
import mongoose from "mongoose";
 
mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log("✅ MongoDB Connected"))
  .catch(console.error);
```
 
---
 
## 🔐 Step 8 — Environment Variable Setup
 
Your MongoDB URI contains your **username and password**. You must **never** hardcode it in your source files or push it to GitHub.
 
### 8.1 — Create `.env` File
 
```env
# Bot Token
TOKEN=your_discord_bot_token_here
 
# MongoDB URI
MONGO_URI=mongodb+srv://synora-bot:Synora_Bot_2024@synora-cluster.abc12.mongodb.net/?retryWrites=true&w=majority
```
 
### 8.2 — Create `.gitignore` File
 
```gitignore
# Dependencies
node_modules/
 
# Environment Variables — NEVER commit this
.env
 
# Build output
dist/
build/
 
# Logs
*.log
```
 
> 🚨 **Critical:** If you push `.env` to GitHub with your credentials, anyone can access your database. The `.gitignore` file tells Git to completely ignore the `.env` file.
 
### 8.3 — Load `.env` in Your Project
 
**Option A — Node.js v20.6+ (no package needed):**
 
```bash
node --env-file=.env boot.js
```
 
**Option B — `dotenv` package (works with all versions):**
 
```bash
npm install dotenv
```
 
Then at the very top of your entry file:
 
```ts
import "dotenv/config";
// Now process.env.MONGO_URI is available everywhere
```
 
### 8.4 — Access in Code
 
```ts
process.env.MONGO_URI    // your MongoDB connection string
process.env.TOKEN        // your Discord bot token
```
 
---
 
## ✔️ Step 9 — Verify Your Connection
 
Run your project and check the console output.
 
### Success
 
```
✅ MongoDB Connected Successfully
✅ Bot is online — Synora#1234
```
 
### Failure
 
```
❌ MongoDB Connection Failed: MongoServerError: Authentication failed
```
 
If you see an error, check the [Common Errors](#-common-errors--fixes) section below.
 
### Test with a Simple Write
 
```ts
import mongoose from "mongoose";
 
const TestSchema = new mongoose.Schema({ message: String });
const Test = mongoose.model("Test", TestSchema);
 
await connectDB();
 
await Test.create({ message: "Synora DB Test ✅" });
console.log("Test document written to MongoDB!");
 
const docs = await Test.find();
console.log("Documents in DB:", docs);
```
 
If you see your document printed, your Atlas setup is 100% working.
 
---
 
## ❌ Common Errors & Fixes
 
| Error Message | Cause | Fix |
|---------------|-------|-----|
| `Authentication failed` | Wrong username or password in URI | Go to `Security → Database Access`, verify credentials, update URI |
| `bad auth: Authentication failed` | Password has special characters | Remove `@` `:` `/` from password, update URI |
| `connect ECONNREFUSED` | Wrong URI format or cluster offline | Double-check URI, verify cluster is Active on Atlas |
| `IP not whitelisted` | Your IP is not allowed | Go to `Security → Network Access`, add `0.0.0.0/0` |
| `MongoParseError: URI malformed` | Special characters in password | Use only letters, numbers, `_`, `-` in password |
| `MongooseError: buffering timed out` | Queries ran before `connectDB()` finished | Always `await connectDB()` before any DB queries |
| `Cannot find module 'mongoose'` | Package not installed | Run `npm install mongoose` |
| `process.env.MONGO_URI is undefined` | `.env` not loaded | Add `import "dotenv/config"` at top of entry file |
| `MongoNetworkError: connection timed out` | Firewall blocking port 27017 | Check your VPS firewall settings |
 
---
 
## 📁 Recommended Project Structure
 
```
your-bot/
│
├── app/
│   ├── commands/
│   │   └── ping.ts
│   └── events/
│       └── ready.ts
│
├── lib/
│   └── database.ts         ← MongoDB connection logic lives here
│
├── storage/
│   └── data.json
│
├── .env                    ← MONGO_URI and TOKEN (never commit!)
├── .gitignore              ← Must include .env and node_modules
├── package.json
├── tsconfig.json
└── boot.ts                 ← Entry point: call connectDB() here first
```
 
---
 
## 📊 Quick Reference Cheat Sheet
 
```
What you need                   How to get it
-----------------------------   -----------------------------------------------
Free Cloud Database             MongoDB Atlas M0 tier (no credit card)
Dashboard URL                   https://cloud.mongodb.com
Add DB User                     Security → Database Access → Add New User
Allow All IPs                   Security → Network Access → Add 0.0.0.0/0
Get Connection String           Connect → Drivers → Node.js → Copy URI
Install Driver                  npm install mongoose
Load Environment Variables      npm install dotenv  →  import "dotenv/config"
Connect to DB                   mongoose.connect(process.env.MONGO_URI)
Confirm Connected               .then(() => console.log("Connected"))
Handle Error                    .catch(console.error) or try/catch
Keep URI Secret                 Store in .env, add .env to .gitignore
```
 
---
 
**Synora 乂 Development** — [Join Support Server](https://dsc.gg/synoraxdev)
 
*If this guide helped you, consider starring ⭐ the repository!*
