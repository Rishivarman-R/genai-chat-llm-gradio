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
import io
from IPython.display import Image, display, HTML
from PIL import Image
import base64 

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
```
```
#### Helper function
import requests, json

#Here we are going to call multiple endpoints!
def get_completion(inputs, parameters=None, ENDPOINT_URL=""):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }   
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))
```
```
#text-to-image
TTI_ENDPOINT = os.environ['HF_API_TTI_BASE']
#image-to-text
ITT_ENDPOINT = os.environ['HF_API_ITT_BASE']
```
```
#Bringing the functions from lessons 3 and 4!
def image_to_base64_str(pil_image):
    byte_arr = io.BytesIO()
    pil_image.save(byte_arr, format='PNG')
    byte_arr = byte_arr.getvalue()
    return str(base64.b64encode(byte_arr).decode('utf-8'))

def base64_to_pil(img_base64):
    base64_decoded = base64.b64decode(img_base64)
    byte_stream = io.BytesIO(base64_decoded)
    pil_image = Image.open(byte_stream)
    return pil_image

def captioner(image):
    base64_image = image_to_base64_str(image)
    result = get_completion(base64_image, None, ITT_ENDPOINT)
    return result[0]['generated_text']

def generate(prompt):
    output = get_completion(prompt, None, TTI_ENDPOINT)
    result_image = base64_to_pil(output)
    return result_image
```
```
import gradio as gr 
with gr.Blocks() as demo:
    gr.Markdown("# Describe-and-Generate game 🖍️")
    image_upload = gr.Image(label="Your first image",type="pil")
    btn_caption = gr.Button("Generate caption")
    caption = gr.Textbox(label="Generated caption")
    
    btn_caption.click(fn=captioner, inputs=[image_upload], outputs=[caption])

gr.close_all()
demo.launch(share=True, server_port=int(os.environ['PORT1']))
```
### OUTPUT:
<img width="1030" height="735" alt="Screenshot 2025-11-20 213621" src="https://github.com/user-attachments/assets/d8e1fcae-231a-4b92-9954-aeae4ee7a481" />

### RESULT:
The "Chat with LLM" application was successfully designed and deployed. The Gradio Blocks framework provided a user-friendly interface, ensuring seamless communication with the large language model.
