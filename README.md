
# PyTutor

PyTutor is a browser-based AI coding tutor. It provides an interactive chat interface with an AI tutor and a simple simulated Python environment.

## Features

* AI-powered tutor using OpenAI API
* Chat-based interface for asking coding questions
* Emulator panel to show simulated Python outputs
* Built with plain HTML, CSS, and JavaScript

## Project Structure

```
pytutor/
  ├── index.html   # Main app (HTML, CSS, JS)
  └── README.md    # Project documentation
```

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/pytutor.git
cd pytutor
```

### 2. Set Up API Key

The project requires an OpenAI API key.

* Obtain a key from: [https://platform.openai.com/](https://platform.openai.com/)
* For quick local testing, you can paste the key directly into `index.html`.
* For deployment, do not expose the key in client-side code. Use a server proxy to securely forward requests.

### 3. Run Locally

You can open `index.html` directly in a browser, or use a local server:

```bash
# Python
python3 -m http.server 8000
# or Node
npx serve
```

Then go to `http://localhost:8000`.

## Notes

* Hardcoding the API key in the front-end is insecure. For production, set up a backend proxy.
* The app is currently front-end only.

## Roadmap

* Add secure backend proxy
* Add code syntax highlighting
* Improve Python emulator
* Add step-by-step tutorials

## License

MIT License

