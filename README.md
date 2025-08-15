# Advanced Rule-Based Chatbot with Multi-Intent & Chat History
from datetime import datetime
import random

# File to store chat history
HISTORY_FILE = "chat_history.txt"

# Save chat messages
def save_history(speaker, message):
    with open(HISTORY_FILE, "a", encoding="utf-8") as file:
        file.write(f"[{datetime.now().strftime('%Y-%m-%d %H:%M:%S')}] {speaker}: {message}\n")

# Predefined responses
responses = {
    ("hi", "hello", "hey", "good morning", "good afternoon", "good evening"):
        "Hello! How can I help you today?",
    ("your name", "who are you"):
        "I’m ChatBot, your friendly virtual assistant.",
    ("how are you", "how's it going", "how do you do"):
        "I'm doing great, thanks for asking! How about you?",
    ("help", "what can you do", "features"):
        "I can chat with you, tell you the time/date, share jokes, and answer basic questions.",
    ("weather",):
        "I can't check live weather yet, but I hope it's sunny where you are! ☀️",
    ("joke", "funny"):
        lambda: random.choice([
            "Why don’t skeletons fight each other? They don’t have the guts.",
            "I told my computer I needed a break, and it froze!",
            "Why was the math book sad? It had too many problems."
        ]),
    ("quote", "motivate me"):
        lambda: random.choice([
            "Believe you can and you're halfway there.",
            "Do something today that your future self will thank you for.",
            "Push yourself, because no one else is going to do it for you."
        ]),
    ("creator", "who made you"):
        "I was created by a Python developer for an internship task.",
    ("age", "how old are you"):
        "I don’t have an age, but I’m always up-to-date!",
    ("python", "programming"):
        "Python is a versatile programming language, great for AI, data, and web development."
}

print("Hello! I'm ChatBot. Type 'exit' to end the chat.\n")

while True:
    user_input = input("You: ").lower().strip()
    save_history("User", user_input)  # Save user message

    if user_input == "exit":
        print("ChatBot: Goodbye! Have a great day!")
        save_history("ChatBot", "Goodbye! Have a great day!")
        break

    responses_found = []  # Store all matches

    # Special commands
    if "time" in user_input:
        responses_found.append(f"The current time is {datetime.now().strftime('%H:%M:%S')}")
    if "date" in user_input:
        responses_found.append(f"Today's date is {datetime.now().strftime('%Y-%m-%d')}")

    # Keyword matching for other responses
    for keys, reply in responses.items():
        if any(k in user_input for k in keys):
            if callable(reply):
                responses_found.append(reply())
            else:
                responses_found.append(reply)

    # If no match found
    if not responses_found:
        responses_found.append("Sorry, I didn't understand that. Can you try rephrasing?")

    # Print and save all responses
    for res in responses_found:
        print("ChatBot:", res)
        save_history("ChatBot", res)
