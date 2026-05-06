# 🧠 Local LLM CLI Chat Agent (LM Studio)

A simple command-line chatbot powered by a locally running LLM via **LM Studio** using an OpenAI-compatible API.

This project demonstrates:

* Local LLM inference (no cloud required)
* Stateful conversation memory
* CLI-based interactive chat loop
* Minimal agent architecture

---

# 🚀 Features

* 🧩 **Local LLM integration** via HTTP API
* 💬 **Multi-turn conversation memory**
* ⚡ **Fast CLI interaction**
* 🔌 **OpenAI-compatible API usage**
* 🛠️ Lightweight and easy to extend

---

# 🏗️ Architecture

```text
User (CLI)
   ↓
Agent.chat()
   ↓
HTTP Request
   ↓
LM Studio (localhost:1234)
   ↓
LLM Response
   ↓
Console Output
```

---

# 📦 Requirements

* Python 3.10+
* LM Studio running locally
* A loaded model (e.g. Qwen)

Install dependencies:

```bash
pip install requests rich
```

---

# ⚙️ Setup

## 1. Start LM Studio

* Open LM Studio
* Load a model (e.g. `qwen3.5`)
* Start the **local server**

Default endpoint:

```text
http://127.0.0.1:1234/v1
```

---

## 2. Verify API is running

```bash
curl http://127.0.0.1:1234/v1/models
```

---

## 3. Run the app

```bash
python main.py
```

---

# 💻 Usage

```text
You: hello
Assistant: Hello! How can I help you today?

You: what is 2 + 2
Assistant: 4

You: exit
Goodbye!
```

---

# 🧠 How It Works

## Agent structure

```python
@dataclass
class Agent:
    model: str
    base_url: str
    messages: list
```

---

## Conversation memory

Messages are stored as:

```json
[
  {"role": "user", "content": "Hello"},
  {"role": "assistant", "content": "Hi!"}
]
```

This is sent on every request → enabling **context awareness**

---

## API call

```python
requests.post(
    f"{base_url}/chat/completions",
    json={
        "model": model,
        "messages": messages
    }
)
```

---

# 🔁 Chat Loop

```python
while True:
    user_input = console.input()

    if user_input in {"quit", "exit"}:
        break

    response = agent.chat(user_input)
```

This creates a continuous interaction cycle.

---

# ⚠️ Known Limitations

## 1. No output control

You are not specifying:

```python
temperature
max_tokens
```

This can lead to:

* inconsistent responses
* overly long outputs
* occasional empty responses

---

## 2. No error handling for empty responses

```python
response = message.get("content") or ""
```

If model returns empty → chat breaks silently.

---

## 3. Global memory growth

* `messages` keeps growing
* No truncation / token control

---

## 4. Single-session only

* No multi-user support
* No session isolation

---

# 🛠️ Recommended Improvements

## Add generation controls

```python
json={
    "model": self.model,
    "messages": self.messages,
    "temperature": 0.2,
    "max_tokens": 400
}
```

---

## Add response validation

```python
response = (message.get("content") or "").strip()

if not response:
    response = "Model returned empty response."
```

---

## Limit memory size

```python
self.messages = self.messages[-10:]
```

---

## Add debug logging

```python
print(json.dumps(data, indent=2))
```

---

# 🔐 Notes on API Key

```python
api_key = "NO_API_KEY"
```

LM Studio does not require authentication, but the header is kept for compatibility.

---

# 📁 Project Structure

```text
.
├── main.py
└── README.md
```

---

# 🧪 Example Models

* Qwen (recommended)
* Mistral
* Phi
* LLaMA variants

Use quantized versions (Q4) for better performance.

---

# 🧠 Key Concepts Learned

* Local LLM serving
* OpenAI-compatible API usage
* Stateful conversation design
* CLI interaction loops
* Prompt + message structuring

---

# 🚀 Next Steps

* Convert to FastAPI web app
* Add session-based memory
* Implement tool calling
* Add streaming responses
* Integrate with your Agent architecture (Project 6)

---

# ✅ Summary

This project is a **minimal but functional local AI agent**:

```text
Simple → Extendable → Fully Local
```

It forms a solid base for:

* agent systems
* local assistants
* experimentation with LLM workflows