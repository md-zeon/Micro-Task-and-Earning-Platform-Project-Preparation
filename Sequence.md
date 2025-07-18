# 🔁 Micro-Task and Earning Platform – Sequence Diagrams

This document provides a clear overview of key workflows and user interactions in the Micro-Task and Earning Platform. It covers registration, task management, submissions, coin transactions, withdrawals, and admin operations through **sequence diagrams**.

---

## 🧭 Overview

The system is built using the **MERN Stack** and involves three user roles:

- **Worker** – Completes tasks and earns coins
- **Buyer** – Posts tasks and pays workers using coins
- **Admin** – Manages users, tasks, and handles withdrawal requests

The project follows a RESTful API structure, uses JWT authentication, and role-based access control.

---

## 1. 👤 User Registration (Buyer or Worker)

```mermaid
sequenceDiagram
    participant U as User
    participant C as Client App
    participant F as Firebase Auth
    participant S as Server (Node.js)
    participant DB as MongoDB

    U->>C: Submit registration form
    C->>F: Create user with email/password
    F-->>C: Return JWT token
    C->>S: POST /register with user data + token
    S->>DB: Insert user with role and initial coins
    DB-->>S: Success
    S-->>C: Registration success
    C->>C: Store JWT & redirect to /dashboard
```

---

## 2. ✍️ Buyer Adds a Task

```mermaid
sequenceDiagram
    participant B as Buyer
    participant C as Client App
    participant S as Server
    participant DB as MongoDB

    B->>C: Fill Task Form
    C->>S: POST /tasks
    S->>DB: Check if coins >= total cost
    alt Not enough coins
        S-->>C: Error "Purchase Coin"
        C->>C: Redirect to /purchase-coin
    else Sufficient coins
        DB->>DB: Save task
        DB->>DB: Deduct coins from buyer
        DB-->>S: Success
        S-->>C: Task added
    end
```

---

## 3. 🧑‍🔧 Worker Submits a Task

```mermaid
sequenceDiagram
    participant W as Worker
    participant C as Client
    participant S as Server
    participant DB as MongoDB

    W->>C: Open Task Details
    C->>S: GET /tasks/:id
    S-->>C: Return task details

    W->>C: Submit proof
    C->>S: POST /submissions
    S->>DB: Save submission with status "pending"
    DB-->>S: Saved
    S->>DB: Add notification to buyer
    S-->>C: Submission success
```

---

## 4. ✅ Buyer Reviews Submission

```mermaid
sequenceDiagram
    participant B as Buyer
    participant C as Client
    participant S as Server
    participant DB as MongoDB

    B->>C: Visit /task-review
    C->>S: GET /submissions/to-review
    S->>DB: Fetch submissions
    DB-->>S: Return data
    S-->>C: Show in table

    B->>C: Click Approve/Reject
    alt Approve
        C->>S: PATCH /submissions/:id/approve
        S->>DB: Update status to "approved"
        DB->>DB: Add coins to worker
        DB->>DB: Notify worker
    else Reject
        C->>S: PATCH /submissions/:id/reject
        DB->>DB: Update status to "rejected"
        DB->>DB: Increase required_workers
        DB->>DB: Notify worker
    end
    S-->>C: Action complete
```

---

## 5. 💰 Coin Purchase via Stripe

```mermaid
sequenceDiagram
    participant B as Buyer
    participant C as Client
    participant Stripe as Stripe API
    participant S as Server
    participant DB as MongoDB

    B->>C: Click on coin package
    C->>Stripe: Create payment intent
    Stripe-->>C: Payment success

    C->>S: POST /payments
    S->>DB: Save payment data
    DB->>DB: Add coins to user
    S-->>C: Payment confirmed
```

---

## 6. 💸 Worker Withdraws Coins

```mermaid
sequenceDiagram
    participant W as Worker
    participant C as Client
    participant S as Server
    participant DB as MongoDB

    W->>C: Fill Withdrawal Form
    C->>S: POST /withdrawals
    S->>DB: Save request with status "pending"
    DB->>DB: Notify admin
    S-->>C: Request submitted
```

---

## 7. 🛠️ Admin Approves Withdrawal

```mermaid
sequenceDiagram
    participant A as Admin
    participant C as Client
    participant S as Server
    participant DB as MongoDB

    A->>C: Click Approve
    C->>S: PATCH /withdrawals/:id/approve
    S->>DB: Update status to "approved"
    DB->>DB: Deduct coins from worker
    DB->>DB: Notify worker
    S-->>C: Approval complete
```

---

## 8. 🔔 Notifications Flow

```mermaid
sequenceDiagram
    participant SYS as System
    participant S as Server
    participant DB as MongoDB
    participant U as User
    participant C as Client

    SYS->>S: Trigger notification event
    S->>DB: Save notification (toEmail, route, time)
    
    U->>C: Click bell icon
    C->>S: GET /notifications
    S->>DB: Fetch user notifications
    DB-->>S: Return list
    S-->>C: Show popup with latest alerts
```

---

## 🧠 Tips

- Use `jwt.decode()` in frontend to extract user info and role
- Always secure API routes with middleware like `verifyJWT` + `checkRole`
- Store JWT in `localStorage` or `httpOnly` cookies (recommended)
- Add loading and error states for all async actions

---

## 🧪 Tools Recommended

- [https://mermaid.live](https://mermaid.live) – for testing Mermaid diagrams
- [Draw.io](https://draw.io) – for manual visual diagrams
- [PlantUML](https://plantuml.com/) – for UML-based diagrams

---

Happy Coding! 🚀