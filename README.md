# -Chatbot-using-NLP-project-with-Python-NLTK-Flask-deployed-as-a-small-web-app-.
📁 Folder Structure
chatbot/
│
├── app.py                # Flask web app
├── chatbot_model.py      # Core chatbot logic using NLTK
├── intents.json          # Predefined questions/intents
├── templates/
│    └── index.html       # Frontend (chat UI)
└── static/
     └── style.css        # Simple styling

🧩 Step 1: intents.json

Create a file named intents.json in your chatbot folder:

{
  "intents": [
    {
      "tag": "greeting",
      "patterns": ["Hi", "Hello", "Hey", "Good morning", "Good evening"],
      "responses": ["Hello!", "Hi there!", "Hey! How can I help you?"]
    },
    {
      "tag": "goodbye",
      "patterns": ["Bye", "See you later", "Goodbye"],
      "responses": ["Goodbye!", "See you later!", "Take care!"]
    },
    {
      "tag": "thanks",
      "patterns": ["Thanks", "Thank you", "That's helpful"],
      "responses": ["You're welcome!", "Glad I could help!"]
    },
    {
      "tag": "about",
      "patterns": ["Who are you?", "What can you do?", "Tell me about yourself"],
      "responses": ["I'm a simple chatbot built using Python, NLTK, and Flask."]
    },
    {
      "tag": "name",
      "patterns": ["What is your name?", "Your name please"],
      "responses": ["I'm ChatBot created by Ashwini!"]
    }
  ]
}

🧠 Step 2: chatbot_model.py

This script handles the NLP processing (tokenization, lemmatization, etc.)

import random
import json
import nltk
from nltk.stem import WordNetLemmatizer
from nltk.tokenize import word_tokenize

# Download necessary NLTK data
nltk.download('punkt')
nltk.download('wordnet')

lemmatizer = WordNetLemmatizer()

# Load intents file
with open('intents.json', 'r') as file:
    intents = json.load(file)

def clean_text(text):
    """Tokenize and lemmatize input text"""
    tokens = word_tokenize(text.lower())
    return [lemmatizer.lemmatize(word) for word in tokens]

def get_response(user_input):
    """Return chatbot response based on matching intent"""
    user_words = clean_text(user_input)

    for intent in intents['intents']:
        for pattern in intent['patterns']:
            pattern_words = clean_text(pattern)
            if set(pattern_words).issubset(set(user_words)):
                return random.choice(intent['responses'])
    return "I'm not sure I understand. Could you please rephrase?"

🌐 Step 3: app.py (Flask Web App)
from flask import Flask, render_template, request, jsonify
from chatbot_model import get_response

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/get", methods=["POST"])
def chat():
    user_input = request.form["msg"]
    response = get_response(user_input)
    return jsonify({"response": response})

if __name__ == "__main__":
    app.run(debug=True)

💬 Step 4: templates/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Chatbot using NLP</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
<div class="chatbox">
    <h2>🤖 Chatbot</h2>
    <div id="chatlog"></div>
    <form id="chat-form">
        <input type="text" id="user-input" placeholder="Type your message..." required>
        <button type="submit">Send</button>
    </form>
</div>

<script>
    const form = document.getElementById("chat-form");
    const chatlog = document.getElementById("chatlog");

    form.onsubmit = async (e) => {
        e.preventDefault();
        const input = document.getElementById("user-input");
        const msg = input.value.trim();
        if (!msg) return;

        chatlog.innerHTML += `<div class='user-msg'>You: ${msg}</div>`;
        input.value = "";

        const response = await fetch("/get", {
            method: "POST",
            headers: {"Content-Type": "application/x-www-form-urlencoded"},
            body: `msg=${encodeURIComponent(msg)}`
        });
        const data = await response.json();
        chatlog.innerHTML += `<div class='bot-msg'>Bot: ${data.response}</div>`;
        chatlog.scrollTop = chatlog.scrollHeight;
    };
</script>
</body>
</html>

🎨 Step 5: static/style.css
body {
    font-family: Arial, sans-serif;
    background: #f1f1f1;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.chatbox {
    background: white;
    width: 400px;
    border-radius: 10px;
    box-shadow: 0 0 10px gray;
    padding: 20px;
}

#chatlog {
    height: 300px;
    overflow-y: scroll;
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 10px;
}

.user-msg {
    text-align: right;
    color: blue;
    margin: 5px 0;
}

.bot-msg {
    text-align: left;
    color: green;
    margin: 5px 0;
}

form {
    display: flex;
}

input[type="text"] {
    flex: 1;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
}

button {
    padding: 10px 15px;
    margin-left: 5px;
    border: none;
    background: #4CAF50;
    color: white;
    border-radius: 5px;
    cursor: pointer;
}

▶️ Step 6: Run the Chatbot

In your terminal:

pip install flask nltk
python app.py


Then open your browser at http://127.0.0.1:5000/
 💬

🖼 Example Chat Output (Screenshot Alternative)
User: Hello
Bot: Hi there!

User: What can you do?
Bot: I'm a simple chatbot built using Python, NLTK, and Flask.

User: Bye
Bot: Goodbye!

🧠 Chatbot using NLP
📌 Project Description

The Chatbot using NLP is a simple yet effective conversational AI system that interacts with users in natural language. The goal of this project is to build an intelligent assistant capable of answering basic queries using Natural Language Processing (NLP) techniques. It is developed in Python using NLTK for text preprocessing and Flask for web deployment.

This chatbot identifies user intent based on tokenized and lemmatized text inputs. It matches user queries with predefined intents from a JSON file and provides relevant responses. The model uses rule-based matching, making it lightweight and easy to understand for beginners exploring NLP concepts.

The user interacts through a clean web interface built with HTML, CSS, and JavaScript, which communicates with the Flask backend asynchronously using AJAX.

🎯 Goal

To build a simple rule-based Q&A chatbot that can engage in small talk and answer frequently asked questions using NLP techniques.

🧰 Technologies Used

Python – Programming language

NLTK (Natural Language Toolkit) – For text preprocessing (tokenization, lemmatization)

Flask – To deploy the chatbot as a web application

HTML, CSS, JavaScript – For creating the front-end chat interface

⚙️ Key Features

Processes and understands natural language input using NLTK

Matches user queries with predefined intents and responses

Provides instant replies in real-time through the Flask web app

Interactive chat-style user interface

Lightweight and easy to customize for any domain (e.g., FAQ bot, student helpdesk bot)

🧩 Workflow

User sends a message through the web interface.

Flask sends the input text to the backend model.

NLTK tokenizes and lemmatizes the text to normalize words.

The chatbot searches for matching patterns from the intents.json file.

If a match is found, a relevant response is selected; otherwise, a fallback message is shown.

The bot’s response is displayed instantly on the chat screen.

🚀 Outcome

The chatbot successfully simulates a human-like conversation by responding intelligently to user queries. It demonstrates core NLP concepts like text preprocessing, tokenization, and lemmatization, and shows how these can be integrated into a Flask-based web application.

📈 Skills Gained

Natural Language Processing (NLP)

Python (NLTK library)

Flask web deployment

JSON-based intent classification

Frontend-backend integration
