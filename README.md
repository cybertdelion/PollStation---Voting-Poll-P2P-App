# PollStation 🗳️

> A decentralized, peer-to-peer polling and voting app built on top of [Intercom](https://github.com/Trac-Systems/intercom) — the Trac Network agent stack.

PollStation lets any peer create a poll and broadcast it over Intercom sidechannels. Other peers join, cast their vote, and results are tallied in real-time across all connected nodes — no server, no central authority.

---

## ✨ Features

- **Create Polls** — Any peer can open a poll with up to 6 choices
- **P2P Vote Propagation** — Votes travel over Intercom sidechannels to all connected peers
- **Real-time Tally** — Results update live as votes arrive
- **Tamper-resistant** — One vote per peer public key, enforced by contract state
- **Lightweight** — Runs on Pear runtime, no backend needed

---

## 🚀 Quick Start

```bash
# 1. Install Pear runtime (required — do NOT use plain node)
npm i -g pear

# 2. Clone this fork
git clone https://github.com/YOUR_USERNAME/intercom
cd intercom

# 3. Install dependencies
npm install

# 4. Run as admin (first peer / poll creator)
pear run . --admin

# 5. Run as participant (voter) on another terminal/machine
pear run . --join <invite-key>
```

---

## 🖥️ App Demo

### Creating a Poll
![Create Poll Screenshot](./screenshots/create-poll.png)

### Voting Interface
![Vote Screenshot](./screenshots/vote.png)

### Live Results
![Results Screenshot](./screenshots/results.png)

---

## 🔧 How It Works

1. The **admin peer** opens a Hypercore-backed contract via Intercom and broadcasts a poll object `{ question, choices, creator_key }`.
2. **Joining peers** receive the poll via the Intercom sidechannel, render the UI, and submit a signed vote message.
3. Votes are written to the shared contract state and replicated to all peers using Autobase.
4. The tally is computed deterministically from contract state — every peer sees the same result.

---

## 📁 Skill File

For agent-oriented setup and operational instructions, see [`SKILL.md`](./SKILL.md).

---

## 🔑 Trac Address

```
trac1xwf2zfjj6cu0d3lnnycevp70n899yl973svmhs23u8c5km70wvdqhsx7zx
```

> ⚠️ Replace `trac1xwf2zfjj6cu0d3lnnycevp70n899yl973svmhs23u8c5km70wvdqhsx7zx` with your actual Trac wallet address before submitting to the awesome-intercom list.

---

## 🏗️ Built On

- [Intercom](https://github.com/Trac-Systems/intercom) by Trac Systems
- [Pear Runtime](https://docs.pears.com)
- [Hypercore Protocol](https://hypercore-protocol.org)

---

## License

MIT
