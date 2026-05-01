# 🌾 Land Verification & Scheme Application System - Phase 1

## 📋 Phase 1: Authentication & User Management

This is a complete implementation of Phase 1 including:

- Farmer registration with email, mobile, and district
- Secure login with JWT authentication
- Role-based access control (FARMER, OFFICER, ADMIN)
- User status management
- Officer creation by Admin
- Admin seeding on server startup

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

## ⚡ Quick Start

### Prerequisites
- Node.js v18+
- MetaMask Browser Extension
- MongoDB Atlas Account
- Cloudinary Account

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/welfora.git

# Install dependencies
cd client && npm install
cd ../server && npm install
cd ../blockchain && npm install

# Configure environment variables
# Copy .env.example to .env in both client/ and server/

# Run the application
cd server && npm run dev    # Backend on port 5000
cd client && npm run dev    # Frontend on port 3000
```

### For Team Members (Testing)
1. Install **MetaMask** browser extension
2. Create a new wallet
3. Add **Polygon Amoy Testnet** network
4. Get free test MATIC from [Polygon Faucet](https://faucet.polygon.technology/)
5. Open the hosted website link and start testing!

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

This project is developed for academic purposes under **Problem ID S0718** (Smart Governance / LegalTech).
