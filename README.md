# jarvis-ai
My Personal JARVIS AI Assistant

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JARVIS AI</title>
<style>
* { box-sizing: border-box; }
body {
  margin: 0;
  min-height: 100vh;
  background: radial-gradient(circle, #123b58, #020711 75%);
  color: #e5faff;
  font-family: Arial, sans-serif;
  display: grid;
  place-items: center;
  padding: 20px;
}
.app { width: 100%; max-width: 700px; text-align: center; }
h1 { color: #42d9ff; letter-spacing: 8px; }
.status { color: #73dfff; letter-spacing: 3px; font-size: 13px; }
.orb {
  width: 160px; height: 160px; border-radius: 50%;
  border: 2px solid #42d9ff;
  box-shadow: 0 0 30px #168bbd, inset 0 0 30px #168bbd;
  display: grid; place-items: center;
  font-size: 55px; color: #8cecff;
  margin: 30px auto;
}
.message {
  background: #071827; border: 1px solid #176080;
  border-radius: 12px; padding: 20px;
  min-height: 110px; text-align: left;
  line-height: 1.7; margin: 20px 0;
  overflow-wrap: anywhere;
}
.controls { display: flex; gap: 10px; }
input {
  flex: 1; min-width: 0; padding: 14px;
  border-radius: 8px; border: 1px solid #2585a8;
  background: #071827; color: white; font-size: 16px;
}
button {
  padding: 13px 18px; border: 0; border-radius: 8px;
  background: #13a9d2; color: #00131d;
  font-weight: bold; cursor: pointer; margin: 5px;
}
button:hover { background: #69e4ff; }
small { display: block; margin-top: 25px; color: #8caebe; }
@media(max-width: 500px) {
  .controls { flex-wrap: wrap; }
  input { flex-basis: 100%; }
}
</style>
</head>
<body>
<main class="app">
  <h1>J.A.R.V.I.S</h1>
  <div class="status">PERSONAL AI ASSISTANT</div>
  <div class="orb">✦</div>
  <div class="status" id="status">SYSTEM ONLINE</div>

  <div class="message" id="message">
    <strong>JARVIS:</strong> Hello! Your JARVIS interface is ready.
    How can I help you?
  </div>

  <div class="controls">
    <input id="userInput" placeholder="Type your message...">
    <button onclick="sendMessage()">SEND</button>
  </div>

  <button onclick="startVoice()">🎙 SPEAK</button>
  <button onclick="speakReply()">🔊 READ ALOUD</button>

  <small>JARVIS AI • VERSION 1.0</small>
</main>

<script>
function sendMessage() {
  const input = document.getElementById("userInput");
  const question = input.value.trim();
  if (!question) return;

  const message = document.getElementById("message");
  const you = document.createElement("strong");
  you.textContent = "YOU: ";
  const userText = document.createElement("div");
  userText.textContent = question;

  const jarvis = document.createElement("div");
  jarvis.innerHTML = "<strong>JARVIS:</strong> ";
  jarvis.appendChild(document.createTextNode(
    "I received your message! My AI brain is not connected yet."
  ));

  message.replaceChildren(you, userText, document.createElement("br"), jarvis);
  input.value = "";
}

function speakReply() {
  if (!("speechSynthesis" in window)) {
    alert("Speech is not supported in this browser.");
    return;
  }
  speechSynthesis.cancel();
  const text = document.getElementById("message").innerText;
  speechSynthesis.speak(new SpeechSynthesisUtterance(text));
}

function startVoice() {
  const Recognition = window.SpeechRecognition ||
                      window.webkitSpeechRecognition;
  if (!Recognition) {
    alert("Voice recognition is not supported. Try Chrome.");
    return;
  }

  const recognition = new Recognition();
  recognition.lang = "en-IN";
  recognition.onstart = () => {
    document.getElementById("status").textContent = "LISTENING...";
  };
  recognition.onresult = (event) => {
    document.getElementById("userInput").value =
      event.results[0][0].transcript;
    sendMessage();
  };
  recognition.onerror = () => {
    document.getElementById("status").textContent = "VOICE ERROR";
  };
  recognition.onend = () => {
    document.getElementById("status").textContent = "SYSTEM ONLINE";
  };
  recognition.start();
}
</script>
</body>
</html>
