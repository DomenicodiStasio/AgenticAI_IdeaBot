# 🎙️💡 IdeaBot: Telegram Voice-to-Categorized Idea Organizer

<p align="center">
  <img src="https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-logo.png" alt="n8n logo" width="120" draggable="false">
  <br>
  <br>
  <img src="https://img.shields.io/badge/Platform-n8n-EA4AAA?style=for-the-badge&logo=n8n&logoColor=white" alt="n8n platform">
  <img src="https://img.shields.io/badge/Integration-Telegram-0088CC?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram integration">
  <img src="https://img.shields.io/badge/AI-Gemini_Powered-8E75B2?style=for-the-badge&logo=google-gemini&logoColor=white" alt="Gemini integration">
</p>

---

## 🌟 The Problem: Great ideas never strike when you're at your desk.
They hit while you’re driving, running, or at the grocery store. Opening a notes app, typing, and categorizing takes too long... and the spark vanishes.

## ✨ The Solution: IdeaFlow
Turn your Telegram Bot into a personal brain assistant. Send a **10-second voice message**, and IdeaFlow handles all the dirty work.

### 🔄 The Idea Lifecycle
1.  **Speak**: Record a voice note on Telegram 🗣️
2.  **AI Transcribes**: Groq/Whisper converts voice to text 📝
3.  **The Brain (Gemini)**: Analyzes the text to extract Category, Priority (High/Med/Low), and Difficulty 🧠
4.  **Saves**: Archives everything to your Database (Google Sheets/Airtable) 📊
5.  **Reports**: On-demand, it generates an **elegantly formatted HTML/PDF report** and sends it back to you 📄

---

## 📸 Project Showcase

### The Automation Engine (n8n)
<img width="758" height="272" alt="image" src="https://github.com/user-attachments/assets/f6cdd7ae-32d0-4f39-be8c-d11d336a1b29" />

### Simple Bot Interaction (Telegram)
<img width="1024" height="557" alt="image" src="https://github.com/user-attachments/assets/b9df9bea-7feb-4218-87ae-1d7cc37fa999" />

### The Telegram Result (HTML Report)
<img width="1045" height="638" alt="image" src="https://github.com/user-attachments/assets/6ef6eda9-c8aa-453b-aa94-1d2c98febbe4" />

---

## 🛠️ The Tech Stack

To replicate this magic, you'll need these "LEGO bricks":

| Component | Recommended Service | Role |
| :--- | :--- | :--- |
| **Trigger & Output** | <img src="https://img.shields.io/badge/Telegram-0088CC?style=flat&logo=telegram&logoColor=white" alt="Telegram"> | Receives voice & sends reports. |
| **Transcription (STT)**| <img src="https://img.shields.io/badge/Groq-f5f5f5?style=flat&logo=groq&logoColor=black" alt="Groq"> / Whisper | Converts `.oga` to text. |
| **The Brain (LLM)** | <img src="https://img.shields.io/badge/Google_Gemini-8E75B2?style=flat&logo=google-gemini&logoColor=white" alt="Gemini"> | Smartly categorizes and analyzes the idea. |
| **Database** | <img src="https://img.shields.io/badge/n8n_Data_Tables-EA4AAA?style=flat&logo=n8n&logoColor=white" alt="n8n"> | Built-in, self-contained database. |
| **Engine** | <img src="https://img.shields.io/badge/n8n-EA4AAA?style=flat&logo=n8n&logoColor=white" alt="n8n"> | The glue that connects it all. |

---

## 🤖 Bot Commands

* **Send a Voice Message**: Automatically adds a new idea.
* `/idealist`: Generates and sends the stylish HTML report with all saved ideas.
* `/start`: Welcome message and instructions.

---

## 🧩 Technical Focus: The "Pure" Binary Generator

The most challenging part was creating a report that looked **beautiful on mobile** without using expensive third-party PDF services.

I used a custom **JavaScript Code node** that transforms database JSON directly into a binary `text/html` file. This ensures:
1.  ✅ **Speed**: No external rendering delays.
2.  ✅ **Zero Cost**: Runs entirely within your n8n instance.
3.  ✅ **CSS Styling**: Colors, rounded borders, and priority tags are baked directly into the file.

```javascript
// Snippet of the core HTML generator within the Code node
const htmlBody = `
  <html>
    <head><style>/* Stylish CSS here */</style></head>
    <body><h1>My Ideas</h1> /* ...mapped ideas... */ </body>
  </html>`;

// The n8n magic to create a file ready for Telegram
const binaryData = await helpers.prepareBinaryData(Buffer.from(htmlBody), 'My_Ideas.html', 'text/html');
return [{ binary: { data: binaryData } }];
