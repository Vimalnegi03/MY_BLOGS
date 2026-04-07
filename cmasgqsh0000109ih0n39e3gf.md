---
title: "🤖 Agents and Tokenizer in Generative AI (GenAI) — Explained Simply"
datePublished: 2025-05-17T16:50:10.212Z
cuid: cmasgqsh0000109ih0n39e3gf
slug: agents-and-tokenizer-in-generative-ai-genai-explained-simply
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/nGoCBxiaRO0/upload/27024dd47d58e2cf93920e2b30435570.jpeg
tags: huggingface, chatgpt, finetuning, gemini, genai, ollama, chaiaurcode, chaicode, deepseek

---

## 🤖 LLMs vs. Agents: Understanding the Difference (with a Simple Analogy)

In the world of AI, there's a lot of buzz around **Large Language Models (LLMs)** and **Agents**. But what's the real difference between the two?

Let’s break it down in simple terms. 🧠💡

## 🧠 What is an LLM?

A **Large Language Model** (LLM), like GPT-4 or Claude, is essentially a powerful brain that has been trained on massive amounts of text data. It can:

* Understand natural language
    
* Generate human-like responses
    
* Summarize, translate, and even write code
    

But here's the **limitation**:

> **LLMs do not have memory or the ability to access real-time data on their own.**

### Think of it like this:

> ❝An LLM is like a brain with no body. It knows a lot, but it can’t act on anything in the real world.❞

---

## 🦾 What is an Agent?

An **Agent** is what happens when you give that brain a **body**.

An **AI Agent** is built on top of an LLM and is designed to:

* **Interact with real-time data** (e.g., check the weather, stock prices)
    
* **Use tools/APIs**
    
* **Remember previous interactions**
    
* **Take actions** based on goals
    

### So in short:

> ❝An Agent is an LLM with tools, memory, and the ability to take actions.❞

---

## 🧩 Analogy: Brain vs. Assistant

* **LLM = Brain**: Smart, well-read, but passive. Can answer questions but can't "do" anything.
    
* **Agent = Assistant with a Brain**: Not only smart, but can also check your emails, book appointments, and tell you the weather—**in real-time**.
    

---

## 🆚 Key Differences

| Feature | LLM | Agent |
| --- | --- | --- |
| Access to real-time data | ❌ No | ✅ Yes |
| Has memory | ❌ No (unless extended) | ✅ Yes |
| Uses tools / APIs | ❌ No | ✅ Yes |
| Acts autonomously | ❌ No | ✅ Yes |
| Example | ChatGPT answering questions | AutoGPT planning and executing tasks |

---

## 💡 Why Does This Matter?

Understanding the distinction helps in:

* Building smarter applications
    
* Choosing the right tool for the job
    
* Innovating in how AI is used (think AI assistants, automated workflows, etc.)
    

## Example-:

Ask LLM to provide real time weather data you will note that it will ask you to make a google search or check any weather related website

Where as an agent will provide you the details

## Creating weather agent-:

```python
from openai import OpenAI
from dotenv import load_dotenv
load_dotenv()
import os
import requests
import json
api_key=os.getenv("GOOGLE_GEMINI_API_KEY")
client=OpenAI(api_key=api_key,
              base_url="https://generativelanguage.googleapis.com/v1beta/openai/")
messages=[]
def get_weather(city:str):
    url=f'https://wttr.in/{city}?format=%C+%t'
    response=requests.get(url)
    if response.status_code==200:
       return f"the weather in {city} is {response.text}"
    return "Something went wrong"

available_tools={
    'get_weather':{
        "fn":get_weather,
        "description":"Takes a city name as an input and return current weather of the city"
    }
}
System_prompt=f"""You are a powerful ai agent who is specialized in resolving user query.
 You work on start,plan,action,observe mode.
For the given user query and available tools plan step by step execution,based on the planning select the relevant tools from the avilable 
tools and based on the tool selction you perfom an action to call the tool wait for observation and based on observation from the tool call resolve the user query

Rules-:
- Follow the output JSON format.
- Always Perform one step at a time and wait for next input.
- Carefully analyse the user query

Output JSON Format:
{{
    "step":"string",
"content":"string",
"function":"the name of function if the step is action",
"input":"the input parameter for the function"
}}
Available Tools:
-get_weather:Takes city name as input returns the current weather for the city
Example:
User Query:what is the weather of new york?
Output:{{"step":"plan","content":"the user is interested in weather data of new york}}
Output:{{"step""plan","content":"From the available tools I should call get_weather"}}
Output:{{"step":"action","function":"get_weather","input":"new york"}}
Output:{{"step":"observe","output":"12 degree celsius"}}
Output:{{"step":"output","The weather of new york is 12 degree celsius"}}

"""
messages.append({
            'role':"system",'content':System_prompt})

user_query=input("Enter your query >")
messages.append({"role":"user","content":user_query})
outputs=[]
while True:
    response=client.chat.completions.create(
        model='gemini-2.0-flash',
        messages=messages,
        response_format={"type":"json_object"},
        
    )
    output=response.choices[0].message.content
    messages.append({"role":"assistant",'content':json.dumps(output)})
    extracted_output=json.loads(output)
    if(extracted_output.get("step")=='plan'):
     print(extracted_output.get("content"))
     continue
    if(extracted_output.get("step")=='action'):
       tool_name=extracted_output.get("function") #get weather
       tool_input=extracted_output.get("input") #city name
       if(available_tools.get(tool_name,False).get("fn")(tool_input))!=False:
          output=available_tools[tool_name].get("fn")(tool_input)
          messages.append({"role":"assistant","content":json.dumps({"step":"observe","output":output})})

    if(extracted_output.get("step")=="output"):
       print(extracted_output.get("content"))
       break
```

This is how you can create a basic weather agent

## Creating mini cursor that even support voice assistant-:

```python
from openai import OpenAI
from dotenv import load_dotenv
load_dotenv()
import os
import requests
import json
import platform
api_key=os.getenv("GOOGLE_GEMINI_API_KEY")
client=OpenAI(api_key=api_key,
              base_url="https://generativelanguage.googleapis.com/v1beta/openai/")


import speech_recognition as sr
import time

recognizer = sr.Recognizer()

with sr.Microphone() as source:
    print("🔧 Calibrating for background noise...")
    recognizer.adjust_for_ambient_noise(source, duration=2)
    print("✅ Calibration done.")

    print("🎤 You can speak after this countdown:")
    for i in range(3, 0, -1):
        print(i)
        time.sleep(1)

    print("🎙️ Listening now (speak up to 30 seconds)...")

    try:
        audio = recognizer.listen(
            source,
            timeout=60,               # wait up to 60s for user to start talking
            phrase_time_limit=30      # let them speak up to 30s max
        )

        print("🔍 Recognizing...")
        text = recognizer.recognize_google(audio)
        print("✅ You said:", text)

    except sr.WaitTimeoutError:
        print("⌛ Timeout: No speech detected within the time window.")
    except sr.UnknownValueError:
        print("❌ Could not understand the audio.")
    except sr.RequestError as e:
        print(f"❌ Recognition error: {e}")


messages=[]


def run_command(command:str):
   print(platform.system())
   os.system(command=command)
available_tools={
   
    'run_command':{
       "fn":run_command,
       "description":"takes a command from user and execute that"
    }

}
System_prompt=f"""You are a powerful ai agent who is specialized in resolving user query.
 You work on start,plan,action,observe mode.
For the given user query and available tools plan step by step execution,based on the planning select the relevant tools from the avilable 
tools and based on the tool selction you perfom an action to call the tool wait for observation and based on observation from the tool call resolve the user query

Rules-:
- Follow the output JSON format.
- Always Perform one step at a time and wait for next input.
- Carefully analyse the user query

Output JSON Format:
{{
    "step":"string",
"content":"string",
"function":"the name of function if the step is action",
"input":"the input parameter for the function"
}}
Available Tools:
-run_command:Takes a command and execute that command on windows or linux system according to a system configuration
Example:
User Query:what is the weather of new york?
Output:{{"step":"plan","content":"the user is interested in weather data of new york}}
Output:{{"step""plan","content":"From the available tools I should call get_weather"}}
Output:{{"step":"action","function":"get_weather","input":"new york"}}
Output:{{"step":"observe","output":"12 degree celsius"}}
Output:{{"step":"output","The weather of new york is 12 degree celsius"}}

"""
messages.append({
            'role':"system",'content':System_prompt})

user_query=text
messages.append({"role":"user","content":user_query})
outputs=[]
while True:
    response=client.chat.completions.create(
        model='gemini-2.0-flash',
        messages=messages,
        response_format={"type":"json_object"},
        
    )
    output=response.choices[0].message.content
    messages.append({"role":"assistant",'content':json.dumps(output)})
    extracted_output=json.loads(output)
    if(extracted_output.get("step")=='plan'):
     print(extracted_output.get("content"))
     continue
    if(extracted_output.get("step")=='action'):
       tool_name=extracted_output.get("function") #get weather
       tool_input=extracted_output.get("input") #city name
       if(available_tools.get(tool_name,False).get("fn")(tool_input))!=False:
          output=available_tools[tool_name].get("fn")(tool_input)
          messages.append({"role":"assistant","content":json.dumps({"step":"observe","output":output})})

    if(extracted_output.get("step")=="output"):
       print(extracted_output.get("content"))
       break
```

## How to run an LLM locally-:

## 🔧 **1\. Key Differences: OpenAI vs. Open-Source LLMs**

| Feature | OpenAI (like GPT) | Open-Source LLMs (e.g., LLaMA, Mistral) |
| --- | --- | --- |
| Hosting | Cloud/API only | Local or server |
| Fine-tuning | ❌ Not available | ✅ Fully supported |
| Inference | ✅ Supported via API | ✅ Supported locally and via API |
| Privacy | ❌ Depends on API | ✅ Full local control |
| Cost | 💸 Pay-per-use | ✅ Free (one-time compute cost) |

---

## 🚀 **2\. What is Ollama?**

[**Ollama**](https://ollama.com) is a CLI tool and runtime that lets you run LLMs (like LLaMA2, Mistral, Gemma, etc.) *locally on your machine* with just a few commands.

### ✅ Benefits of Ollama:

* Runs entirely on your CPU or GPU
    
* Easy to install and use
    
* Supports many open-source models
    
* Lightweight models like `llama2:7b` or `mistral:7b` work well even on mid-range laptops
    

### Running Ollama using Docker commands

```yaml
services:
  ollama:
   image: ollama/ollama:latest
   ports:
     - '11434:11434'
   volumes:
     - models:/root/.ollama/models

volumes:
  models:     
  
```

> ```plaintext
> docker compose up
> ```
> 
> Using it in code

```python
from fastapi import FastAPI
from ollama import Client
from fastapi import Body
app=FastAPI()
client=Client(
    host='http://localhost:11434'
) #our ollama will run here
client.pull('gemma3:1b') #pulled this model from ollama
@app.post("/chat")
def chat(message:str=Body(...,description="Chat Message")):
    response=client.chat(model='gemma3:1b',messages=[
        {"role":"user","content":message}
    ])
    return response['message']['content']
```

download package uvicorn-<mark>pip install uvicorn</mark>

```plaintext
 uvicorn ollama_api:app --port 8000
```

### 🔧 Basics of Fine-Tuning Transformers: Tokenizer and AutoModelForCausalLM Explained

Fine-tuning a transformer model sounds complex, but if you break it down into its key components, it's totally manageable—even exciting!

In this post, we'll cover the **core concepts** of fine-tuning a language model using **Hugging Face Transformers**, focusing on:

* What is a `Tokenizer`?
    
* What is `AutoModelForCausalLM`?
    
* How they work together during fine-tuning
    

---

## 📚 What is Fine-Tuning in NLP?

Fine-tuning is the process of **taking a pre-trained model** (like GPT-2, LLaMA, or Falcon) and training it further on your **specific dataset** so it learns the patterns and context related to your task.

This could be:

* A chatbot for your domain
    
* A code generator
    
* A language-specific assistant
    
* Custom text completions
    

---

## 🔤 Tokenizer: Your Input Translator

Before we feed data to a model, we need to **convert text into numbers** (tokens). That’s what a tokenizer does.

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("gpt2")
inputs = tokenizer("Hello, how are you?", return_tensors="pt")
```

### ✅ Why It’s Important:

* Ensures input is in the right format for the model
    
* Handles special tokens (like padding, EOS, BOS)
    
* Must match the model architecture (e.g., GPT2 tokenizer for GPT2 model)
    

> **Think of the tokenizer as a translator** that converts human text into a format the model understand

## 🧠 AutoModelForCausalLM: Predict the Next Word

Once text is tokenized, it’s fed into a **causal language model**, which is a type of model that predicts the **next token** in a sequence.

```python
from transformers import AutoModelForCausalLM

model = AutoModelForCausalLM.from_pretrained("gpt2")
outputs = model(**inputs)
```

* **Causal LM = "left to right" prediction**
    
* Used for **text generation tasks**
    
* Common models: GPT2, LLaMA, Mistral, Falcon, etc.
    

***Complete code-:***

```python
!pip install transformers
import os
os.environ["HF_TOKEN"]="Your hugging face token"
model_name="google/gemma-3-1b-it"
from transformers import AutoTokenizer
tokenizer=AutoTokenizer.from_pretrained(model_name) # generates token for a model or pulls token.json from hugging face
print(tokenizer("hello how are you"))
print(tokenizer.get_vocab())
input_tokens=tokenizer("hello how are you")["input_ids"]
from transformers import AutoModelForCausalLM #This is basically next token predictor
import torch
model=AutoModelForCausalLM.from_pretrained(model_name,torch_dtype=torch.bfloat16)
from transformers import pipeline
genPipeLine=pipeline("text-generation",model=model,tokenizer=tokenizer)
genPipeLine("hey there")
```

## Conclusion-:

So this was the basic follow for such content and feel free to comment if you have any doubt.