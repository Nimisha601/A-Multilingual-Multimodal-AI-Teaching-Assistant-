# A-Multilingual-Multimodal-AI-Teaching-Assistant-

This project is an intelligent tutoring assistant we built that is both multimodal and multilingual, powered by Google's Gemini LLM. The whole idea was to move past the limits of traditional, passive chatbots and create a learning experience that feels genuinely interactive and adaptive. We developed the entire system to run within a Google Colab notebook.

Core Features
Two-Stage Learning Flow: We designed a unique "Read then Discuss" model. First, a user gets a detailed text-based explanation, which lets them absorb the information at their own speed. After that, they can choose to opt-in to an interactive "Discuss Mode" to explore the topic more deeply.

Engineered Teacher Persona: We used Few-Shot Prompting to give the AI a specific personality. It's designed to be a friendly, patient, and encouraging "teacher." This persona is prompted to explain complex topics step-by-step, use real-life analogies, and add "Fun Facts" to keep the learning process engaging.

Natural Conversation (Barge-In): The user can interrupt the AI at any time while it's speaking by simply saying "stop" or "ruko." We achieved this using Python's threading library, which creates a much more natural and human-like conversational dynamic.

Multimodal Input: When in "Discuss Mode," the user isn't limited to typing. They have the choice to either type their query or speak it, which makes the platform more accessible and flexible.

Dynamic Diagram Generation: The tutor can also generate and display colorful flowcharts and diagrams on-the-fly. We use Mermaid.js for this, allowing it to visually explain complex concepts, like the water cycle, for instance.

How It Works
Here's a breakdown of the process:

Stage 1: Read Mode The user starts by entering a topic. The Gemini model, guided by a "Master Prompt" we wrote, generates a high-quality, text-only explanation.

Stage 2: The Choice After reading, the user is simply asked if they would like to "DISCUSS" the topic.

Stage 3: Discuss Mode If the user agrees, the fully interactive session kicks off.

A Listener Thread starts running continuously in the background. Its only job is to listen for "stop" commands.
A Main Thread handles the actual conversation. It gets the user's input (either from voice or text), fetches a response from Gemini, and then speaks that response back sentence-by-sentence.
The crucial part is that before speaking each new sentence, the main thread checks for the "stop signal" from the listener thread. This "barge-in" capability is what allows the user to take control of the conversation at any moment.

Technology Stack
LLM: Google Gemini (using the google-generativeai library)
Speech-to-Text: SpeechRecognition
Text-to-Speech: gTTS
Concurrency: Python's threading library
Diagrams: Mermaid.js (which we render in Colab using HTML/JS)
Environment: Google Colab
This project essentially demonstrates how you can build a very sophisticated, state-of-the-art conversational AI with advanced features, all just by using publicly available tools.


Project Implementation Plan: A Cell-by-Cell Breakdown

This document outlines the step-by-step implementation of the Conversational Tutoring Agent, with each logical component organized into its own Google Colab cell. This modular approach allows for independent development, testing, and easy modification.

Cell 1: Setup & Installation

Purpose: To install all necessary third-party Python libraries required for the project.
Actions: This cell will execute shell commands to install:
!pip install google-generativeai: For interacting with the Google Gemini API.
!pip install gTTS: (Google Text-to-Speech) For converting the AI's text responses into audible speech.
!pip install SpeechRecognition: For transcribing the user's spoken voice into text.
!pip install pydub: An audio manipulation library, often required as a dependency for speech recognition tasks.

Cell 2: Import Libraries & API Key Setup

Purpose: To import the installed libraries into our script and securely configure the Gemini API.

Actions:
Import all required modules (e.g., google.generativeai as genai, speech_recognition as sr, gTTS, threading).
Access the GOOGLE_API_KEY securely from the Colab 'Secrets' manager.
Configure the genai module with the API key to authenticate our requests.

Cell 3: The "Brain" - Master Prompt & Persona

Purpose: To define the core logic of our AI tutor, engineering its "teacher" persona.

Function: get_explanation(topic_query, conversation_history)
This function will contain our "Master Prompt" based on the Few-Shot Prompting technique.
The prompt will instruct the LLM to:
Adopt a Persona: "You are 'EduPal,' a friendly, patient, and encouraging AI tutor..."
Follow Rules: "Explain topics step-by-step," "Use simple analogies," "Add one 'Fun Fact!' to keep it engaging..."
Learn from Examples: Provide 1-2 examples of a good teacher-like response vs. a generic bot response.
It will take the user's query and the chat history as input and return a high-quality, persona-driven text response from the Gemini model.

Cell 4: The "Senses" - Audio & Text I/O Tools

Purpose: To create all the necessary helper functions for handling user input and AI output.

Functions:

speak(text): A simple, non-interruptible function that takes text, uses gTTS to create an audio file, and plays it in the Colab output.
listen(): Uses the SpeechRecognition library to capture audio from the user's microphone and return the transcribed text. Includes error handling (e.g., "Sorry, I didn't catch that.").
get_user_input(): This is our multimodal input function. It will prompt the user (e.g., "Type your query, or press Enter to speak..."). If the user types, it returns the text. If they press Enter, it calls the listen() function.

Cell 5: Advanced Feature - The "STOP" Mechanism (Modular Cell)

Purpose: To implement the complex "barge-in" (interrupt) logic in a single, isolated cell.
Components: This cell uses the threading library.
stop_speaking_signal: A global threading.Event() object is created to act as a signal between threads.
listener_worker(): This function runs on a separate background thread. Its only job is to continuously listen. If it detects a "stop" or "ruko" command, it sets the stop_speaking_signal.
speak_interruptible(text): A new speak function. It splits the AI's response into sentences. Before speaking each sentence, it checks if stop_speaking_signal.is_set(). If it is, the function stops speaking immediately.

Cell 6: Bonus Feature - Diagram Generator

Purpose: To dynamically generate and display diagrams for the user.
Function: generate_diagram(topic_for_diagram)
This function will prompt the Gemini model with a specific request (e.g., "Generate the Mermaid code for a fun and colourful flowchart of the {topic}...").
It will then take the text-based Mermaid code returned by Gemini.
Using HTML and JavaScript (executed within the Colab output), it will render that code into a beautiful, visual diagram for the user to see.

Cell 7: The Main Program Flow (Part 1 - Text Mode)

Purpose: To define the main application logic, starting with the text-only stage.
Function: main_session() (Part 1)
Welcomes the user.
Prompts the user for their initial topic using input().
Stage 1: Calls get_explanation() (from Cell 3) to get the detailed text response.
Prints the formatted text explanation to the screen.
Stage 2: Asks the user, "Would you like to DISCUSS this topic? (Type 'DISCUSS' to start voice chat)."

Cell 8: The Main Program Flow (Part 2 - Discuss Mode)

Purpose: To complete the main_session() function by adding the interactive "Discuss Mode."
Logic: main_session() (Part 2)
Checks if the user's input is "DISCUSS".
If yes, it:
Starts the listener_worker() (from Cell 5) in the background.
Initializes an empty conversation_history list.
Enters a while True loop for the conversation.
Inside the loop:
Calls get_user_input() (from Cell 4) to get the user's query (voice or text).
Checks for a "quit" command to break the loop.
Clears the stop_speaking_signal before getting a new response.
Calls get_explanation() (from Cell 3), passing the new query and the history.
Calls speak_interruptible() (from Cell 5) to speak the AI's response.
When the loop breaks, it signals the listener_worker to stop.

Cell 9: Start the Project!

Purpose: To run the entire application.
Action: This cell contains the single line of code that executes the main function:
main_session()
