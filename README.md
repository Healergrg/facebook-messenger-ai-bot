# Facebook Messenger AI Agent (n8n + OpenRouter)

An automated, context-aware AI chatbot for Facebook Messenger built with **n8n** and **OpenRouter (GPT-4o-mini)**.

Designed for personal Facebook Pages, it chats naturally with friends in **Romanized Nepali**, **Devanagari script**, and **English**, remembers chat history across conversations, and monitors for emergency or urgent messages.

---

## 🚀 Features

- **Context Memory:** Remembers past conversation context using LangChain `Window Buffer Memory` keyed to each friend's Messenger ID.
- **Multilingual & Casual Slang:** Naturally matches casual Romanized Nepali slang (*"k cha bro"*, *"kasto cha"*), Devanagari (*"कस्तो छ"*, *"के गर्दै छौ"*), or casual English.
- **Emergency Detection:** AI classifies incoming messages for urgency (accidents, hospital, family emergency, urgent money).
- **Self-Hosted & Private:** Runs locally inside Docker on your own machine.
- **Zero Ban Risk:** Connects via Meta's official Messenger Platform Graph API & Webhooks.

---

## 🛠️ Architecture

```
Facebook Messenger ➔ Meta Webhook ➔ Cloudflare Tunnel ➔ n8n
                                                          │
                   ┌──────────────────────────────────────┴──────────────────────────────────────┐
                   ▼                                                                             ▼
        Window Buffer Memory (Session Key: sender_id)                                OpenRouter (GPT-4o-mini)
                   │                                                                             │
                   └──────────────────────────────────────┬──────────────────────────────────────┘
                                                          ▼
                                             Check: Is Emergency?
                                          ┌───────┴───────┐
                                      (True)           (False)
                                         ▼                ▼
                                   Emergency Alert   Send Reply to Messenger
                                   (Twilio / SMS)    (Meta Graph API)
```

---

## 📦 Getting Started

### 1. Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) (for local webhook tunneling)
- Meta Developer Account ([developers.facebook.com](https://developers.facebook.com/))
- OpenRouter API Key ([openrouter.ai](https://openrouter.ai/))

### 2. Run n8n in Docker
```bash
docker run -d --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

### 3. Start Cloudflare Tunnel
```bash
cloudflared tunnel --url http://localhost:5678
```

### 4. Import Workflow
1. Open n8n at `http://localhost:5678`.
2. Click **...** (top right) ➔ **Import from File**.
3. Select `facebook_messenger_ai_workflow.json`.
4. Connect your **OpenRouter API Key** in the OpenRouter Chat Model node.
5. Paste your Meta **Page Access Token** in the `Send Reply to Messenger` node.
6. Toggle **Publish** to make it active!

---

## 📄 License
MIT License. Feel free to use and customize for your own personal projects!
