# A-Multilingual-Multimodal-AI-Teaching-Assistant-
A Multilingual Multimodal AI Teaching Assistant with Adaptive Explanation and User-Controlled Dialogue Flow 

This repository contains the source code for a Python-based AI Tutoring Agent designed for a Google Colab environment. The primary function of this agent is to facilitate an interactive learning session through a bimodal interface: first, a text-based explanation is provided, which can be followed by a real-time, voice-driven conversational dialogue.

Technical Features
Asynchronous Dialogue System: The agent utilizes a multithreaded architecture to handle simultaneous speaking and listening operations. This allows for a "barge-in" capability, where the user can interrupt the agent's speech synthesis process, simulating a natural conversational turn-taking.

Bimodal Learning Flow: The system implements a two-stage interaction model. Stage one involves static text display for initial comprehension. Stage two is a dynamic, user-initiated voice session for interactive Q&A, thus decoupling information reception from active discussion.

Persona-Driven NLP Core: The agent's conversational logic is powered by the Google Gemini Pro LLM. We employ few-shot prompting techniques to constrain the model's output, forcing it to adopt a consistent "teacher" persona for generating context-aware and pedagogically effective responses.

Robust Command Parsing: To handle multilingual commands (Hinglish/English), a fuzzy string matching algorithm (thefuzz library's partial_ratio) is implemented. This allows for reliable intent recognition even with significant phonetic variations or speech-to-text transcription errors.

Resilient Input Fallback: A stateful counter monitors speech-to-text failures. If transcription fails for two consecutive attempts, the system's main loop automatically transitions the input modality from voice to text, preventing conversational dead-ends.

Technology Stack
Language: Python 3

AI Core: Google Gemini API (gemini-2.5-pro)

Execution Environment: Google Colab

Concurrency: Python threading and queue modules for managing parallel tasks.

Speech Synthesis (TTS): gTTS library.

Speech Recognition (STT): SpeechRecognition library.

Audio Processing: pydub and PyAudio for handling audio streams and playback.

String Comparison: thefuzz for Levenshtein distance-based similarity checks.

System Setup and Execution
1. Prerequisite Configuration
A valid Google Account.

A web browser with necessary permissions for microphone access.

2. API Key Configuration
The Gemini API key must be configured as a secret within the Colab environment to avoid hardcoding.

Navigate to the Secrets tab in the Colab UI.

Define a new secret with the name GOOGLE_API_KEY.

Insert your API key into the 'Value' field.

Activate the 'Notebook access' toggle for this secret.

3. Dependency Installation
Execute the following shell command in a notebook cell to install all system-level and Python dependencies.

Bash

!apt-get -qq install -y ffmpeg portaudio19-dev && pip install -q google-generativeai gTTS pydub SpeechRecognition thefuzz PyAudio
Usage Protocol
Post-installation, execute the main Python script cell.

The program will solicit a topic via a text prompt.

After rendering the text-based explanation, the system will prompt the user to initiate the voice session (yes/no).

On confirmation, grant the browser's request for microphone permissions.

The interactive session will commence, guided by audio and text prompts.

System Architecture
To ensure stability in the Colab environment, the interactive session is architected using a Producer-Consumer design pattern. This is necessary to decouple the UI-blocking operation (audio recording) from the background processing tasks.

The Main Thread acts as the Producer. Its sole responsibility is to interface with the Colab frontend, execute the JavaScript-based audio recorder, and place the resulting audio data chunks into a shared queue.Queue object.

The Listener Thread acts as the Consumer. It runs asynchronously, continuously polling the queue for new audio data. Upon receiving data, it performs the STT and command parsing operations.

This architecture prevents race conditions and deadlocks that would otherwise occur if a background thread attempted to directly manipulate the Colab frontend's JavaScript context.
