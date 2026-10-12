# talk2task-command-center-
**talk2task-ai** is a futuristic AI productivity assistant for managing habits, tasks, meetings, reminders, and goals. Built with HTML, CSS, and JavaScript, it features an interactive chatbot, habit tracking, streaks, achievements, progress charts, and voice input. Powered by Google Gemini API for intelligent, conversational assistance.
<!DOCTYPE html>
<html lang="en">
<head>
<!-- ===== STEP 1: PASTE YOUR API KEY BETWEEN THE QUOTES ON LINE 6 ===== -->
<script>
const API_KEY = "AQ.Ab8RN6LhCRL0KpnBJFzx769Cn3oFAWalmzzTkH5ZClSTrEz5SQ";
const MODEL = "gemini-3.5-flash-lite";
</script>
<script>
// ===== YOUR CHATBOT'S INSTRUCTIONS (RTCC) =====
const SYSTEM_PROMPT = `
ROLE: You are a friendly physics tutor.
TASK: Help students understand physics concepts.
CONTEXT: The student is in Class 9.
CONSTRAINTS: Use simple words. Give one real-life example every time.
OUTPUT: Short explanation, then an example, then a one-line summary.
`;
</script>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#10172a">
<title>PhysicsAI — Your Physics Tutor</title>
<style>
:root {
  color-scheme: dark;
  --bg: #0b1020;
  --panel: #111a2e;
  --panel2: #18243b;
  --text: #edf3ff;
  --muted: #94a3b8;
  --accent: #65e4c4;
  --accent2: #4f9cf9;
  --border: rgba(148,163,184,.17);
}
* { box-sizing: border-box; }
body {
  margin: 0;
  min-height: 100vh;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system,
    BlinkMacSystemFont, "Segoe UI", sans-serif;
  background:
    radial-gradient(ellipse at 10% 0%, #1a3151 0, transparent 42%),
    radial-gradient(ellipse at 95% 100%, #182849 0, transparent 38%),
    var(--bg);
  color: var(--text);
  display: flex;
  justify-content: center;
  padding: 22px;
}
button, textarea { font: inherit; }
button { cursor: pointer; }
.app {
  width: 100%;
  max-width: 1000px;
  height: min(900px, calc(100vh - 44px));
  min-height: 520px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border: 1px solid var(--border);
  border-radius: 24px;
  background: rgba(12,19,35,.88);
  box-shadow: 0 25px 90px #0006;
  backdrop-filter: blur(18px);
}
header {
  padding: 19px 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 14px;
  border-bottom: 1px solid var(--border);
  background: #111a2bce;
}
.brand { display: flex; align-items: center; gap: 13px; min-width: 0; }
.logo {
  width: 45px; height: 45px; flex-shrink: 0;
  display: grid; place-items: center;
  border-radius: 15px;
  color: #071b22;
  font-size: 24px;
  background: linear-gradient(140deg, var(--accent), #81aaff);
  box-shadow: 0 5px 22px #65e4c433;
}
.brand h1 { margin: 0; font-size: 20px; letter-spacing: -.5px; }
.brand p { margin: 4px 0 0; font-size: 12px; color: var(--muted); }
.clear {
  border: 1px solid var(--border);
  background: var(--panel2);
  color: #d5deef;
  padding: 9px 12px;
  border-radius: 10px;
  font-size: 12px;
  white-space: nowrap;
}
.clear:hover { border-color: var(--accent); color: var(--accent); }
.status {
  display: flex; align-items: center; gap: 6px;
  font-size: 11px; color: var(--accent); margin-top: 5px;
}
.status span {
  width: 6px; height: 6px; border-radius: 50%; background: var(--accent);
}
.chat {
  flex: 1;
  overflow-y: auto;
  padding: 26px clamp(14px, 4vw, 38px);
  scroll-behavior: smooth;
}
.chat::-webkit-scrollbar { width: 6px; }
.chat::-webkit-scrollbar-thumb { background: #35435b; border-radius: 10px; }
.message {
  display: flex;
  gap: 10px;
  align-items: flex-start;
  margin: 0 0 22px;
  animation: appear .22s ease-out;
}
@keyframes appear {
  from { opacity: 0; transform: translateY(6px); }
  to { opacity: 1; transform: translateY(0); }
}
.message.user { flex-direction: row-reverse; }
.avatar {
  width: 32px; height: 32px; flex-shrink: 0;
  display: grid; place-items: center;
  border-radius: 11px;
  background: #20334b;
  font-size: 15px;
}
.message.user .avatar { background: #284f62; }
.content { max-width: min(78%, 650px); min-width: 0; }
.name { font-size: 11px; color: var(--muted); margin: 0 0 7px 3px; }
.message.user .name { text-align: right; margin-right: 3px; }
.bubble {
  padding: 13px 16px;
  border: 1px solid var(--border);
  border-radius: 4px 17px 17px 17px;
  background: var(--panel);
  color: #e7edf8;
  font-size: 14px;
  line-height: 1.75;
  overflow-wrap: anywhere;
  white-space: pre-wrap;
}
.message.user .bubble {
  background: linear-gradient(135deg, #214a60, #233c60);
  border-color: #4e7893;
  border-radius: 17px 4px 17px 17px;
}
.bubble.error {
  border-color: #b94353;
  background: #401d2b;
  color: #ffd3d8;
}
.welcome-title {
  font-size: 22px;
  font-weight: 750;
  letter-spacing: -.6px;
  margin: 0 0 8px;
  color: #fff;
}
.welcome-sub { color: #aebbd0; margin: 0 0 17px; }
.chips { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 15px; }
.chip {
  border: 1px solid #3b536d;
  border-radius: 12px;
  padding: 10px 13px;
  text-align: left;
  background: #17263b;
  color: #cce6f6;
  font-size: 12px;
  transition: .18s ease;
}
.chip:hover { border-color: var(--accent); color: var(--accent); transform: translateY(-1px); }
.typing { display: flex; align-items: center; gap: 5px; padding: 17px 18px; }
.typing span {
  width: 7px; height: 7px; border-radius: 50%;
  background: var(--accent);
  animation: bounce 1s infinite ease-in-out;
}
.typing span:nth-child(2) { animation-delay: .15s; }
.typing span:nth-child(3) { animation-delay: .3s; }
@keyframes bounce {
  0%, 60%, 100% { transform: translateY(0); opacity: .5; }
  30% { transform: translateY(-6px); opacity: 1; }
}
.composer-wrap { padding: 15px 20px 18px; border-top: 1px solid var(--border); background: #10182ae8; }
.composer {
  max-width: 850px; margin: auto;
  display: flex; align-items: flex-end; gap: 10px;
  border: 1px solid #34455d;
  background: #0e1628;
  border-radius: 17px;
  padding: 9px;
  transition: border-color .2s;
}
.composer:focus-within { border-color: #65e4c4aa; }
textarea {
  resize: none;
  flex: 1;
  min-width: 0;
  max-height: 150px;
  border: 0;
  outline: 0;
  padding: 8px 7px;
  background: transparent;
  color: var(--text);
  line-height: 1.5;
  font-size: 14px;
}
textarea::placeholder { color: #78879f; }
.send {
  width: 43px; height: 43px; flex-shrink: 0;
  border: 0; border-radius: 12px;
  display: grid; place-items: center;
  background: var(--accent); color: #08211d;
  font-size: 19px; font-weight: 800;
  transition: .2s;
}
.send:hover:not(:disabled) { background: #9af4df; transform: translateY(-1px); }
.send:disabled { opacity: .4; cursor: not-allowed; }
.hint { max-width: 850px; margin: 9px auto 0; color: #73829b; font-size: 10px; text-align: center; }
@media (max-width: 600px) {
  body { padding: 0; }
  .app { height: 100dvh; min-height: 0; border-radius: 0; border: 0; }
  header { padding: 14px 15px; }
  .logo { width: 39px; height: 39px; border-radius: 12px; }
  .brand h1 { font-size: 18px; }
  .brand p { font-size: 10px; }
  .clear { padding: 8px 9px; font-size: 11px; }
  .chat { padding: 20px 13px; }
  .content { max-width: 87%; }
  .bubble { font-size: 13px; padding: 11px 13px; }
  .composer-wrap { padding: 11px 12px 13px; }
  .hint { font-size: 9px; }
}
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: .01ms !important; transition-duration: .01ms !important; scroll-behavior: auto !important; }
}
</style>
</head>
<body>
<div class="app">
  <header>
    <div class="brand">
      <div class="logo" aria-hidden="true">⚛</div>
      <div>
        <h1>PhysicsAI</h1>
        <p>Your friendly Class 9 physics tutor</p>
        <div class="status"><span></span> Ready to explore</div>
      </div>
    </div>
    <button class="clear" id="clearBtn" type="button">↺ Clear chat</button>
  </header>

  <main class="chat" id="chat" aria-live="polite" aria-label="Chat messages">
    <div class="message bot">
      <div class="avatar" aria-hidden="true">⚛</div>
      <div class="content">
        <div class="name">PhysicsAI</div>
        <div class="bubble">
          <p class="welcome-title">Hey, future scientist! 👋</p>
          <p class="welcome-sub">I'm PhysicsAI, your friendly physics tutor. Let's make Class 9 physics simple, interesting, and fun!</p>
          Ask me anything about motion, force, gravity, sound, or the world around you.
          <div class="chips" id="starterChips">
            <button class="chip" type="button">Why do objects fall towards Earth?</button>
            <button class="chip" type="button">What is Newton's first law?</button>
            <button class="chip" type="button">Explain speed and velocity.</button>
          </div>
        </div>
      </div>
    </div>
  </main>

  <div class="composer-wrap">
    <form class="composer" id="chatForm">
      <textarea id="userInput" rows="1" placeholder="Ask a physics question..." aria-label="Your question" required></textarea>
      <button class="send" id="sendBtn" type="submit" aria-label="Send message">↑</button>
    </form>
    <div class="hint">Enter to send · Shift + Enter for a new line · Learn something amazing today ✨</div>
  </div>
</div>

<script>
async function callAI(history) {
  // history = [{ role: "user" | "assistant", text: "..." }, ...]
  const url = "https://generativelanguage.googleapis.com/v1beta/models/" + MODEL + ":generateContent";
  const res = await fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json", "x-goog-api-key": API_KEY.trim() },
    body: JSON.stringify({
      system_instruction: { parts: [{ text: SYSTEM_PROMPT }] },
      contents: history.map(m => ({
        role: m.role === "assistant" ? "model" : "user",
        parts: [{ text: m.text }]
      }))
    })
  });
  const data = await res.json().catch(() => ({}));
  if (!res.ok) throw new Error("Error " + res.status + ": " + ((data.error && data.error.message) || "Request failed"));
  const parts = (data.candidates && data.candidates[0] && data.candidates[0].content && data.candidates[0].content.parts) || [];
  const text = parts.filter(p => p.text && !p.thought).map(p => p.text).join("");
  if (!text) throw new Error("The AI sent an empty reply. Try asking again.");
  return text;
}

let history = [];
let waiting = false;

const chat = document.getElementById("chat");
const form = document.getElementById("chatForm");
const input = document.getElementById("userInput");
const sendBtn = document.getElementById("sendBtn");
const clearBtn = document.getElementById("clearBtn");
const starterChips = document.getElementById("starterChips");

function scrollToBottom() {
  requestAnimationFrame(() => {
    chat.scrollTop = chat.scrollHeight;
  });
}

function addMessage(role, text, isError = false) {
  const isUser = role === "user";
  const message = document.createElement("div");
  message.className = "message " + (isUser ? "user" : "bot");

  const avatar = document.createElement("div");
  avatar.className = "avatar";
  avatar.setAttribute("aria-hidden", "true");
  avatar.textContent = isUser ? "🙂" : "⚛";

  const content = document.createElement("div");
  content.className = "content";

  const name = document.createElement("div");
  name.className = "name";
  name.textContent = isUser ? "You" : "PhysicsAI";

  const bubble = document.createElement("div");
  bubble.className = "bubble" + (isError ? " error" : "");
  // textContent safely escapes HTML; CSS white-space preserves line breaks.
  bubble.textContent = text;

  content.append(name, bubble);
  message.append(avatar, content);
  chat.appendChild(message);
  scrollToBottom();
  return message;
}

function addTypingIndicator() {
  const message = document.createElement("div");
  message.className = "message bot";
  message.id = "typingIndicator";

  const avatar = document.createElement("div");
  avatar.className = "avatar";
  avatar.textContent = "⚛";
  avatar.setAttribute("aria-hidden", "true");

  const content = document.createElement("div");
  content.className = "content";

  const name = document.createElement("div");
  name.className = "name";
  name.textContent = "PhysicsAI is thinking...";

  const dots = document.createElement("div");
  dots.className = "bubble typing";
  dots.setAttribute("aria-label", "AI is typing");

  for (let i = 0; i < 3; i++) {
    const dot = document.createElement("span");
    dots.appendChild(dot);
  }

  content.append(name, dots);
  message.append(avatar, content);
  chat.appendChild(message);
  scrollToBottom();
}

function removeTypingIndicator() {
  document.getElementById("typingIndicator")?.remove();
}

function setWaiting(value) {
  waiting = value;
  sendBtn.disabled = value;
  input.disabled = value;
  sendBtn.textContent = value ? "…" : "↑";
  sendBtn.setAttribute("aria-label", value ? "Waiting for reply" : "Send message");
  document.querySelectorAll(".chip").forEach(chip => {
    chip.disabled = value;
  });
}

function resetWelcome() {
  chat.innerHTML = `
    <div class="message bot">
      <div class="avatar" aria-hidden="true">⚛</div>
      <div class="content">
        <div class="name">PhysicsAI</div>
        <div class="bubble">
          <p class="welcome-title">Hey, future scientist! 👋</p>
          <p class="welcome-sub">I'm PhysicsAI, your friendly physics tutor. Let's make Class 9 physics simple, interesting, and fun!</p>
          Ask me anything about motion, force, gravity, sound, or the world around you.
          <div class="chips" id="starterChips">
            <button class="chip" type="button">Why do objects fall towards Earth?</button>
            <button class="chip" type="button">What is Newton's first law?</button>
            <button class="chip" type="button">Explain speed and velocity.</button>
          </div>
        </div>
      </div>
    </div>`;
}

async function sendMessage(rawText) {
  const text = rawText.trim();
  if (!text || waiting) return;

  // Check the key before making any network request.
  if (!API_KEY.trim() || API_KEY.trim() === "PASTE_YOUR_API_KEY_HERE") {
    addMessage("assistant", "Paste your API key on line 6 of index.html, save, and refresh.", true);
    return;
  }

  // Add the question to the visible chat and conversation history.
  addMessage("user", text);
  history.push({ role: "user", text });
  input.value = "";
  resizeInput();
  setWaiting(true);
  addTypingIndicator();

  try {
    const reply = await callAI(history);
    history.push({ role: "assistant", text: reply });
    removeTypingIndicator();
    addMessage("assistant", reply);
  } catch (error) {
    removeTypingIndicator();
    addMessage("assistant", error && error.message ? error.message : "Something went wrong. Please try again.", true);

    // Remove the failed question from history so it won't be sent again.
    history.pop();
  } finally {
    setWaiting(false);
    input.focus();
  }
}

function resizeInput() {
  input.style.height = "auto";
  input.style.height = Math.min(input.scrollHeight, 150) + "px";
}

form.addEventListener("submit", event => {
  event.preventDefault();
  sendMessage(input.value);
});

input.addEventListener("input", resizeInput);

input.addEventListener("keydown", event => {
  if (event.key === "Enter" && !event.shiftKey && !event.isComposing) {
    event.preventDefault();
    if (!waiting) form.requestSubmit();
  }
});

chat.addEventListener("click", event => {
  const chip = event.target.closest(".chip");
  if (chip && !waiting) {
    sendMessage(chip.textContent);
  }
});

clearBtn.addEventListener("click", () => {
  if (waiting) return;
  history = [];
  resetWelcome();
  input.value = "";
  resizeInput();
  input.focus();
});

</script>
</body>
</html>
