# 🚌 Bus Transport Booking Assistant (n8n Workflow)

## 📌 Overview
This repository contains an end-to-end AI-powered transport booking automation built using **n8n, LangChain, Supabase, Stripe, and OpenAI**.

The system acts as a **virtual ticket assistant for a Bus Transport Company**, enabling users to:

- Ask questions about routes, fares, timings, and classes
- Book bus tickets via chat
- Generate secure Stripe payment links
- Verify Stripe webhook signatures
- Automatically email e-tickets after payment confirmation

---

## ❓ Problem Statement
Traditional bus ticket booking systems suffer from several limitations:

- Users must manually visit terminals or websites
- No conversational interface for queries
- Disconnected data sources (fares, routes, bookings)
- Manual payment verification
- No automated ticket confirmation emails

The Bus Transport Company required a single automated system to:

- Answer transport-related questions accurately
- Collect booking details step-by-step
- Fetch real fares dynamically
- Handle secure online payments
- Deliver instant e-tickets via email

---

## ✅ Solution Architecture
This workflow solves the problem by combining:

- **AI Chat Agent** → Handles conversations & intent detection
- **Supabase Vector Store** → Stores official Bus Transport Company data
- **Supabase Tables** → Fetches fare prices
- **Stripe Checkout** → Secure payment processing
- **Stripe Webhooks** → Payment verification
- **Email Automation (Gmail)** → Sends confirmed e-tickets

All components are orchestrated inside **n8n** as a single automated pipeline.

---

## 🧠 High-Level Workflow Flow
1. User sends a chat message
2. AI Agent classifies intent (info or booking)
3. Required booking fields are collected
4. Fare is fetched from Supabase
5. Total fare is calculated
6. Stripe payment link is generated
7. Short payment URL is sent to user
8. Stripe webhook verifies payment
9. E-ticket is emailed to customer

---

## 🧩 Node-by-Node Documentation

### 🔹 Triggers

#### 1. When clicking “Execute Workflow”
- **Type:** Manual Trigger  
- **Purpose:** Testing and initializing vector data ingestion

#### 2. When chat message received
- **Type:** Chat Trigger (Webhook)
- **Purpose:** Entry point for user chat messages
- **Mode:** Public Webhook

---

### 🔹 Data Ingestion & AI Knowledge Base

#### 3. Download File
- **Type:** Google Drive
- **Purpose:** Downloads official Bus Transport Company transport CSV data

#### 4. Default Data Loader
- **Type:** LangChain Document Loader
- **Purpose:** Converts CSV into documents for embeddings

#### 5. Embeddings OpenAI
- **Purpose:** Generates embeddings for Bus Transport Company data

#### 6. Supabase Vector Store
- **Mode:** Insert
- **Purpose:** Stores transport data embeddings in Supabase

#### 7. Supabase Vector Store (Retrieve-as-Tool)
- **Mode:** Retrieve-as-Tool
- **Purpose:** Used by AI Agent to answer transport queries

**Strict Rule:**  
If data is not found →  
`"Sorry, I can only answer queries related to Bus Transport Company service."`

---

### 🔹 AI & Conversation Handling

#### 8. OpenAI Chat Model
- **Model:** GPT-4.1-mini
- **Temperature:** 0.2
- **Purpose:** Powers the AI Agent

#### 9. Postgres Chat Memory
- **Purpose:** Stores last 4 messages for conversation context
- **Table:** `faisalmovers_chat_histories`

#### 10. AI Agent
- **Purpose:** Core brain of the system

**Responsibilities:**
- Intent detection
- Field collection
- JSON-only output on confirmation
- Strict anti-hallucination rules

#### 11. JSON-OUTPUT
- **Type:** Code Node
- **Purpose:** Extracts valid JSON from AI output
- **Intents:** `check_info`, `book_ticket`

#### 12. Switch
- **Purpose:** Routes execution based on intent
  - `check_info` → Information flow
  - `book_ticket` → Booking flow

---

### 🔹 Booking & Fare Calculation

#### 13. Fetching Fare Prices Along Cities
- **Type:** Supabase
- **Purpose:** Fetches fare based on source, destination, and bus class

#### 14. Merge
- **Purpose:** Combines booking data and fare data

#### 15. Fare Calculation
- **Type:** Code Node

---

### 🔹 Payment & URL Handling

#### 16. Stripe
- **Type:** HTTP Request
- **Purpose:** Creates Stripe checkout session
- **Currency:** PKR
- **Client Reference ID:** Generated dynamically

#### 17. Set Long URL
- **Purpose:** Stores Stripe checkout URL

#### 18. Short.io API Call
- **Purpose:** Shortens Stripe payment link
- **Domain:** aimlteam39.short.gy

#### 19. Set Node
- **Purpose:** Sends payment message with short link

#### 20. Webhook
- **Purpose:** Receives Stripe payment events
- **Path:** `/webhook_bus_payment_123`
- **Method:** POST

---

### 🔹 Payment Verification & Ticket Delivery

#### 21. Code Node
- **Purpose:** Extracts Stripe signature headers

#### 22. Crypto Node
- **Purpose:** Generates HMAC SHA256 signature

#### 23. IF Node
- **Purpose:** Validates signature, event type, and timestamp freshness

#### 24. Stop and Error
- **Purpose:** Stops execution if verification fails

#### 25. Edit Fields
- **Purpose:** Extracts customer name, email, fare, and booking ID

#### 26. Send a Message
- **Type:** Gmail
- **Purpose:** Sends HTML-styled e-ticket email

---

## 🔐 Security & Compliance
- Stripe webhook signature verification
- Time-based replay attack prevention
- Strict AI anti-hallucination rules
- No guessed fares or routes

---

## 🛠 Tech Stack
- n8n
- OpenAI (GPT-4.1-mini)
- LangChain
- Supabase (Database + Vector Store)
- Stripe
- Short.io
- Gmail API


