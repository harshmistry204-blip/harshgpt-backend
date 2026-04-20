<!DOCTYPE html>
<html>
<head>
  <title>HarshGPT Pro</title>

  <style>
    body {
      margin: 0;
      display: flex;
      height: 100vh;
      font-family: Arial;
      background: #343541;
      color: white;
    }

    .sidebar {
      width: 250px;
      background: #202123;
      padding: 10px;
      display: flex;
      flex-direction: column;
    }

    .new-chat {
      background: #343541;
      border: 1px solid #555;
      padding: 10px;
      cursor: pointer;
      margin-bottom: 10px;
      text-align: center;
    }

    .chat-list {
      flex: 1;
      overflow-y: auto;
    }

    .chat-item {
      padding: 8px;
      margin: 5px 0;
      background: #2a2b32;
    }

    .main {
      flex: 1;
      display: flex;
      flex-direction: column;
    }

    .chat-area {
      flex: 1;
      padding: 20px;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
    }

    .message {
      margin: 8px 0;
      max-width: 70%;
      padding: 10px;
      border-radius: 10px;
    }

    .user {
      background: #19c37d;
      align-self: flex-end;
    }

    .bot {
      background: #444654;
      align-self: flex-start;
    }

    .input-area {
      display: flex;
      padding: 10px;
      background: #40414f;
    }

    input {
      flex: 1;
      padding: 12px;
      border-radius: 5px;
      border: none;
      outline: none;
    }

    button {
      margin-left: 8px;
      padding: 10px;
      background: #19c37d;
      border: none;
      color: white;
      cursor: pointer;
    }

    #loginBox {
      position: absolute;
      top: 10px;
      right: 10px;
    }
  </style>
</head>

<body>

<!-- Login -->
<div id="loginBox">
  <button onclick="login()">Login</button>
</div>

<!-- Sidebar -->
<div class="sidebar">
  <div class="new-chat" onclick="newChat()">+ New Chat</div>
  <div class="chat-list" id="chatList"></div>
</div>

<!-- Main -->
<div class="main">
  <div class="chat-area" id="chat"></div>

  <div class="input-area">
    <input id="input" placeholder="Send a message...">
    <button onclick="startVoice()">🎤</button>
    <button onclick="send()">Send</button>
  </div>
</div>

<script>
let chat = document.getElementById("chat");
let currentChat = [];

// 🔹 Save chats
function saveChats() {
  localStorage.setItem("harshgpt", JSON.stringify(currentChat));
}

// 🔹 Load chats
function loadChats() {
  let saved = localStorage.getItem("harshgpt");
  if (saved) {
    currentChat = JSON.parse(saved);
    currentChat.forEach(msg => {
      addMessage(msg.text, msg.sender, false);
    });
  }
}

window.onload = loadChats;

// 🔹 Add message
function addMessage(text, sender, save = true) {
  let div = document.createElement("div");
  div.className = "message " + sender;
  div.textContent = text;
  chat.appendChild(div);

  if (save) {
    currentChat.push({ text, sender });
    saveChats();
  }

  chat.scrollTop = chat.scrollHeight;
}

// 🔹 New chat
function newChat() {
  chat.innerHTML = "";
  currentChat = [];
  saveChats();
}

// 🔹 Send message
async function send() {
  let input = document.getElementById("input");
  let text = input.value;
  if (!text) return;

  addMessage(text, "user");
  input.value = "";

  let typing = document.createElement("div");
  typing.className = "message bot";
  typing.textContent = "AI is typing...";
  chat.appendChild(typing);

  try {
    let res = await fetch("https://harshgpt-backend.onrender.com/chat", {
      method: "POST",
      headers: {"Content-Type": "application/json"},
      body: JSON.stringify({ message: text })
    });

    let data = await res.json();
    let reply = data.choices[0].message.content;

    typing.remove();
    addMessage(reply, "bot");

  } catch (error) {
    typing.textContent = "Error connecting to AI";
  }
}

// 🔹 Enter key
document.getElementById("input").addEventListener("keypress", e => {
  if (e.key === "Enter") send();
});

// 🎤 Voice input
function startVoice() {
  let recognition = new webkitSpeechRecognition();
  recognition.lang = "en-US";

  recognition.onresult = function(event) {
    document.getElementById("input").value =
      event.results[0][0].transcript;
  };

  recognition.start();
}

// 🔐 Simple login (demo)
function login() {
  let name = prompt("Enter your name:");
  if (name) alert("Welcome " + name);
}
</script>

</body>
</html>
