# WhatsApp AI Agent using Google Gemini

**Description:**  
This workflow enables **AI-powered WhatsApp conversations** using **n8n**, **Google Gemini**, and the **WhatsApp Business API**.  
It listens for incoming WhatsApp messages, sends them to a Google Gemini language model for intelligent processing, and replies back automatically with a smart, contextual response.
<img width="1215" height="562" alt="image" src="https://github.com/user-attachments/assets/1e8eb26a-a626-4c3e-b181-e8cd3f70cb2f" />

---

## 🧠 Workflow Overview

**Workflow Name:** `Whatsapp_Agent`  
**Platform:** [n8n.io](https://n8n.io)  
**Messaging API:** [WhatsApp Business Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api)  
**AI Model:** [Google Gemini (PaLM)](https://ai.google.dev/)  

This workflow acts as a **chat automation bridge** between WhatsApp and Gemini AI — allowing users to chat naturally while receiving intelligent, dynamic responses powered by generative AI.

---

## ⚙️ Key Features

- 🤖 **AI-Powered Chat:** Generates natural, context-aware responses using Gemini AI.  
- 💬 **Real-time WhatsApp Integration:** Triggers instantly when a new message is received.  
- 🔄 **Two-Way Communication:** Receives and sends messages seamlessly.  
- ⚙️ **Low Code:** Built entirely with n8n’s visual automation platform.  
- 🧩 **Extensible:** Can be connected to databases, CRMs, or APIs for business workflows.  

---

## 🧩 Workflow Steps (Simplified)

1. **WhatsApp Trigger**  
   - Detects incoming messages via webhook.  
   - Uses WhatsApp Business API credentials to authenticate.

2. **AI Agent (LangChain Node)**  
   - Processes the received message as a prompt.  
   - Routes it to Google Gemini Chat Model for understanding and reply generation.

3. **Google Gemini Chat Model**  
   - Generates human-like responses using Google’s advanced generative AI.

4. **Send Message Node**  
   - Sends the AI-generated response back to the user on WhatsApp.

---

## 🧰 Prerequisites

| Requirement | Description |
|--------------|-------------|
| n8n Account | [https://n8n.io](https://n8n.io) |
| WhatsApp Business Cloud API | [https://developers.facebook.com/docs/whatsapp/cloud-api](https://developers.facebook.com/docs/whatsapp/cloud-api) |
| Google AI API Key | Required for Gemini model |
| Meta Developer Account | For setting up the WhatsApp webhook |
| Verified Phone Number | Linked to your WhatsApp Business account |

---

## 🔐 Environment Variables

| Variable | Description |
|-----------|--------------|
| `WHATSAPP_PHONE_NUMBER_ID` | ID from your WhatsApp Cloud API |
| `WHATSAPP_ACCESS_TOKEN` | Meta access token for sending/receiving messages |
| `GOOGLE_API_KEY` | Key for Gemini model |
| `N8N_WEBHOOK_URL` | Public webhook endpoint for message trigger |

---

## 🚀 Usage

1. Clone the repository:  
   ```bash
   git clone https://github.com/mudassar-awan/n8n_workflows.git
