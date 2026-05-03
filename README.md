# 🏛️ Welfora — Blockchain-Based Farmer & Land Record Management System

A full-stack decentralized application that replaces fragile manual land records with **cryptographically verified, tamper-proof digital governance** — powered by the Polygon blockchain.

---

## 📌 Problem Statement

All farmer and land records are currently maintained **manually** by government departments, leading to:
- Difficulty in access and retrieval of records
- Vulnerability to **fraudulent tampering** of ownership and acreage
- Leakage of government subsidies to **ghost beneficiaries**

**Our Solution**: Design a blockchain-based mechanism to ensure frauds are eliminated in land records, thereby assisting in maintaining the documents.

---

## 🏗️ System Architecture

```
React (Vite) ←→ Node.js (Express) ←→ MongoDB + Cloudinary
       ↓                ↓
   MetaMask        SHA-256 Hashing
       ↓                ↓
   Polygon Amoy Blockchain (LandRegistry.sol)
```

**Hybrid Model**: Heavy data (PDFs, images) stored off-chain in MongoDB/Cloudinary. Only the cryptographic **SHA-256 hash** is minted on-chain — ensuring security at near-zero cost.

---

## ✨ Key Features

| Module | Description |
|:-------|:------------|
| 🔐 **Authentication (RBAC)** | JWT-based login with 3 roles: Farmer, Officer, Admin |
| 👨‍🌾 **Farmer Module** | KYC registration, land registration, deed upload, scheme application |
| 👮 **Officer Module** | Farmer verification, field inspection reports, approve/reject |
| 🛡️ **Admin Module** | Final approval, **blockchain minting**, transfer management, scheme creation |
| ⛓️ **Blockchain Module** | Solidity smart contract on Polygon Amoy — immutable land hash storage |
| 🔍 **Public Verification** | Anyone can verify a land record against the blockchain (no login needed) |
| 📋 **Scheme Module** | Government scheme distribution linked to blockchain-verified land |
| 🔄 **Transfer Module** | Secure ownership transfer with Chain of Title history |
| 🔔 **Notifications** | Real-time in-app alerts for all role-based actions |
| 🤖 **AI Chatbot** | Google Gemini-powered assistant for farmer queries |

---

## 🔒 How Fraud Detection Works

```
1. Admin mints land → SHA-256 hash stored on Polygon Blockchain
2. Hacker changes "2 acres" to "20 acres" in the database
3. User searches the Survey Number on the public verification page
4. System re-calculates hash from current DB data
5. Compares with blockchain hash → MISMATCH → 🚨 FRAUD DETECTED
```

> Even changing **one character** in the database produces a completely different hash. Tampering is mathematically impossible to hide.

---

## 🛠️ Tech Stack

| Layer | Technology |
|:------|:-----------|
| **Frontend** | React 18, Vite, Tailwind CSS, Ethers.js v6, React Router |
| **Backend** | Node.js, Express.js, Mongoose, JWT, bcrypt, Multer |
| **Database** | MongoDB Atlas, Cloudinary |
| **Blockchain** | Solidity 0.8+, Hardhat, Polygon Amoy Testnet |
| **Wallet** | MetaMask |
| **AI** | Google Gemini API |
| **Hosting** | Vercel (Frontend), Render (Backend) |

---

## 📂 Project Structure

```
├── client/                # React Frontend (Vite)
│   ├── src/
│   │   ├── components/    # Navbar, Chatbot, ServerLoader
│   │   ├── pages/         # Dashboard, Login, VerifyLand, etc.
│   │   └── services/      # API client (Axios)
│
├── server/                # Node.js Backend
│   ├── controllers/       # Auth, Land, Transfer, Scheme, Notification
│   ├── models/            # User, Land, Scheme, TransferRequest
│   ├── routes/            # REST API routes
│   ├── middleware/         # JWT auth middleware
│   └── utils/             # Notification helpers
│
├── blockchain/            # Smart Contract
│   ├── contracts/         # LandRegistry.sol
│   ├── scripts/           # deploy.js
│   └── hardhat.config.js
```

---

## ⚡ Complete Setup & Run Guide

### 1. Prerequisites
- **Node.js** v18+
- **MetaMask** Browser Extension
- **MongoDB Atlas** Account (for database)
- **Cloudinary** Account (for PDF/Image storage)
- **Google Account** (for Apps Script / Gmail Notifications)

### 2. Clone the Repository
```bash
git clone https://github.com/your-username/welfora.git
cd welfora
```

### 3. Backend Setup (Node.js/Express)
The backend handles the API, database operations, and hashing.
```bash
cd server
npm install
```
- Create a `.env` file in the `server` directory (use `.env.example` as a template).
- Fill in your `MONGODB_URI`, `CLOUDINARY` keys, `GEMINI_API_KEY`, etc.
- Start the server:
```bash
npm run dev
# The backend will start on http://localhost:5000
```

### 4. Frontend Setup (React/Vite)
The frontend is the user interface for Farmers, Officers, and Admins.
```bash
cd client
npm install
```
- Create a `.env` file in the `client` directory (if required) to set the API URL.
- Start the development server:
```bash
npm run dev
# The frontend will start on http://localhost:3000
```

### 5. Blockchain Setup (Polygon Amoy Testnet)
The blockchain module contains the Solidity smart contract.
```bash
cd blockchain
npm install
```
1. Create a `.env` file in the `blockchain` directory.
2. Add your `PRIVATE_KEY` (from MetaMask) and `POLYGON_AMOY_RPC_URL`.
3. Compile and Deploy the contract:
```bash
npx hardhat compile
npx hardhat run scripts/deploy.js --network amoy
```
4. **Important**: Copy the deployed contract address output from the terminal and update the `SMART_CONTRACT_ADDRESS` in your `server/.env`.

### 6. Google Apps Script Setup (Email Notifications)
We use a custom Google Apps Script to send real-time email notifications for approvals, rejections, and transfers.
1. Go to [script.google.com](https://script.google.com/) and create a New Project.
2. Open `server/EMAIL_SETUP.md` in the repository and copy the provided `.gs` code.
3. Paste the code into your Google Apps Script editor.
4. Click **Deploy > New Deployment**.
   - Select type: **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Click **Deploy**, authorize the permissions, and copy the generated **Web App URL**.
6. Paste this URL into your `server/.env` as `APPS_SCRIPT_URL=your_url_here`.

### 7. MetaMask Configuration (For Testing)
To interact with the app (Admin minting):
1. Install **MetaMask** and create a wallet.
2. Add the **Polygon Amoy Testnet** network to MetaMask.
3. Get free test MATIC from the [Polygon Faucet](https://faucet.polygon.technology/).
4. Log into the app as an Admin to start minting land hashes!

---

## 📜 Smart Contract

| Detail | Value |
|:-------|:------|
| **Contract** | `LandRegistry.sol` |
| **Network** | Polygon Amoy Testnet |
| **Address** | `0x180845C148eb4fA5a84D362AD2d6e2Cc1AaA36E4` |
| **Functions** | `registerLand()`, `transferLand()`, `getLand()` |
| **Gas Cost** | ~0.005 MATIC per transaction |

---

## 🔄 Land Transfer — Chain of Title

We do **NOT** erase the old owner. We **append** a new transaction:

- **Block #100**: "SR-101 belongs to Ramesh." *(Permanent)*
- **Block #200**: "SR-101 transferred to Suresh." *(Permanent)*

Both records exist forever. The smart contract points to the **latest** hash. This creates a complete, auditable ownership history — no "backdoor edits" possible.

---

## 📋 Scheme Integration

Government schemes (PM-KISAN, subsidies, insurance) are linked to **blockchain-verified land**:

1. Farmer's land is minted on blockchain ✅
2. Farmer applies for a scheme
3. System validates: Does this farmer have verified land? ✅
4. Application proceeds — **no ghost beneficiaries**

---

## 🔑 Keywords

`Blockchain` · `Smart Contracts` · `Polygon` · `SHA-256` · `Immutability` · `Digital Governance` · `MERN Stack` · `Cryptographic Verification` · `Decentralized Ledger` · `Role-Based Access Control`

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
