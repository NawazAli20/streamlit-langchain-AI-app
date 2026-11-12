[A live Chatbot](https://app-langchain-ai-app-nawaz-ali.streamlit.app/?embed_options=dark_theme,show_footer,show_colored_line,show_toolbar,light_theme,show_padding,disable_scrolling)

# Deploy a Streaming LangChain Chatbot

This Streamlit demo uses a LangChain agent with streaming responses and short-term memory. The viewer chooses Groq or OpenAI and enters one API key in the sidebar.

| Provider | Model |
| --- | --- |
| Groq | `openai/gpt-oss-120b` |
| OpenAI | `gpt-4.1-mini` |

## Run locally

Install [uv](https://docs.astral.sh/uv/getting-started/installation/), then run these commands from this folder.

```bash
uv venv --python 3.13
uv pip install -r requirements.txt
uv run --no-project streamlit run app.py
```

Open the local URL printed in the terminal and enter a provider API key. API usage follows the selected provider's pricing and limits.

## Publish a public URL

1. Push the contents of this folder to a public GitHub repository.
2. Sign in to [Streamlit Community Cloud](https://share.streamlit.io/) and choose **Create app**.
3. Select your repository and branch, set the main file path to `app.py`, and select Python 3.13 in **Advanced settings**. Viewers enter their own keys through the app.
4. Deploy, then open the resulting `streamlit.app` URL in a private browser window. Try a prompt before sharing the link.

[Streamlit deployment guide](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/deploy)

## Demo prompts

1. **Streaming.** “Explain retrieval-augmented generation in three short paragraphs.”
2. **Memory.** “My name is Nawaz and I live in Peoria, Illinois.” Then ask, “What city did I say I live in?”
3. **New chat.** Select **New chat** and ask the memory question again.

## How the app works

- Each browser session has its own `InMemorySaver` and thread ID. Each request submits the newest message, and the agent reads the earlier turns from its checkpoint. Reloading the page, restarting the app, or selecting **New chat** starts a fresh conversation.
- The agent streams model text with `stream_mode="messages"`. Streamlit updates the answer in place as text arrives.
- The app server receives the password-field value and passes it to the selected provider client. The app keeps the key out of chat history and checkpoints. Use an API key only with a deployment you trust, or run the demo locally.
