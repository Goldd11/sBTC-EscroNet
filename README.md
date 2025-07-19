# sBTC-EscroNet - Decentralized Escrow & Reputation Smart Contract

A trustless escrow mechanism combined with a reputation system, enabling secure transactions and user credibility tracking on the Stacks blockchain.

---

## 🚀 Features

* **Escrow Transactions**: Users can create time-stamped, pending transactions to be released later.
* **Reputation Management**: After successful transactions, recipients can rate senders, building a decentralized trust layer.
* **Access Control**: Only senders can release funds, and only recipients can give reputation points.
* **Error Handling**: Clean, enumerated error responses.
* **Validation Checks**: Ensures only valid and authorized actions are processed.

---

## 📁 Contract Structure

### Constants

| Constant                     | Description                                    |
| ---------------------------- | ---------------------------------------------- |
| `CONTRACT-OWNER`             | The contract deployer                          |
| `ERR-UNAUTHORIZED`           | Unauthorized action (err u1)                   |
| `ERR-INSUFFICIENT-FUNDS`     | Amount must be > 0 (err u2)                    |
| `ERR-INVALID-RECIPIENT`      | Recipient must not be sender or owner (err u3) |
| `ERR-TRANSACTION-NOT-FOUND`  | Missing transaction (err u4)                   |
| `ERR-INVALID-POINTS`         | Reputation points must be > 0 (err u5)         |
| `ERR-INVALID-TRANSACTION-ID` | ID is out of valid range (err u6)              |

---

## 🧠 Data Structures

### Maps

* **`transactions`**: Stores transaction data by `id`
* **`user-reputation`**: Tracks reputation for each user

### Variables

* **`next-transaction-id`**: Sequential counter for transaction IDs

---

## 🔧 Functions

### Public Functions

#### `create-transaction(recipient, amount) → (ok id) | (err code)`

Creates a pending transaction.

* Validates:

  * `recipient` is not sender or contract owner
  * `amount > 0`

#### `release-funds(transaction-id) → (ok true) | (err code)`

Sender finalizes the transaction and transfers funds to recipient.

* Validates:

  * Transaction exists
  * Caller is transaction sender

#### `add-reputation-points(transaction-id, points) → (ok true) | (err code)`

Recipient can rate the sender post-transaction.

* Validates:

  * Transaction exists
  * Caller is the recipient
  * `points > 0`

#### `get-user-reputation(user) → { total-points, total-transactions }`

Returns a user's cumulative reputation.

#### `get-transaction-details(transaction-id) → Optional<Transaction>`

Fetches a transaction by ID.

---

## 🔒 Access Control

| Function                  | Who Can Call               |
| ------------------------- | -------------------------- |
| `create-transaction`      | Any user                   |
| `release-funds`           | Sender of transaction only |
| `add-reputation-points`   | Recipient only             |
| `get-user-reputation`     | Read-only, open            |
| `get-transaction-details` | Read-only, open            |

---

## ⚙️ How It Works (Flow)

1. **User A** creates a transaction with **User B** as the recipient and amount X.
2. Funds are locked in **PENDING** state.
3. **User A** later calls `release-funds`, which sends X STX to **User B**.
4. **User B**, after receiving the funds, calls `add-reputation-points` to rate **User A**.
5. **User A**'s reputation score is incremented.

---

## 📈 Reputation System Logic

* Reputation consists of:

  * Total points received from transactions
  * Total number of transactions completed
* Useful for identifying trustworthy participants in decentralized markets.

---
