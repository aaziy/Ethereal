# 🌌 Etherial: Decentralized Campus Rumor Verification System

**A fully decentralized, peer-to-peer platform where university students verify campus rumors and news through Byzantine-resistant consensus, blind authentication, and reputation-weighted voting.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-brightgreen.svg)]()
[![Tech Stack: Next.js + Gun.js](https://img.shields.io/badge/Stack-Next.js%20%2B%20Gun.js-blue.svg)]()

---

## 🚀 Quick Start

### Get Running in 2 Minutes

```bash
# 1. Install dependencies
pnpm install

# 2. Start development server (includes relay node)
pnpm run dev

# 3. Open browser
# Frontend: http://localhost:3000
# GunDB Relay: http://localhost:8765/gun

# 4. Test the system
# Use test credentials in QUICKSTART.md
```

**[→ Full Quick Start Guide](./etherial-rumor-verification-system/docs/QUICKSTART.md)**

---

## 🎯 What is Etherial?

Etherial solves a critical problem in campus communities: **How do you verify rumors and news when you can't trust centralized authorities?**

### The Problem
Traditional rumor verification systems fail because:
- ❌ Admins can censor or manipulate truth
- ❌ Students fear identity exposure and retaliation
- ❌ Bots and fake accounts manipulate votes
- ❌ Popular lies spread faster than truth
- ❌ No accountability for bad actors

### The Solution: Etherial

A **fully decentralized, zero-trust platform** where:

✅ **No Central Authority** — Truth is determined collectively, not by admins  
✅ **Anonymity by Design** — Blind cryptographic authentication, no email storage  
✅ **Reputation Matters** — Vote weight = √(your karma) prevents farming  
✅ **Structured Truth** — Rumors progress through timed voting windows  
✅ **Opposition System** — Anyone can challenge verified facts  
✅ **P2P Network** — Peer-to-peer replication, no single server required  

---

## ⭐ Key Innovations

### 1. **Blind Authentication (Zero-Knowledge)**
```
Email + Passphrase → [Gun.SEA Crypto] → Deterministic Keypair

Key Properties:
- ✨ Email never stored on any server
- ✨ Same credentials = same identity across sessions
- ✨ Cryptographically secure anonymous identity
- ✨ No central authentication server
```

### 2. **Square Root Karma Weighting**
```javascript
vote_weight = Math.sqrt(user_karma)

Examples:
karma=1   → weight=1.0    (new users have minimal influence)
karma=4   → weight=2.0
karma=9   → weight=3.0
karma=100 → weight=10.0   (NOT 100! — prevents farming)
```

**Why √?** Prevents users from building karma farms. A user with 100 karma votes with 10x power, not 100x.

### 3. **Byzantine-Resistant Resolution Engine**

The heart of Etherial: A mathematically sound system for converting votes into verified facts.

```
Rumor Lifecycle:
1. Submit → Posted immediately to all peers
2. Voting Window (48 hours) → Peer votes accumulate
3. Blind Counting → Vote totals hidden during voting
4. Resolution Phase → Once window closes:
   - If ratio ≥ 0.60 → FACT (verified)
   - If ratio ≤ 0.40 → FALSE (debunked)
   - If between → Extend window once, else UNVERIFIED

Quorum Requirements:
- Minimum 5 voters
- Minimum total weight ≥ 10
```

### 4. **Asymmetric Karma System**
```
Vote Outcomes:
- Voted with majority → +1.0 karma
- Voted against majority → -1.5 karma
- Posted false rumor → -2.0 karma

Effect: Bad actors lose reputation faster than good actors gain it
```

### 5. **Opposition Mechanism**
```
Any user with sufficient karma can:
- Challenge a FACT with new evidence
- Trigger re-voting on disputed claims
- Prevent stale information from becoming "permanent"
```

### 6. **Ghost Deletion System**
```
Soft-delete with cascading recalculation:
- Deleted rumors hidden from feed
- Dependent facts recalculated
- Referential integrity maintained across P2P network
- No orphaned data
```

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────┐
│              ETHERIAL P2P NETWORK                       │
│                                                         │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐             │
│  │ Peer A  │◄──►│ Peer B  │◄──►│ Peer C  │  ... (∞)    │
│  │ (Node)  │    │ (Node)  │    │ (Relay) │             │
│  └─────────┘    └─────────┘    └─────────┘             │
│       │              │              │                   │
│       └──────────────┼──────────────┘                   │
│                      │                                   │
│           ┌──────────▼──────────┐                        │
│           │   Gun.js Graph DB   │                        │
│           │  (Fully Replicated) │                        │
│           └──────────┬──────────┘                        │
│                      │                                   │
│      ┌───────────────┼───────────────┐                   │
│      │               │               │                   │
│  ┌───▼────┐      ┌───▼────┐     ┌───▼────┐             │
│  │nu.edu.pk│      │lums.edu.pk│ │nust.pk │ ...         │
│  └────────┘      └────────┘    └────────┘             │
│  (Community)     (Community)   (Community)             │
│                                                         │
│ Layers:                                                 │
│ • Auth: Blind Key Generation (Gun.SEA)                │
│ • Trust: Reputation Engine (Karma Score)              │
│ • Consensus: Weighted Byzantine Voting                │
│ • Lifecycle: Time Windows → Fact Lock → Opposition    │
└─────────────────────────────────────────────────────────┘
```

**Data Flow: Peer-to-Peer**
- Every student's browser is a node
- GunDB replicates rumors across all peers
- No single point of failure or control
- Automatic sync when peers connect

---

## 📂 Project Structure

```
Etherial/
├── README.md                           # This file
├── docs/
│   ├── QUICKSTART.md                   # ⭐ 2-minute startup guide
│   ├── ETHERIAL.md                     # 🔥 Complete feature specification
│   ├── ARCHITECTURE.md                 # System design & data flows
│   ├── IMPLEMENTATION_SUMMARY.md       # Code walkthrough
│   ├── TESTING.md                      # Test cases & verification
│   ├── JUDGES_QUICK_START.md           # For hackathon judges
│   └── ...
│
├── lib/
│   ├── gun-db.ts                       # GunDB client & type definitions
│   ├── auth-service.ts                 # Blind authentication (Gun.SEA)
│   ├── rumor-engine.ts                 # ⭐ Resolution algorithm (CORE)
│   ├── opposition-engine.ts            # Challenge mechanism
│   ├── reputation-logic.ts             # Karma calculation
│   ├── ghost-system.ts                 # Soft-deletion cascades
│   ├── resolution-scheduler.ts         # Time-window management
│   └── ...
│
├── components/
│   ├── auth-modal.tsx                  # Blind login interface
│   ├── rumor-card.tsx                  # Rumor display & voting
│   ├── truth-meter.tsx                 # Visual status indicator
│   ├── opposition-modal.tsx            # Challenge interface
│   ├── community-sidebar.tsx           # Domain selection
│   └── ui/                             # Shadcn UI components
│
├── app/
│   ├── page.tsx                        # Main dashboard
│   ├── layout.tsx                      # Root layout
│   └── globals.css                     # Global styles
│
├── server/
│   └── index.js                        # Gun.js relay server
│
├── __tests__/
│   ├── auth-modal.test.tsx
│   ├── rumor-card.test.tsx
│   └── ... (Jest test suite)
│
├── package.json                        # Dependencies
├── next.config.mjs                     # Next.js config
├── tsconfig.json                       # TypeScript config
└── tailwind.config.ts                  # Tailwind CSS config
```

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | Next.js 16, React 19 | Full-stack framework with App Router |
| **Database** | Gun.js P2P | Decentralized, replicated graph database |
| **Encryption** | Gun.SEA | Cryptographic authentication & signing |
| **UI Framework** | Shadcn UI | Beautiful, accessible React components |
| **Styling** | Tailwind CSS 4 | Utility-first CSS with design tokens |
| **Language** | TypeScript | Type-safe development |
| **Testing** | Jest + Supertest | Unit & integration tests |
| **Package Mgr** | pnpm | Fast, efficient node package manager |

---

## 🚀 Features

### ✅ Current Implementation

- [x] **Blind Authentication** — Email + passphrase → anonymous keypair
- [x] **Rumor Submission** — Post rumors to domain communities
- [x] **Weighted Voting** — Vote with influence = √(karma)
- [x] **Reputation System** — Karma earned/lost based on vote outcomes
- [x] **Resolution Engine** — Converts votes to FACT/FALSE/UNVERIFIED
- [x] **Opposition System** — Challenge verified facts
- [x] **Ghost Deletion** — Soft-delete with cascade recalculation
- [x] **P2P Sync** — Peer-to-peer GunDB replication
- [x] **Domain Communities** — Segregated by university email domain
- [x] **UI Components** — React components for all features
- [x] **Test Suite** — Jest tests covering core logic
- [x] **Documentation** — Comprehensive guides for developers & users

### 🔄 Key Workflows

**Creating & Verifying a Rumor:**
1. User authenticates with email + passphrase
2. System generates deterministic keypair (email never stored)
3. User submits rumor to their domain community
4. Rumor displays immediately across P2P network
5. Community votes for 48 hours (√karma weighted)
6. Vote counts hidden until window closes
7. System calculates resolution:
   - Ratio ≥0.60 = FACT
   - Ratio ≤0.40 = FALSE
   - In-between = extend once, then UNVERIFIED
8. Result immutable until challenged via opposition

**Challenging a Fact:**
1. User with sufficient karma clicks "Challenge"
2. Submits opposing evidence
3. Triggers new voting window
4. Community re-votes on disputed claim
5. New resolution replaces old one if threshold met

---

## 📊 Mathematical Guarantees

### Sybil Resistance
The √ karma weighting prevents single users from dominating:

```
Traditional: 100 accounts with 1 karma each = 100x vote power
Etherial:   100 accounts with 1 karma each = 10x vote power
                     (√100 = 10, not 100)
```

### Spam Prevention
Posting false rumors is expensive:
- Each post you create: -2.0 karma
- Each vote you get wrong: -1.5 karma
- Good votes: +1.0 karma

**Net Effect:** Creating false rumors costs reputation faster than gaining it.

### Byzantine Resilience
Voting requires:
- Minimum 5 participants (prevents small groups deciding)
- Minimum total weight ≥ 10 (prevents low-karma brigades)
- 60% supermajority for FACT (not simple majority)

---

## 🔒 Security & Privacy

### Privacy by Design
- ✨ **Email never stored** — Cleared immediately after key generation
- ✨ **No accounts database** — Identity = cryptographic keypair
- ✨ **No login tracking** — Same credentials = same identity, no server record
- ✨ **P2P encryption** — Gun.SEA provides asymmetric encryption

### Cryptographic Security
- **Gun.SEA:** Industry-standard ECDSA key generation
- **Deterministic Keys:** Same credentials always produce same keypair
- **Ed25519:** Used for message signing
- **AES-256:** For message encryption

### Sybil Resistance
See Mathematical Guarantees section above.

---

## 📦 Installation & Setup

### Prerequisites
- Node.js 18+
- pnpm (or npm/yarn)

### Step 1: Clone & Install
```bash
git clone https://github.com/aaziy/ethereal.git
cd Ethereal/etherial-rumor-verification-system
pnpm install
```

### Step 2: Configure Environment
```bash
# Copy example env file (if exists)
cp .env.example .env.local

# Or create minimal .env.local:
# NODE_ENV=development
# NEXT_PUBLIC_GUN_RELAY=http://localhost:8765/gun
```

### Step 3: Run Development Server
```bash
pnpm run dev

# This starts both:
# - Next.js frontend on http://localhost:3000
# - Gun.js relay on http://localhost:8765/gun
```

### Step 4: Open Browser
```
http://localhost:3000
```

**[→ Full Setup Guide](./etherial-rumor-verification-system/docs/QUICKSTART.md)**

---

## 🧪 Testing

### Run Tests
```bash
pnpm test
```

### Test Coverage
- Auth modal authentication flow
- Rumor card voting mechanism
- Resolution engine logic
- Reputation calculation
- Opposition system

### Manual Testing
Follow the test scenarios in `docs/TESTING.md`

---

## 🏛️ System Components Explained

### 1. **Authentication Service** (`lib/auth-service.ts`)
```typescript
// Blind authentication: Email + passphrase → deterministic keypair
const { pub, priv } = await Gun.SEA.pair(...)
// pub = public key (visible identity)
// priv = private key (never shared)
// Email is NOT stored anywhere
```

### 2. **Rumor Engine** (`lib/rumor-engine.ts`)
```typescript
// Core resolution algorithm
const resolution = calculateResolution({
  upvotes: W_true,      // Weight of upvotes
  downvotes: W_false,   // Weight of downvotes
  ratio: W_true / (W_true + W_false)
})
// Returns: FACT, FALSE, UNVERIFIED, or LOCKED
```

### 3. **Reputation Logic** (`lib/reputation-logic.ts`)
```typescript
// Karma calculation based on voting outcomes
updateKarma({
  votedCorrectly: +1.0,
  votedIncorrectly: -1.5,
  postedFalse: -2.0
})
```

### 4. **Ghost System** (`lib/ghost-system.ts`)
```typescript
// Soft deletion with cascade
ghostDelete(rumorId) {
  // Mark rumor as deleted (don't remove)
  // Recalculate dependent rumors
  // Hide from feed
}
```

---

## 📚 Documentation

| Document | Purpose | Read Time |
|----------|---------|-----------|
| [QUICKSTART.md](./etherial-rumor-verification-system/docs/QUICKSTART.md) | Get running in 2 minutes | 2 min |
| [ETHERIAL.md](./etherial-rumor-verification-system/docs/ETHERIAL.md) | Complete feature guide | 20 min |
| [ARCHITECTURE.md](./etherial-rumor-verification-system/docs/ARCHITECTURE.md) | System design & flows | 15 min |
| [IMPLEMENTATION_SUMMARY.md](./etherial-rumor-verification-system/docs/IMPLEMENTATION_SUMMARY.md) | Code walkthrough | 25 min |
| [TESTING.md](./etherial-rumor-verification-system/docs/TESTING.md) | Test cases | 10 min |
| [fada-ethereal.md](./fada-ethereal.md) | Academic deep-dive | 30 min |

---

## 🤝 Contributing

Etherial is open-source! Contributions welcome:

1. **Fork** the repository
2. **Create feature branch:** `git checkout -b feature/your-feature`
3. **Make changes** with tests
4. **Commit:** `git commit -m "feat: your feature"`
5. **Push:** `git push origin feature/your-feature`
6. **Create Pull Request**

### Development Guidelines
- TypeScript for type safety
- Jest for testing
- Follow component structure in `components/`
- Document changes in commit messages

---

## 📄 License

MIT License - See [LICENSE](./LICENSE) file for details

This project is open source and freely available for educational and commercial use.

---

## 🎓 Academic Background

Etherial implements concepts from:
- **Byzantine Fault Tolerance** — Consensus without trusting any single party
- **Reputation Systems** — Game-theoretic incentive design
- **Cryptographic Commitments** — Zero-knowledge proof concepts
- **P2P Networks** — Decentralized replication and synchronization

See `fada-ethereal.md` for full academic treatment.

---

## 📞 Support & Questions

- **Documentation:** [Check docs folder](./etherial-rumor-verification-system/docs/)
- **Issues:** [GitHub Issues](https://github.com/aaziy/ethereal/issues)
- **Feature Requests:** [GitHub Discussions](https://github.com/aaziy/ethereal/discussions)

---

## 🌟 Acknowledgments

- **Next.js team** for the amazing framework
- **Gun.js community** for decentralized database
- **Shadcn UI** for beautiful components
- **Campus communities** for inspiring this project

---

## 🚀 What's Next?

**Planned Features:**
- [ ] Mobile app (React Native)
- [ ] Advanced filtering & search
- [ ] Reputation badges & achievements
- [ ] Integration with campus newsfeeds
- [ ] Multi-language support
- [ ] Offline-first sync improvements

---

**Built with ❤️ by students, for students.**

*Transform how your campus community verifies truth.*

