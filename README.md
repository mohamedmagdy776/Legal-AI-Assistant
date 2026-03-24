# ⚖️ Legal AI Assistant

![Built with n8n](https://img.shields.io/badge/Built%20with-n8n-FF6B6B?style=flat-square&logo=n8n)
![Frontend](https://img.shields.io/badge/Frontend-Vibe%20Coding-6B3FA0?style=flat-square)
![AI](https://img.shields.io/badge/AI-OpenRouter%20%2B%20SerpAPI-1565C0?style=flat-square)
![Status](https://img.shields.io/badge/Status-Published-2E7D32?style=flat-square)

---

## 1. 📋 Overview

**Legal AI Assistant** is a real-world example of what becomes possible when you combine two modern AI-first approaches: **n8n** and **Vibe Coding**.

> 💡 **The idea:** Instead of building everything from scratch with traditional development, the frontend was created using **Vibe Coding** — describing the idea in natural language and letting AI generate the full UI instantly using [bolt.new](https://bolt.new). The backend logic, AI calls, web search, and data processing were all handled through an **n8n workflow**. Two powerful tools, zero traditional coding, one complete product.

This combination means:

- 💻 **Vibe Coding** handled the frontend — a beautiful, responsive web app generated from a simple description, published and live instantly via [bolt.new](https://bolt.new)
- ⚙️ **n8n** handled the backend — routing webhook requests, calling AI models, running web searches, and returning structured legal answers
- 🤖 **OpenRouter + SerpAPI** handled the intelligence — understanding legal questions by country and searching for up-to-date official sources

The result is a fully functional, production-ready legal assistant built in a fraction of the time it would take with conventional development.

**Legal AI Assistant** allows users to:

- 🌍 **Select their country** for jurisdiction-aware legal answers
- ✍️ **Type any legal question** in natural language
- 📖 **Receive answers in Arabic** with cited sources from official government websites
- 🔗 **Get direct source links** so they can read further and verify the information

The app returns a detailed legal explanation — including relevant laws, rights, and references — delivered as plain flowing Arabic text with no jargon or markdown clutter.

---

## 2. 🔧 Workflow Structure

```
┌─────────┐     ┌──────────┐     ┌────────────────┐     ┌───────────────┐     ┌─────────┐     ┌──────────────────┐
│         │     │          │     │                │     │               │     │         │     │                  │
│ Webhook │────▶│ AI Agent │────▶│ OpenRouter LLM │     │ Simple Memory │     │ SerpAPI │     │ Respond to       │
│         │     │          │     │                │     │               │     │ Search  │     │ Webhook          │
└─────────┘     └────┬─────┘     └────────────────┘     └───────────────┘     └─────────┘     └──────────────────┘
                     │                    ▲ Model                ▲ Memory            ▲ Tool           ▲
                     │                    └────────────────────────────────────────────────────────────┘
                     │
                     ▼
             ┌───────────────────┐
             │ Structured Output │
             │     Parser        │
             └───────────────────┘
```

| Node | Role |
|------|------|
| 🔗 **Webhook** | Entry point — receives POST requests from the frontend with `legalQuestion` and `country` |
| 🤖 **AI Agent** | Core reasoning engine — processes the question, calls tools, and enforces output format |
| 🧠 **OpenRouter Chat Model** | LLM provider powering the AI Agent (supports GPT-4, Claude, Gemini, and more) |
| 💾 **Simple Memory** | Maintains session context to support follow-up questions within a conversation |
| 🔍 **SerpAPI** | Real-time Google Search tool used by the agent to retrieve current legal sources |
| 📋 **Structured Output Parser** | Enforces a strict JSON schema `{ "answer": "..." }` on the agent's response |
| 📤 **Respond to Webhook** | Returns the final parsed JSON answer back to the frontend |

---

## 3. 🎯 Use Case

The Legal AI Assistant is designed for individuals who need quick, accessible legal information without the cost or wait time of consulting a lawyer. Specific scenarios include:

- A citizen in Egypt wanting to know tenant rights under Egyptian law
- A business owner asking about contract enforcement rules in their country
- A user inquiring about family law, labor law, or consumer rights
- Anyone seeking government-sourced legal references in Arabic

---

## 4. ⚙️ How It Works

### Step 1 — User Submits a Question
The user visits the web app built with Bolt.new, selects their country from a dropdown, and types their legal question. Clicking **"Submit Question"** sends a POST request to the n8n Webhook with two fields: `legalQuestion` and `country`.

### Step 2 — Webhook Receives the Request
The Webhook node captures the incoming request and forwards the data to the AI Agent for processing.

### Step 3 — AI Agent Processes the Query
The AI Agent is given a detailed system prompt instructing it to:
- Act as a legal AI assistant
- Prefer **official government websites** as sources
- Respond entirely in **Arabic**
- Provide source URLs on a new line starting with the word `Source`
- Return the answer in a strict JSON format with a single `"answer"` key

The user message sent to the agent is:
```
My Question: {legalQuestion}
My Country: {country}
Search The Web and Give me an answer to my question
```

### Step 4 — Web Search via SerpAPI
The AI Agent uses SerpAPI as a Tool to perform real-time web searches, ensuring the answer references up-to-date and authoritative legal sources rather than relying solely on training data.

### Step 5 — Structured Output Parsing
The Structured Output Parser enforces that the final response conforms to a JSON schema with a single `"answer"` field — plain flowing Arabic text with no markdown, asterisks, or newline characters.

### Step 6 — Response Sent to Frontend
The **Respond to Webhook** node returns the parsed JSON to the web application, which displays the legal answer cleanly to the user.

---

## 5. 🛠️ Setup

### Prerequisites
- n8n instance (self-hosted or cloud)
- OpenRouter API key (for LLM access)
- SerpAPI key (for web search)
- A frontend app or HTTP client to send POST requests

### Installation Steps

1. Import the workflow JSON into your n8n instance
2. Configure credentials:
   - Add your **OpenRouter API key** under the OpenRouter Chat Model node
   - Add your **SerpAPI key** under the SerpAPI node
3. Activate the workflow and copy the **Webhook URL**
4. Point your frontend to the Webhook URL with POST requests

### Request Format

```json
POST /webhook/{id}
Content-Type: application/json

{
  "legalQuestion": "What are my rights as a tenant?",
  "country": "Egypt"
}
```

### Response Format

```json
{
  "answer": "وفقاً للقانون المصري، يحق للمستأجر... المصدر: https://..."
}
```

---

## 6. ✅ Benefits

- 🌍 **Jurisdiction-aware answers** tailored to the user's selected country
- 🔍 **Real-time web search** ensures up-to-date legal information from official sources
- 🇦🇪 **Arabic language output** makes the tool accessible to Arabic-speaking users across the region
- 📦 **Structured JSON responses** enable clean, reliable frontend rendering without extra parsing
- 🧠 **Session memory** supports multi-turn legal conversations
- 🔗 **Official source citations** build trust and allow users to verify answers independently
- ⚡ **No-code / low-code pipeline** — fully built in n8n and Bolt.new, easy to extend or customize

---

## 7. 🧰 Tools & Technologies

| Category | Tool | Purpose |
|----------|------|---------|
| Workflow & Automation | [n8n](https://n8n.io) | Backend pipeline hosting the entire workflow |
| LLM Provider | [OpenRouter](https://openrouter.ai) | Unified API for GPT-4, Claude, Gemini, and more |
| AI Agent | n8n AI Agent | Orchestrates tool use, memory, prompts, and output |
| Search | [SerpAPI](https://serpapi.com) | Real-time Google Search for legal source retrieval |
| Output Handling | Structured Output Parser | Enforces JSON schema compliance on agent responses |
| Frontend | [Bolt.new](https://bolt.new) | AI-powered Vibe Coding tool for the web interface |

---

## 👤 Author

**Eng Mohamed Magdy Elghandour**

🤖 AI Automation Engineer
