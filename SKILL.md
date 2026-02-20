# SKILL.md — PollStation

> Agent-oriented setup and operational guide for the **PollStation** fork of Intercom.

---

## Overview

PollStation is a P2P polling app built on Intercom (Trac Network). Agents can:
- Create and broadcast polls to all peers on a shared sidechannel
- Cast votes that are cryptographically tied to the agent's key pair
- Read live tally results from contract state

---

## Runtime Requirement

**PEAR RUNTIME ONLY.** Never use plain `node` or `npm start`.

```bash
npm i -g pear
```

---

## Setup Steps

### Step 1 — Clone and install

```bash
git clone https://github.com/YOUR_USERNAME/intercom
cd intercom
npm install
```

### Step 2 — Start admin peer (poll creator)

```bash
pear run . --admin
```

On first run, an invite key is printed to stdout. Share this key with voters.

### Step 3 — Join as voter

```bash
pear run . --join <invite-key>
```

---

## Creating a Poll (Agent Instructions)

Send a JSON message over the sidechannel in this format:

```json
{
  "type": "create_poll",
  "question": "What is the best Trac use case?",
  "choices": ["DeFi Swaps", "P2P Messaging", "Timestamping", "Prediction Markets"]
}
```

The contract will:
1. Assign the poll a UUID
2. Store it in shared state
3. Broadcast to all peers via the sidechannel

---

## Casting a Vote (Agent Instructions)

```json
{
  "type": "vote",
  "poll_id": "<uuid>",
  "choice_index": 2
}
```

Rules enforced by contract:
- One vote per peer public key per poll
- `choice_index` must be within bounds
- Votes after poll close time are rejected

---

## Reading Results (Agent Instructions)

Query contract state:

```json
{
  "type": "get_results",
  "poll_id": "<uuid>"
}
```

Response shape:

```json
{
  "question": "...",
  "choices": ["...", "..."],
  "votes": [12, 8, 21, 4],
  "total": 45,
  "status": "open"
}
```

---

## App Logic Location

| File | Purpose |
|------|---------|
| `index.js` | Entry point, Pear app bootstrap, UI rendering |
| `contract/index.js` | Autobase contract: poll state + vote logic |
| `features/poll.js` | Poll creation and vote helpers |
| `index.html` | Web UI (rendered inside Pear) |

---

## Known Limitations

- Polls are ephemeral if no peer persists the Hypercore (use `--storage` flag to persist)
- No poll close time enforcement yet (planned)
- Max 6 choices per poll

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `pear: command not found` | Run `npm i -g pear` |
| Peers can't connect | Check firewall, ensure UDP is open |
| Vote rejected | Check you haven't already voted on this poll |

---

## Agent Notes

- Always verify `poll_id` before voting — use `get_results` first to confirm poll exists
- The admin peer must stay online for new peers to receive the initial poll state
- For automation: pipe messages via stdin or use the Intercom JS API directly

---

*PollStation is a fork of [Intercom](https://github.com/Trac-Systems/intercom) by Trac Systems.*
