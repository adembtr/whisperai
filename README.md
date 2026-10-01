# WhisperAI - English Speaking Practice

A hold-to-talk voice chat in the browser for practising spoken English with an AI conversation partner.

**Live demo:** https://adembtr.github.io/whisperai/ (you need your own OpenRouter API key and a text-to-speech webhook, see [Setup](#setup))

## How it works

1. **Hold the microphone button and speak.** The browser's Web Speech API transcribes your speech (`en-US`) and shows the live transcript in the status line.
2. **Release the button.** The transcript is sent to an LLM through the [OpenRouter](https://openrouter.ai/) chat completions API, together with the system prompt and the last 10 messages of the conversation.
3. **Hear the reply.** The reply text is posted to a text-to-speech webhook (an [n8n](https://n8n.io/) workflow in my setup). The returned audio plays automatically and is attached to the chat bubble.

> Despite the repository name, speech-to-text uses the browser's built-in Web Speech API, not OpenAI Whisper.

## Features

- Push-to-talk button that works with mouse and touch, in a mobile-friendly layout
- Live interim transcription while you speak
- Chat view with timestamps; AI replies include a playable audio clip
- Conversation history saved in `localStorage`, with a **Clear Conversation History** button
- Settings panel (gear icon):
  - OpenRouter API key
  - model picker with nine OpenRouter models in three price tiers (default `minimax/minimax-m2.1`)
  - TTS webhook URL
  - editable system prompt
- Default persona **"Alex"**: a casual, friendly American who keeps replies to 1-3 sentences, asks follow-up questions and does not directly correct small grammar mistakes
- Replies capped at 150 tokens to keep the conversation quick
- If text-to-speech fails, the reply is still shown as text
- Everything is in a single `index.html`: no build step, no dependencies

## Setup

1. **OpenRouter API key**: create one at [openrouter.ai/keys](https://openrouter.ai/keys), open Settings and paste it. Settings are saved in your browser's `localStorage`, and the key is sent only to the OpenRouter API.
2. **Text-to-speech webhook**: any HTTP endpoint that accepts `POST` with the JSON body `{"text": "..."}` and responds with JSON `{"audio": "..."}`, where `audio` is a playable audio source (for example a `data:audio/mpeg;base64,...` URI). My n8n workflow is not included in this repository.
3. **Browser**: speech recognition needs a browser with the Web Speech API, such as Chrome, Edge or Safari. In unsupported browsers the talk button is disabled. Microphone access requires HTTPS or `localhost`.

## Run locally

```bash
git clone https://github.com/adembtr/whisperai.git
cd whisperai
python3 -m http.server 8000
```

Then open http://localhost:8000 in a supported browser and allow microphone access.

## Tech stack

- HTML, CSS and vanilla JavaScript (single file)
- Web Speech API (`SpeechRecognition`) for speech-to-text
- OpenRouter chat completions API for the AI replies
- n8n webhook for text-to-speech
- `localStorage` for settings and chat history

## License

Released under the [MIT License](LICENSE).

---

Built by [Adem Batur](https://github.com/adembtr)
