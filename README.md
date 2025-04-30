# Telegram to Twitter URL Summarization Workflow

This project automates the process of taking a URL and subject sent via Telegram, scraping and parsing the URL content, summarizing it with OpenRouter (using the Gemini model: `google/gemini-flash-1.5-8b`), and posting the summary as a tweet on Twitter. The workflow ensures the summary is under Twitter’s 280-character limit before posting, making it a seamless way to share insights on social media.

## Overview
The workflow is built using **n8n** (a no-code automation tool) and involves the following steps:
1. **Telegram Input**: Receive a URL and subject via a Telegram message (e.g., "Subject: Plastic Pollution, URL: https://example.com/plastic").
2. **Scrape and Parse URL**: Extract the main content from the URL.
3. **Summarize with OpenRouter**: Use the Gemini model to summarize the content (e.g., "Plastic pollution is harming human health at every stage... #PlasticPollution").
4. **Length Check**: Ensure the summary is under 280 characters.
5. **Post to Twitter**: Share the summary as a tweet using the Twitter API.

## Contents
- `video/telegram_to_twitter_workflow.mp4`: A video tutorial explaining the workflow with a diagram.
- `diagram.png`: A flowchart visualizing the process.
- `script.md`: The narration script used in the video.
- `n8n_workflow.json`: The n8n workflow configuration file for this automation.
- `screenshots/`: Folder containing visual documentation of the workflow.

## Setup Instructions
1. **Set Up n8n**:
   - Use n8n Cloud or a self-hosted instance (e.g., `http://localhost:5678`).
   - Import the workflow from [`n8n_workflow.json`](./n8n_workflow.json) (go to Workflows > Import > Import from File).
2. **Configure Credentials**:
   - **Telegram API**: Add your Telegram Bot Token (create a bot via BotFather on Telegram).
   - **OpenRouter API**: Add your OpenRouter API key (sign up at [openrouter.ai](https://openrouter.ai)).
   - **Twitter API**: Add your Twitter OAuth 1.0a credentials (API Key, API Secret, Access Token, Access Token Secret) with Read and Write permissions.
3. **Activate the Workflow**:
   - Toggle "Active" in n8n and test by sending a Telegram message with a URL and subject.
4. **Verify Output**:
   - Check your Twitter profile for the posted tweet (e.g., "Plastic pollution is harming human health... #PlasticPollution").

## Workflow Details
The n8n workflow includes nodes for:
- **Telegram Trigger**: Listens for messages with a URL and subject.
- **HTTP Request**: Scrapes and parses the URL content.
- **OpenRouter API**: Summarizes the content using the `google/gemini-flash-1.5-8b` model.
- **Code Node**: Extracts the summary into a `text` field for the Twitter node.
- **Twitter (X) Node**: Posts the summary as a tweet.

### Troubleshooting
- **403 Forbidden Error**: Ensure your Twitter API credentials have Read and Write permissions in the Twitter Developer Portal. Verify the Callback URI matches your n8n setup (e.g., `https://oauth.n8n.cloud/oauth2/callback`).
- **Missing Parameter Error**: Ensure the OpenRouter response is properly mapped. The Code node extracts `choices[0].message.content` into `json.text`. Use `{{ $node["Code"].json["text"] }}` in the "X" node’s Text field.
- **Undefined Output in X Node**: If the "X" node outputs `[undefined]`, verify the Code node name in the expression and ensure the `text` field is populated. Add a Set node to debug the data flow (e.g., log `{{ $input.first().json.text }}`).

## Video Tutorial
I’ve shared a video walkthrough on LinkedIn: [Telegram to Twitter Workflow Video](https://www.linkedin.com/posts/arhumkhan049_aiautomation-growthhacking-nocoderevolution-activity-7321959807718948864-p4Ea?utm_source=share&utm_medium=member_desktop&rcm=ACoAADo3m7wBae9QfdCB6um2S2F195YcIe_tVTY)


## Contact
Have questions or ideas? Reach out to me on [LinkedIn](https://www.linkedin.com/in/arhumkhan049) or email me at educationarhum@gmail.com
