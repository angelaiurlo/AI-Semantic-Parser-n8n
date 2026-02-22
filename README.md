# AI Semantic Parser and Automated Inbox Triage

## 📉 The Business Problem: The "info@" Black Hole
Operations and Customer Care teams waste thousands of hours manually reading generic inbound emails, trying to understand the customer's intent, assessing urgency, and copy-pasting data (names, order IDs, issues) into ticketing systems or CRMs. 
Using expensive human intelligence to act as a basic "text-to-data converter" is a massive operational bottleneck. It doesn't scale: if email volume doubles, the company is forced to hire more data-entry clerks.

## 🏗️ The Architectural Solution: LLM as a Semantic Parser
This repository demonstrates how to use Artificial Intelligence not as a chatbot, but as a rigid **architectural component** within an n8n workflow. 

By passing unstructured text (emails) through an LLM prompted strictly for data extraction, we transform the chaos of human language into a clean, predictable **JSON object**. 

Once the data is structured, standard logical routers (Switch nodes) take over, executing conditional routing based on the AI's semantic understanding of the text.

### Core Benefits:
* **Infinite Scalability:** The system processes 10 emails or 10,000 emails with the exact same speed and accuracy. No bottlenecks.
* **Zero Manual Data Entry:** CRM records and Support Tickets are created instantly and fully populated without a single keystroke from the team.
* **Automated Escalation:** High-urgency emails (e.g., furious clients, critical bugs) are semantically identified and routed to management instantly, bypassing the standard queue.

---

## 🗺️ Workflow Logic

### 1. Ingestion (Email/Webhook)
* Monitors inbound channels for unstructured text data.

### 2. Semantic Parsing (LLM Node)
* Uses a Large Language Model (e.g., OpenAI GPT-4o) with a highly constrained system prompt. 
* It reads the email and is instructed to output *only* a valid JSON schema containing extracted entities: `CustomerName`, `OrderID`, `Intent` (Support/Sales/Complaint), and `UrgencyScore` (1-5).

### 3. Data Structuring & Routing (Switch)
* Parses the LLM's string output into a native JSON object.
* Evaluates the `Intent` and `UrgencyScore`.
* **Fast-Track Lane:** If Urgency is high or Intent is "Complaint", the flow is routed to immediate escalation.
* **Standard Lane:** General inquiries are routed to the standard CRM queue.

### 4. System Execution
* **CRM Upsert:** Automatically creates a structured ticket/deal in the company database (e.g., Zendesk, HubSpot, Notion).
* **Alerting:** Pings the relevant department's Slack/Teams channel based on the AI's categorization.

---

## ⚙️ How to Implement
1. Import `ai-semantic-parser.json` into your n8n instance.
2. *Security Note:* Update the credentials using dummy/safe tokens before exporting your own versions.
3. Configure the Email Read node with your testing inbox, or use the Webhook node to send POST requests containing dummy emails.
4. Add your OpenAI/Anthropic API key to the LLM node.
5. Review the Prompt inside the LLM node. Adjust the requested JSON schema keys to match your specific CRM fields.
6. Connect the output branches of the Switch node to your preferred database or ticketing system.
