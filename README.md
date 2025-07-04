# TalkingHeadEX

A full-stack talking avatar demo with neural TTS and real-time lip sync.

## Project Structure

```
TalkingHeadEX/
├── FE/        # Frontend: UI, 3D avatar, TTS client
├── BE/        # Backend: HeadTTS server, models, voices
└── README.md  # Project documentation
```

## Features
- Neural text-to-speech (TTS) with multiple voices
- Real-time lip sync for 3D avatars
- WebSocket and REST API support
- Modular, easy to extend

---

## Backend (BE)

The backend is a Node.js server that runs the HeadTTS engine, serving TTS requests via REST or WebSocket.

### Setup
1. **Install dependencies:**
   ```bash
   cd BE
   npm install
   ```
2. **Download required voice files:**
   - Place `.bin` voice files in the `voices/` directory (see [Kokoro-82M voices](https://huggingface.co/onnx-community/Kokoro-82M-v1.0-ONNX/tree/main/voices)).
3. **Configure:**
   - Edit `headtts-node.json` to set port, voices, and other options.
4. **Start the server:**
   ```bash
   npm start
   # or
   node headtts-node.mjs
   ```

### Endpoints
- `POST /v1/synthesize` — Synthesize speech from text
- `POST /v1/hello` — Health check
- WebSocket: Connect to the same port for real-time TTS

---

## Frontend (FE)

The frontend is a web app that lets users type text, see a 3D avatar, and hear speech with lip sync.

### Setup
1. **Start a local web server:**
   ```bash
   cd FE
   python3 -m http.server 8000
   # or
   npx http-server .
   ```
2. **Open in browser:**
   - Go to `http://localhost:8000/minimal.html`

### Configuration
- Edit `minimal.html` to set the correct HeadTTS backend endpoint (WebSocket or REST):
  ```js
  const headtts = new HeadTTS({
    endpoints: ["ws://127.0.0.1:8882", "webgpu"],
    languages: ['en-us'],
    voices: ["af_bella", "am_fenrir"]
  });
  ```

---

## How It Works
1. User enters text and clicks "Speak" in the frontend.
2. Frontend sends the text to the backend via WebSocket or REST.
3. Backend generates speech audio (and viseme data for lip sync) using neural models.
4. Frontend receives audio and viseme data, plays the audio, and animates the avatar's mouth in sync.

---

## Requirements
- **Backend:** Node.js 18+, voice model files, internet for model downloads
- **Frontend:** Modern browser (Chrome/Edge recommended), local web server

---

## License
MIT

---

## Credits
- [Kokoro-82M TTS Model](https://huggingface.co/onnx-community/Kokoro-82M-v1.0-ONNX)
- [met4citizen/TalkingHead](https://github.com/met4citizen/TalkingHead)