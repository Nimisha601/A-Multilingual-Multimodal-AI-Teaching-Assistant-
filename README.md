# EduPal:-Multilingual-Multimodal-AI-Teaching-Assistant

EduPal is an interactive, voice-enabled AI teaching assistant designed to make learning fun and accessible. Unlike standard chatbots, EduPal acts as a friendly tutor that explains complex concepts using analogies, generates visual diagrams, and communicates via voice (Speech-to-Text & Text-to-Speech).

Powered by Google Gemini API, EduPal supports multimodal interaction—you can type, speak, or ask for visual flowcharts.

##Key Features

Voice Interaction:

Speak to AI: Uses SpeechRecognition to listen to your queries via microphone.

AI Speaks Back: Uses gTTS (Google Text-to-Speech) to explain topics verbally.

Visual Learning (Diagram Generator):

Automatically generates Mermaid.js flowcharts for topics like "Human Heart" or "Water Cycle".

Renders diagrams directly within the notebook using HTML/JavaScript.

Smart Persona:

Acts as a friendly teacher who uses real-world analogies and "Fun Facts" to explain concepts.

Maintains conversation history (Memory) to have a continuous discussion.

Interruptible Speech:

Includes a "STOP" button feature allowing users to interrupt the AI while it is speaking, mimicking a real conversation flow.

Multilingual Capabilities:

Capable of understanding and responding in English and Hinglish (Hindi + English mix).

##Tech Stack
Core AI: Google Gemini API (gemini-1.5-flash recommended for stability).

Language: Python.

Audio Processing:

gTTS (Text-to-Speech).

SpeechRecognition (Speech-to-Text).

pydub (Audio manipulation).

Visualization: Mermaid.js (via IPython HTML display).

Environment: Google Colab (optimized for browser-based audio recording).

Installation & Setup
Clone the Repository:

Bash
git clone https://github.com/your-username/EduPal-AI-Tutor.git
cd EduPal-AI-Tutor

Install Dependencies: This project requires specific libraries. Run the following in your environment:

!pip install -q google-generativeai gTTS SpeechRecognition pydub

API Key Configuration:

Get your free API Key from Google AI Studio.

If using Google Colab, add your key to the "Secrets" tab with the name GOOGLE_API_KEY.

##How to Use
Start the Session: Run the main_session() function.

Choose a Topic:

Type a topic name (e.g., "Black Holes").

For Diagrams: Type "diagram of [topic]" (e.g., "diagram of photosynthesis") to get a visual flowchart.

Discuss Mode:

After the initial explanation, type DISCUSS to enter interactive mode.

You can now speak your questions or type them.

Press the STOP SPEAKING button if you want to cut the explanation short.

Note on Rate Limits
If you encounter a 429 Resource Exhausted error, please switch the model in the code from gemini-2.5-flash-preview to gemini-1.5-flash in the setup cell.




