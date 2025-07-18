# 🧩 Micro-Task & Earning Platform

A MERN-based platform connecting **Buyers** who need tasks done with **Workers** who complete them to earn virtual coins. Features include real-time task management, role-based dashboards, coin transactions, and Stripe integration.

---

## 🚀 Features

- 👤 Role-based login for Worker, Buyer, and Admin
- 📋 Task posting and completion system
- 💬 Submission approval and feedback
- 💰 Coin economy with Stripe payments
- 📤 Withdraw system for workers
- 🔔 Notification center for real-time updates
- 🛡️ JWT Authentication & Role Authorization

---

## 📁 Project Structure (Client Side Only)

```
client/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── context/
│   ├── hooks/
│   ├── layout/
│   ├── pages/
│   │   ├── auth/
│   │   ├── dashboard/
│   │   │   ├── admin/
│   │   │   ├── buyer/
│   │   │   └── worker/
│   ├── routes/
│   ├── utils/
│   ├── App.jsx
│   └── main.jsx
└── index.html
```

---

## 🔐 API Endpoints Summary

All routes are defined in a centralized `index.js` file. Below are the key categories:

### Auth
- `POST /register` – Register user
- `POST /login` – Login with credentials
- `GET /me` – Get current user info

### Tasks
- `GET /tasks` – All tasks (public)
- `POST /tasks` – Create task (Buyer)
- `PATCH /tasks/:id/assign` – Assign worker

### Submissions
- `POST /submissions` – Worker submit task
- `PATCH /submissions/:id/approve` – Approve
- `PATCH /submissions/:id/reject` – Reject

### Payments
- `POST /payments` – Save payment after Stripe
- `GET /coins` – User coin balance

### Withdrawals
- `POST /withdrawals` – Request withdrawal (Worker)
- `PATCH /withdrawals/:id/approve` – Admin approves

### Notifications
- `GET /notifications` – All notifications for user

---

## 🧭 System Workflows

Explore all user interactions in detail with visual sequence diagrams:

📄 [View Sequence Diagrams »](./Sequence.md)

---

## 🛠️ Tech Stack

- **Frontend**: React, TailwindCSS, DaisyUI, Axios, React Router, React Hook Form
- **Backend**: Node.js, Express.js, MongoDB, Mongoose
- **Auth**: Firebase, JWT
- **Payments**: Stripe API

---

## 💻 Getting Started

```bash
# Clone project
git clone https://github.com/yourusername/microtask-platform.git

# Install client dependencies
cd client
npm install

# Start dev server
npm run dev
```

---

## 👨‍💻 Contribution

Pull requests are welcome. Open issues first to discuss changes. All contributors must follow the [Code of Conduct](CODE_OF_CONDUCT.md).

---

## 📜 License

This project is licensed under the MIT License.

---

## 🗃️ Project Schema (MongoDB Collections)

### 🔹 Users
```json
{
  "_id": ObjectId,
  "name": "John Doe",
  "email": "john@example.com",
  "photoURL": "https://...",
  "role": "buyer | worker | admin",
  "coins": 100,
  "createdAt": ISODate,
  "updatedAt": ISODate
}
```

### 🔹 Tasks
```json
{
  "_id": ObjectId,
  "title": "Follow and Like Instagram Page",
  "description": "Like and follow @example",
  "platform": "Instagram",
  "total_workers": 10,
  "required_workers": 3,
  "coin_per_worker": 5,
  "buyer_email": "buyer@example.com",
  "status": "active | completed",
  "createdAt": ISODate
}
```

### 🔹 Submissions
```json
{
  "_id": ObjectId,
  "task_id": ObjectId,
  "worker_email": "worker@example.com",
  "proof": "screenshot.png",
  "status": "pending | approved | rejected",
  "submittedAt": ISODate
}
```

### 🔹 Payments
```json
{
  "_id": ObjectId,
  "buyer_email": "buyer@example.com",
  "amount": 500,
  "coins_purchased": 100,
  "transaction_id": "stripe_txn_123",
  "createdAt": ISODate
}
```

### 🔹 Withdrawals
```json
{
  "_id": ObjectId,
  "worker_email": "worker@example.com",
  "amount": 300,
  "payment_method": "bkash | nagad | bank",
  "status": "pending | approved | rejected",
  "createdAt": ISODate
}
```

### 🔹 Notifications
```json
{
  "_id": ObjectId,
  "to_email": "user@example.com",
  "message": "Your task has been approved!",
  "actionRoute": "/dashboard/submissions",
  "isRead": false,
  "createdAt": ISODate
}
```

---

