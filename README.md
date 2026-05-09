# WhatsApp → Excel Bot

Automatically saves WhatsApp **photos** (extracts text via AI) and **voice messages** (transcribes to text) into an Excel sheet — running entirely in VS Code on your PC.

---

## Folder Structure

```
whatsapp-to-excel/
├── src/
│   ├── index.js          ← Main bot (run this)
│   ├── excelHelper.js    ← Creates & writes Excel file
│   ├── aiHelper.js       ← OpenAI Vision + Whisper
│   └── mediaHelper.js    ← Saves images/audio to disk
├── downloads/            ← Auto-created: saved media files
├── .env                  ← Your API key & config (DO NOT share)
├── .gitignore
└── package.json
```

---

## Setup Steps

### Step 1 — Install Node.js
Download from https://nodejs.org (choose LTS version)
Verify: open terminal → `node --version`

### Step 2 — Open project in VS Code
```
File → Open Folder → select whatsapp-to-excel folder
```

### Step 3 — Install dependencies
Open VS Code terminal (Ctrl + ` ) and run:
```bash
npm install
```
This takes 1-2 minutes (installs WhatsApp, OpenAI, ExcelJS packages).

### Step 4 — Get your OpenAI API Key
1. Go to https://platform.openai.com/api-keys
2. Click "Create new secret key"
3. Copy the key (starts with sk-)

### Step 5 — Edit your .env file
Open `.env` in VS Code and replace:
```
OPENAI_API_KEY=sk-your-openai-api-key-here
```
with your actual key.

### Step 6 — Run the bot
```bash
node src/index.js
```

### Step 7 — Scan QR Code
- A QR code appears in your terminal
- Open WhatsApp on your phone
- Go to: ⋮ Menu → Linked Devices → Link a Device
- Scan the QR code

### Step 8 — Test it!
Send a photo or voice message to any WhatsApp chat.
Check `whatsapp_data.xlsx` — your message appears as a new row!

---

## Excel Output Columns

| Date | Time | From (Number) | Message Type | Extracted Text | Media File | Raw Caption |
|------|------|---------------|--------------|----------------|------------|-------------|
| 10/04/2026 | 10:30 AM | 919876543210 | Image | "Invoice #101 Total: ₹500" | downloads/image_... | |
| 10/04/2026 | 10:35 AM | 919876543210 | Voice | "Please confirm the delivery order" | downloads/ptt_... | |

---

## Optional Config (.env)

```env
# Only process messages from specific numbers (leave blank = all numbers)
ALLOWED_NUMBERS=919876543210,918765432109

# Change language for voice transcription in aiHelper.js
# 'en' = English, 'ta' = Tamil, 'hi' = Hindi, 'te' = Telugu
```

To change voice language, open `src/aiHelper.js` and change:
```js
language: 'en',  // ← change to 'ta', 'hi', 'te', etc.
```

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| QR code not showing | Delete `.wwebjs_auth` folder and restart |
| "OPENAI_API_KEY not set" | Check your `.env` file |
| Bot disconnects | It auto-reconnects; keep terminal open |
| Excel file locked | Close Excel before the bot writes to it |
| `npm install` fails | Make sure Node.js is installed correctly |

---

## Keep Bot Running

While the bot runs in VS Code terminal, **don't close the terminal**.
For 24/7 use, install pm2:
```bash
npm install -g pm2
pm2 start src/index.js --name whatsapp-bot
pm2 save
```
