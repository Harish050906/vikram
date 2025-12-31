🚀 Quickstart Guide
You have two options to get started: the Hosted Platform (managed for you) or Self-Hosted (run it yourself).

1. Installation
If you are going the self-hosted (Open Source) route, install the SDK via your package manager:

Python:

Bash

pip install mem0ai
Node/JS:

Bash

npm install mem0ai
2. Basic Implementation (Python Example)
Here is a practical script showing how to chat with an AI that "remembers." This script initializes memory, searches for past context, generates a response, and saves the new interaction.

Python

from openai import OpenAI
from mem0 import Memory

# Initialize clients
openai_client = OpenAI()
memory = Memory()

def chat_with_memories(message: str, user_id: str = "default_user") -> str:
    # 1. Retrieve relevant memories based on the user's message
    relevant_memories = memory.search(query=message, user_id=user_id, limit=3)
    
    # Format memories into a string for the LLM
    memories_str = "\n".join(f"- {entry['memory']}" for entry in relevant_memories["results"])

    # 2. Generate Assistant response using the retrieved memories
    system_prompt = f"You are a helpful AI. Answer based on query and memories.\nUser Memories:\n{memories_str}"
    messages = [{"role": "system", "content": system_prompt}, {"role": "user", "content": message}]
    
    response = openai_client.chat.completions.create(model="gpt-4o-mini", messages=messages)
    assistant_response = response.choices[0].message.content

    # 3. Create new memories from this interaction
    messages.append({"role": "assistant", "content": assistant_response})
    memory.add(messages, user_id=user_id)

    return assistant_response

# Simple loop to chat
def main():
    print("Chat with AI (type 'exit' to quit)")
    while True:
        user_input = input("You: ").strip()
        if user_input.lower() == 'exit':
            print("Goodbye!")
            break
        print(f"AI: {chat_with_memories(user_input)}")

if __name__ == "__main__":
    main()
🔗 Integrations
Mem0 plays nicely with the tools you likely already use:

Langgraph: Perfect for building stateful customer bots (View Guide).

CrewAI: Helps tailor autonomous agent outputs based on historical data (View Example).

Browser Extension: A Chrome extension to store memories across ChatGPT, Perplexity, and Claude.

📚 Resources & Support
Documentation: docs.mem0.ai

Community: Join the discussion on Discord or Twitter.
