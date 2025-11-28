## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework

### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:
The objective is to develop an intuitive and interactive interface that enables users to communicate with a large language model (LLM) effectively. The application should handle user inputs, generate responses from the LLM, and display the results in real-time.
### DESIGN STEPS:

#### STEP 1:Set Up the Environment
+ Install the necessary libraries, such as gradio and transformers.
+ Choose an appropriate LLM (e.g., OpenAI's GPT, Hugging Face's GPT-2/3.5/4).
+ Ensure GPU support for faster inference, if required.
#### STEP 2:Create the Gradio Blocks Interface
+ Use gr.Blocks to design a modular and interactive layout.
+ Define the components such as Textbox for user input, Chatbot for displaying messages, and Button for interaction.

#### STEP 3: Integrate the LLM with the Interface
+ Test the application locally to ensure smooth functionality.
+ Deploy the app using platforms like Hugging Face Spaces, Streamlit Cloud, or a custom server.

### PROGRAM:
## Name: Rishivarman R
## Reg No.: 212224100050
```

import os
from dotenv import load_dotenv, find_dotenv
import gradio as gr
from text_generation import Client

load_dotenv(find_dotenv())

HF_API_KEY = os.environ["HF_API_KEY"]
FALCON_ENDPOINT = os.environ["HF_API_FALCOM_BASE"]


client = Client(
    FALCON_ENDPOINT,
    headers={"Authorization": f"Basic {HF_API_KEY}"},
    timeout=120
)


def format_chat_prompt(message, chat_history):
    prompt = ""
    for user_msg, bot_msg in chat_history:
        prompt += f"User: {user_msg}\nAssistant: {bot_msg}\n"
    prompt += f"User: {message}\nAssistant:"
    return prompt

def respond(message, chat_history):
    prompt = format_chat_prompt(message, chat_history)

    # Generate using Falcon
    bot_message = client.generate(
        prompt,
        max_new_tokens=512,
        stop_sequences=["User:", "<|endoftext|>"]
    ).generated_text

    chat_history.append((message, bot_message))
    return "", chat_history

with gr.Blocks() as demo:
    gr.Markdown("## 🦅 Falcon LLM — Chatbot (Jupyter Notebook Version)")

    chatbot = gr.Chatbot(height=350)
    msg = gr.Textbox(label="Type your message...")
    submit = gr.Button("Send")
    clear = gr.ClearButton([msg, chatbot])

    submit.click(respond, [msg, chatbot], [msg, chatbot])
    msg.submit(respond, [msg, chatbot], [msg, chatbot])

demo.launch(share=False)   # share=True only if you want a public link
```
### OUTPUT:

<img width="1216" height="849" alt="Screenshot 2025-11-28 101428" src="https://github.com/user-attachments/assets/faeef50b-50ca-4ddc-9d49-12bf15cac38e" />

### RESULT:
The "Chat with LLM" application was successfully designed and deployed. The Gradio Blocks framework provided a user-friendly interface, ensuring seamless communication with the large language model.
