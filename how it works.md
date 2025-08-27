
# How PyTutor Works

PyTutor is a lightweight, browser-based educational tool designed to teach coding concepts interactively with the help of an AI model. It combines static front-end technologies (HTML, CSS, JavaScript) with dynamic AI responses from the OpenAI API.

This document explains the inner workings of PyTutor in detail.

---

## 1. Overall Architecture

PyTutor is structured as a **single-page web application**. It does not use any frameworks — only plain HTML, CSS, and JavaScript. The core pieces are:

* **HTML**: Defines the structure of the page (chat window, input box, emulator panel).
* **CSS**: Handles styling (layout, colors, chat bubbles, emulator appearance).
* **JavaScript**: Provides interactivity, manages user input, communicates with the AI, and updates the DOM with responses.
* **OpenAI API**: Supplies the intelligence — generating responses in the tutor’s voice and style.

---

## 2. Core Components

### 2.1 Chat Interface

* The user interacts with PyTutor via a chat box at the bottom of the screen.
* Each message typed is captured by JavaScript and rendered as a “chat bubble” inside the chat window.
* Messages are styled differently depending on the sender:

  * **User messages** appear aligned to the right.
  * **Tutor responses** appear aligned to the left.

### 2.2 AI Tutor Behavior

* A **system prompt** is defined in the code (`SYSTEM_PROMPT`).
* This prompt controls the tutor’s personality, teaching style, and level of detail.
* When the user sends a message:

  1. JavaScript constructs a **conversation history** (system + previous user messages + tutor replies).
  2. This conversation is sent to the OpenAI API as a chat completion request.
  3. The model responds with the next tutor reply.
  4. JavaScript inserts the reply into the chat window as a new tutor bubble.

### 2.3 Emulator Panel

* The emulator is a side panel (or a section below the chat) that simulates Python code execution.
* Currently it does not run real Python code in the browser. Instead:

  * It displays code snippets or outputs suggested by the AI.
  * Example: if the user asks “print(2+2)”, the tutor can simulate the output in the emulator as if Python executed it.
* Future versions could integrate a sandboxed Python runtime (e.g., Pyodide or Skulpt) for real code execution.

---

## 3. Data Flow (Step by Step)

Here’s how information flows through the system when a user interacts with PyTutor:

1. **User Input**

   * The user types a question (e.g., “What does a for loop do in Python?”) into the text field and presses Enter.

2. **Event Handling**

   * JavaScript captures the `keydown` event, checks for Enter, and prevents default form submission.

3. **Render User Message**

   * The message is immediately added to the chat panel as a user bubble.

4. **Prepare API Request**

   * JavaScript builds the request payload:

     * `model`: which OpenAI model to use (e.g., `gpt-4o-mini`).
     * `messages`: an array containing the system prompt, previous messages, and the new user input.

5. **Send API Request**

   * A `fetch` request is made to the OpenAI API endpoint.
   * Headers include authorization with the API key.
   * The request is asynchronous; a loading indicator may be shown while waiting.

6. **Receive AI Response**

   * The OpenAI API returns a JSON response containing the tutor’s reply.
   * JavaScript extracts the text from `response.choices[0].message.content`.

7. **Render Tutor Message**

   * The tutor’s reply is appended to the chat as a new tutor bubble.
   * If the message contains code snippets, they may be formatted in a code block style.

8. **Emulator Integration**

   * If the tutor’s response contains an output simulation, JavaScript may also update the emulator panel to display results.
   * Example: showing “Hello, world!” under a simulated terminal output.

---

## 4. Security Considerations

* In the current version, the API key is placed directly in the front-end code.

  * **Risk:** Anyone inspecting the page can see and misuse the key.
* Safer architecture involves adding a backend proxy:

  * The browser sends requests to the backend (Node, Flask, etc.).
  * The backend attaches the API key securely and forwards the request to OpenAI.
  * The frontend never exposes the API key directly.

---

## 5. User Experience Principles

PyTutor is built on educational best practices:

* **Step-by-step guidance**: The tutor explains concepts gradually, instead of overwhelming the learner.
* **Clarity**: Replies avoid jargon or explain terms when they appear.
* **Encouragement**: The tutor’s tone is supportive and approachable.
* **Simulation over execution**: To keep things simple and safe, the Python emulator is simulated rather than running real code.

---

## 6. Extending PyTutor

Future improvements could include:

* **Real Python Execution**: Integrating Pyodide (Python compiled to WebAssembly) to allow running real Python code in the browser.
* **Streaming Responses**: Show tutor messages word-by-word as they arrive, for a more conversational effect.
* **Syntax Highlighting**: Use libraries like Prism.js to display code with colors.
* **Lesson Mode**: Pre-designed tutorials where the AI guides the student through a curriculum.
* **Collaboration**: Allow multiple learners to connect to the same tutor session.

---

## 7. Summary

In essence, PyTutor works by combining:

1. A **chat UI** built in HTML/CSS/JS.
2. An **AI tutor persona** defined via the system prompt.
3. The **OpenAI API**, which generates intelligent, context-aware responses.
4. A **simulated Python emulator** for reinforcing code understanding.

Together, these elements create a lightweight, approachable platform for learning programming concepts interactively.

