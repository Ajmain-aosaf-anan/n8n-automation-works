# 🤖 Slack Image Description Workflow (n8n)

This workflow listens to image uploads in a specific Slack channel, downloads the image, sends it to OpenAI's GPT-4 Vision API for a detailed description, and responds back in Slack. It also logs each event to Google Sheets.

---

## 📦 Features

- 🔁 Triggered by Slack's `file_shared` event (image-only)
- 🖼️ Downloads and processes images from Slack
- 🧠 Sends images to GPT-4 Vision (`gpt-4-vision-preview`)
- 📝 Replies with the image description in Slack
- 📊 Logs each event to Google Sheets with timestamp and details
- ⚠️ Includes error handling with Slack notifications

---

## 📌 Workflow Overview
graph TD
    A[Slack Trigger: file_shared] --> B[Download Image from Slack]
    B --> C[Send Image to OpenAI GPT-4 Vision]
    C --> D[Format GPT Response]
    D --> E1[Reply in Slack]
    D --> E2[Log to Google Sheet]
    C --> F[Error Handler]
    F --> G[Send Error Message in Slack]

## 🔐 API Keys / Credentials Setup

This workflow uses three external services that require credentials to be configured in your n8n instance.

| Service        | Credential Name        | Purpose                                |
|----------------|------------------------|----------------------------------------|
| **Slack**      | `Slack account`        | To receive file_shared events and send messages back |
| **OpenAI**     | `OpenAI API`           | To send images to GPT-4 Vision for description |
| **Google Sheets** | `Google Sheets account` | To log image descriptions to a spreadsheet |

---

## ✅ How to Configure Credentials in n8n

1. **Open n8n** and go to the **Credentials** tab.
2. Click on **“New Credential”**, then choose the appropriate service:
   - `Slack OAuth2 API`
   - `HTTP Header Auth` (for OpenAI)
   - `Google Sheets OAuth2 API`
3. Name each credential **exactly** as shown in the table (e.g., `Slack account`).
4. Fill in the required fields:
   - **Slack:** Connect your Slack workspace and authorize n8n.
   - **OpenAI:** Use `HTTP Header Auth` and set the header:
     - `Authorization: Bearer YOUR_OPENAI_API_KEY`
   - **Google Sheets:** Connect your Google account with access to the target spreadsheet.

> 💡 **Note:** These credential names must match the ones defined in the workflow (`Slack account`, `OpenAI API`, and `Google Sheets account`), otherwise the workflow will fail to run.

---

## ❌ Error Handling
- If OpenAI or any step fails, a fallback message is sent to Slack:
“Sorry, I couldn't process the image due to an error: ...”

---

### 🛠️ How to Use
- Clone or import the JSON workflow into your n8n instance.
- Add the required credentials.
- Configure the Slack channel and Sheet ID.
- Activate the workflow.
- Upload an image to the Slack channel — watch the magic happen ✨

---

## 🔒 Environment Security Tip

If you're storing secrets in version control, never hardcode API keys into workflows. Use environment variables or secured vaults where possible.
