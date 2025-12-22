Bus Transport Booking Assistant (n8n Workflow)
📌 Overview
This repository contains an end-to-end AI-powered transport booking automation built using n8n, LangChain, Supabase, Stripe, and OpenAI.
The system acts as a virtual ticket assistant for Bus Transport Company, enabling users to:
•	Ask questions about routes, fares, timings, and classes
•	Book bus tickets via chat
•	Generate secure Stripe payment links
•	Verify Stripe webhook signatures
•	Automatically email e-tickets after payment confirmation
________________________________________
❓ Problem Statement
Traditional bus ticket booking systems suffer from several limitations:
•	Users must manually visit terminals or websites
•	No conversational interface for queries
•	Disconnected data sources (fares, routes, bookings)
•	Manual payment verification
•	No automated ticket confirmation emails
Bus Transport Company required a single automated system to:
•	Answer transport-related questions accurately
•	Collect booking details step-by-step
•	Fetch real fares dynamically
•	Handle secure online payments
•	Deliver instant e-tickets via email
________________________________________
✅ Solution Architecture
This workflow solves the problem by combining:
•	AI Chat Agent → Handles conversations & intent detection
•	Supabase Vector Store → Stores official Bus Transport Company data
•	Supabase Tables → Fetches fare prices
•	Stripe Checkout → Secure payment processing
•	Stripe Webhooks → Payment verification
•	Email Automation (Gmail) → Sends confirmed e-tickets
All components are orchestrated inside n8n as a single automated pipeline.
________________________________________
🧠 High-Level Workflow Flow
1.	User sends a chat message
2.	AI Agent classifies intent (info or booking)
3.	Required booking fields are collected
4.	Fare is fetched from Supabase
5.	Total fare is calculated
6.	Stripe payment link is generated
7.	Short payment URL is sent to user
8.	Stripe webhook verifies payment
9.	E-ticket is emailed to customer
________________________________________
🧩 Node-by-Node Documentation
🔹 Triggers
1.	When clicking ‘Execute workflow’
o	Type: Manual Trigger
o	Purpose: Testing and initializing vector data ingestion
2.	When chat message received
o	Type: Chat Trigger (Webhook)
o	Purpose: Entry point for user chat messages
o	Mode: Public webhook
🔹 Data Ingestion & AI Knowledge Base
3.	Download file
o	Type: Google Drive
o	Purpose: Downloads official Bus Transport Company transport CSV data
4.	Default Data Loader
o	Type: LangChain Document Loader
o	Purpose: Converts CSV into documents for embeddings
5.	Embeddings OpenAI
o	Purpose: Generates embeddings for Bus Transport Company data
6.	Supabase Vector Store
o	Mode: Insert
o	Purpose: Stores transport data embeddings in Supabase
7.	Supabase Vector Store1
o	Mode: Retrieve-as-Tool
o	Purpose: Used by AI Agent to answer transport queries
o	Strict Rule:
If data is not found → "Sorry, I can only answer queries related to Bus Transport Company service."
🔹 AI & Conversation Handling
8.	OpenAI Chat Model
o	Model: GPT-4.1-mini
o	Temperature: 0.2
o	Purpose: Powers the AI Agent
9.	Postgres Chat Memory
o	Purpose: Stores last 4 messages for conversation context
o	Table: faisalmovers_chat_histories
10.	AI Agent
o	Purpose: Core brain of the system
o	Responsibilities:
	Intent detection
	Field collection
	JSON-only output on confirmation
	Strict anti-hallucination rules
11.	JSON-OUTPUT
o	Type: Code Node
o	Purpose: Extracts valid JSON from AI output and assigns intent (check_info or book_ticket)
12.	Switch
o	Purpose: Routes execution based on intent:
	check_info → Information flow
	book_ticket → Booking flow
🔹 Booking & Fare Calculation
13.	fetching_fare_prices_along_cities
o	Type: Supabase
o	Purpose: Fetches fare based on source, destination, and bus class
14.	Merge
o	Purpose: Combines booking data + fare data
15.	Fare_Calculation
o	Type: Code Node
o	Purpose:
o	totalFare = fare_price_pkr × seat_quantity
🔹 Payment & URL Handling
16.	Stripe
o	Type: HTTP Request
o	Purpose: Creates Stripe checkout session
o	Currency: PKR
o	Client Reference ID: Generated dynamically
17.	Set Long URL
o	Purpose: Stores Stripe checkout URL
18.	Short.io API Call
o	Purpose: Shortens Stripe payment link
o	Domain: Aimlteam39.short.gy
19.	Set_Node
o	Purpose: Sends payment message with short link
20.	Webhook
•	Type: Webhook
•	Purpose: Receives Stripe payment events
•	Path: /webhook_bus_payment_123
•	Method: POST
🔹 Payment Verification & Ticket Delivery
21.	Code1
o	Purpose: Extracts Stripe signature headers
22.	Crypto1
o	Purpose: Generates HMAC SHA256 signature
23.	If
o	Purpose: Validates signature match, event type, and timestamp freshness
24.	Stop and Error1
o	Purpose: Stops execution if verification fails
25.	Edit Fields
o	Purpose: Extracts customer name, email, fare, booking ID
26.	Send a message1
o	Type: Gmail
o	Purpose: Sends HTML-styled e-ticket email
________________________________________
🔐 Security & Compliance
•	Stripe webhook signature verification
•	Time-based replay attack prevention
•	Strict AI anti-hallucination rules
•	No guessed fares or routes
________________________________________
🛠 Tech Stack
•	n8n
•	OpenAI (GPT-4.1-mini)
•	LangChain
•	Supabase (DB + Vector Store)
•	Stripe
•	Short.io
•	Gmail API

