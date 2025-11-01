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



