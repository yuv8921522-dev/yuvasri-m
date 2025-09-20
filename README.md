# yuvasri-m
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Personal Finance Chatbot</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary-color: #2e8b57;
      --secondary-color: #6a5acd;
      --background-color: #f0f2f5;
      --card-background: rgba(255, 255, 255, 0.15);
      --border-color: rgba(255, 255, 255, 0.3);
      --text-color: #333;
      --message-bg-bot: linear-gradient(145deg, #a7e9af, #68b8a5);
      --message-bg-user: linear-gradient(145deg, #a5b7e9, #6c8aef);
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      padding: 0;
      font-family: 'Montserrat', sans-serif;
      background: linear-gradient(135deg, #e22b90, #cd5abe);
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
      color: var(--text-color);
    }

    .chat-container {
      background: var(--card-background);
      backdrop-filter: blur(10px);
      border: 1px solid var(--border-color);
      border-radius: 20px;
      width: 400px;
      height: 600px;
      max-width: 90vw;
      display: flex;
      flex-direction: column;
      overflow: hidden;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    }

    .chat-header {
      background: rgba(255, 255, 255, 0.1);
      color: white;
      padding: 1.2rem;
      font-size: 1.4rem;
      font-weight: bold;
      text-align: center;
      border-bottom: 1px solid var(--border-color);
    }

    .chat-body {
      flex: 1;
      padding: 1rem;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }

    .message {
      max-width: 80%;
      padding: 0.9rem 1.2rem;
      border-radius: 20px;
      font-size: 0.95rem;
      line-height: 1.4;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.08);
      animation: fadeIn 0.4s ease-in-out;
    }

    .bot-message {
      background: var(--message-bg-bot);
      align-self: flex-start;
      border-top-left-radius: 4px;
    }

    .user-message {
      background: var(--message-bg-user);
      align-self: flex-end;
      border-top-right-radius: 4px;
    }

    .chat-footer {
      display: flex;
      padding: 1rem;
      border-top: 1px solid var(--border-color);
      background: rgba(255, 255, 255, 0.1);
      gap: 0.6rem;
    }

    .chat-footer input {
      flex: 1;
      padding: 0.8rem 1.1rem;
      border-radius: 25px;
      border: none;
      font-size: 1rem;
      background: rgba(255, 255, 255, 0.6);
      outline: none;
    }

    .chat-footer button {
      width: 48px;
      height: 48px;
      background: white;
      border: none;
      border-radius: 50%;
      color: var(--secondary-color);
      font-size: 1.5rem;
      cursor: pointer;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      display: flex;
      align-items: center;
      justify-content: center;
      transition: transform 0.2s;
    }

    .chat-footer button:hover {
      transform: scale(1.1);
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 450px) {
      body {
        align-items: flex-start;
        padding-top: 20px;
      }

      .chat-container {
        width: 100%;
        height: 100vh;
        border-radius: 0;
      }

      .chat-footer button {
        width: 45px;
        height: 45px;
      }
    }
  </style>
</head>
<body>
  <div class="chat-container">
    <div class="chat-header">💸 Personal Finance Assistant</div>
    <div class="chat-body" id="chat-body">
      <div class="message bot-message">
        👋 Hi! I'm your personal finance assistant.<br>
        Ask me anything about budgeting, saving, or expense tracking!
      </div>
    </div>
    <div class="chat-footer">
      <input type="text" id="user-input" placeholder="Type your message..." />
      <button id="send-btn" title="Send">
        <svg xmlns="http://www.w3.org/2000/svg" height="24" width="24" fill="currentColor">
          <path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/>
        </svg>
      </button>
    </div>
  </div>

  <script>
    const chatBody = document.getElementById('chat-body');
    const userInput = document.getElementById('user-input');
    const sendBtn = document.getElementById('send-btn');

    const botResponses = [
      "📊 Use the 50/30/20 rule: 50% needs, 30% wants, 20% savings.",
      "💰 Automate your savings to build wealth effortlessly.",
      "📈 Track expenses weekly to spot bad spending habits.",
      "🧾 Want to eliminate debt? Focus on smallest balance first (snowball method)!",
      "💡 Invest in index funds for low-risk long-term growth."
    ];

    function appendMessage(text, sender = 'user') {
      const msg = document.createElement('div');
      msg.className = `message ${sender}-message`;
      msg.innerHTML = text;
      chatBody.appendChild(msg);
      chatBody.scrollTop = chatBody.scrollHeight;
    }

    function handleSend() {
      const text = userInput.value.trim();
      if (!text) return;

      appendMessage(text, 'user');
      userInput.value = '';

      // Simulate typing indicator
      const typing = document.createElement('div');
      typing.className = 'message bot-message';
      typing.textContent = '🤖 Typing...';
      chatBody.appendChild(typing);
      chatBody.scrollTop = chatBody.scrollHeight;

      setTimeout(() => {
        typing.remove();
        const reply = botResponses[Math.floor(Math.random() * botResponses.length)];
        appendMessage(`🤖 ${reply}`, 'bot');
      }, 700);
    }

    sendBtn.addEventListener('click', handleSend);
    userInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') handleSend();
    });
  </script>
</body>
</html>




