# Social-to-Lead Agentic Workflow

This project demonstrates a GenAI-powered conversational agent for a fictional SaaS product.

## Features
- Intent detection (product queries vs high-intent leads)
- Retrieval Augmented Generation (RAG) from FAQ knowledge base
- Lead capture workflow

## Run
```bash
python app.py

#code
# Social-to-Lead Agentic Work

# -------------------------------
# 1. Import Libraries
# -------------------------------
import os
import json
import re
import random
import string

# -------------------------------
# 2. Knowledge Base (FAQ)
# -------------------------------
KB = [
    {
        "keywords": ["feature","workflow","automation"],
        "answer": "Our SaaS product automates workflows with AI-powered triggers."
    },
    {
        "keywords": ["support","help","issue"],
        "answer": "You can reach support via chat or email 24/7."
    },
    {
        "keywords": ["integration","api"],
        "answer": "We provide REST APIs and integrations with Slack, Salesforce, and HubSpot."
    },
    {
        "keywords": ["pricing","cost","subscription"],
        "answer": "We offer flexible subscription plans tailored to your business size."
    }
]

# -------------------------------
# 3. Intent Detection
# -------------------------------
def detect_intent(message: str) -> str:
    message = message.lower()
    if any(word in message for word in ["price","cost","subscription","demo","trial","integration"]):
        return "high_intent"
    elif any(word in message for word in ["feature","how","what","support","faq","api"]):
        return "product_query"
    else:
        return "general"

# -------------------------------
# 4. Retrieval Augmented Generation (RAG)
# -------------------------------
def retrieve_answer(query: str) -> str:
    query = query.lower()
    for item in KB:
        if any(keyword in query for keyword in item["keywords"]):
            return item["answer"]
    return "I couldn’t find an exact match, but our support team can assist further."

# -------------------------------
# 5. Lead Capture Tool
# -------------------------------
def capture_lead(message: str, session: dict) -> dict:
    # Simple placeholder: in real app, parse name/email/company
    lead = {
        "name": session.get("name","Rahul"),
        "email": session.get("email","rahul@example.com"),
        "company": session.get("company","Unknown"),
        "lead_id": ''.join(random.choices(string.ascii_uppercase + string.digits, k=6))
    }
    # Here you could push to CRM or database
    return lead

# -------------------------------
# 6. Agent Workflow
# -------------------------------
def agent_response(user_message, session):
    intent = detect_intent(user_message)

    if intent == "product_query":
        return retrieve_answer(user_message)

    elif intent == "high_intent":
        lead_info = capture_lead(user_message, session)
        return f"I’d love to connect you with our team. I’ve captured your details: {lead_info}"

    else:
        return "I’m here to help! Could you clarify your question?"

# -------------------------------
# 7. Run Agent (Console Loop)
# -------------------------------
if __name__ == "__main__":
    print("🤖 Social-to-Lead Agentic Workflow Started")
    print("Type 'quit' to exit.\n")
    session = {}
    while True:
        user_message = input("You: ")
        if user_message.lower() in ["quit","exit"]:
            print("Agent: Goodbye!")
            break
        print("Agent:", agent_response(user_message, session))

