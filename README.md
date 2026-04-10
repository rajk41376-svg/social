<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Social-to-Lead Agentic Workflow</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f9;
      margin: 0;
      padding: 0;
    }
    #chatbox {
      width: 60%;
      margin: 40px auto;
      background: #fff;
      border: 1px solid #ccc;
      border-radius: 8px;
      padding: 20px;
    }
    .message {
      margin: 10px 0;
    }
    .user {
      text-align: right;
      color: #333;
    }
    .agent {
      text-align: left;
      color: #0078d7;
    }
    #inputArea {
      display: flex;
      margin-top: 20px;
    }
    #userInput {
      flex: 1;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    #sendBtn {
      padding: 10px 20px;
      margin-left: 10px;
      background: #0078d7;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    #sendBtn:hover {
      background: #005fa3;
    }
  </style>
</head>
<body>
  <div id="chatbox">
    <h2>🤖 Social-to-Lead Agentic Workflow</h2>
    <div id="conversation"></div>
    <div id="inputArea">
      <input type="text" id="userInput" placeholder="Type your message...">
      <button id="sendBtn">Send</button>
    </div>
  </div>

  <script>
    // Knowledge Base (FAQ)
    const KB = [
      { keywords: ["feature","workflow","automation"], answer: "Our SaaS product automates workflows with AI-powered triggers." },
      { keywords: ["support","help","issue"], answer: "You can reach support via chat or email 24/7." },
      { keywords: ["integration","api"], answer: "We provide REST APIs and integrations with Slack, Salesforce, and HubSpot." },
      { keywords: ["pricing","cost","subscription"], answer: "We offer flexible subscription plans tailored to your business size." }
    ];

    // Intent Detection
    function detectIntent(message) {
      const msg = message.toLowerCase();
      if (["price","cost","subscription","demo","trial","integration"].some(word => msg.includes(word))) {
        return "high_intent";
      } else if (["feature","how","what","support","faq","api"].some(word => msg.includes(word))) {
        return "product_query";
      } else {
        return "general";
      }
    }

    // RAG Retrieval
    function retrieveAnswer(query) {
      const msg = query.toLowerCase();
      for (let item of KB) {
        if (item.keywords.some(keyword => msg.includes(keyword))) {
          return item.answer;
        }
      }
      return "I couldn’t find an exact match, but our support team can assist further.";
    }

    // Lead Capture
    function captureLead(session) {
      const leadId = Math.random().toString(36).substring(2,8).toUpperCase();
      return {
        name: session.name || "Rahul",
        email: session.email || "rahul@example.com",
        company: session.company || "Unknown",
        lead_id: leadId
      };
    }

    // Agent Response
    function agentResponse(userMessage, session) {
      const intent = detectIntent(userMessage);
      if (intent === "product_query") {
        return retrieveAnswer(userMessage);
      } else if (intent === "high_intent") {
        const leadInfo = captureLead(session);
        return `I’d love to connect you with our team. I’ve captured your details: ${JSON.stringify(leadInfo)}`;
      } else {
        return "I’m here to help! Could you clarify your question?";
      }
    }

    // Chat UI
    const conversation = document.getElementById("conversation");
    const userInput = document.getElementById("userInput");
    const sendBtn = document.getElementById("sendBtn");
    const session = {};

    function addMessage(text, sender) {
      const div = document.createElement("div");
      div.className = "message " + sender;
      div.textContent = (sender === "user" ? "You: " : "Agent: ") + text;
      conversation.appendChild(div);
      conversation.scrollTop = conversation.scrollHeight;
    }

    sendBtn.addEventListener("click", () => {
      const msg = userInput.value.trim();
      if (!msg) return;
      addMessage(msg, "user");
      const response = agentResponse(msg, session);
      addMessage(response, "agent");
      userInput.value = "";
    });

    userInput.addEventListener("keypress", (e) => {
      if (e.key === "Enter") sendBtn.click();
    });
  </script>
</body>
</html>
