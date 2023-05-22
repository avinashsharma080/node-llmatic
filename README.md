<div align="center">

# LLMatic

<img alt="LLMatic Logo" width="300px" height="300px" src="/media/logo.png">

Use self-hosted LLMs with an OpenAI compatible API

</div>

<hr />

LLMatic can be used as a drop-in replacement for OpenAI's API (see the
supported endpoints). It uses [llama-node](https://github.com/Atome-FE/llama-node)
with [llama.cpp](https://github.com/ggerganov/llama.cpp) backend to run the models locally.

Supported endpoints:

- [x] /completions (stream and non-stream)
- [x] /chat/completions (stream and non-stream)
- [x] /embeddings
- [x] /models

This project is currently a work in progress. At this point, it's recommended
to use it only for ad-hoc development and testing.

## Motivation

The main motivation behind making LLMatic was to experiment with OpenAI's API
without worrying about the cost. I have seen other attempts at creating
OpenAI-Compatible APIs such as:

1. [FastChat](https://github.com/lm-sys/FastChat/blob/main/docs/openai_api.md)
2. [GPT4All Chat Server Mode](https://docs.gpt4all.io/gpt4all_chat.html#gpt4all-chat-server-mode)
3. [simpleAI](https://github.com/lhenault/simpleAI)

But I wanted a small, simple, and easy to extend implementation in Javascript based on the
[official OpenAI API specification](https://github.com/openai/openai-openapi/blob/master/openapi.yaml).

## How to use

Clone the repo and install the dependencies:

```bash
git clone https://github.com/fardjad/node-llmatic.git
cd node-llmatic
npm install
```

Download a
[compatible model](https://github.com/Atome-FE/llama-node#supported-models) and
place it in the `models` directory.

Set the `LLMATIC_MODEL` environment variable to the name of the model:

```bash
# the value must be the name of the model file with the .bin extension
export LLMATIC_MODEL_NAME=Wizard-Vicuna-13B-Uncensored.ggml.q5_1.bin
```

Run the server:

```bash
npm start
```

### Usage with [chatbot-ui](https://github.com/mckaywrigley/chatbot-ui)

Clone the repo and install the dependencies:

```bash
git clone https://github.com/mckaywrigley/chatbot-ui.git
cd chatbot-ui
npm install
```

Create a `.env.local` file:

```bash
cat <<EOF > .env.local

# For now, this is ignored by LLMatic
DEFAULT_MODEL=${LLMATIC_MODEL_NAME:-Ignored}

NEXT_PUBLIC_DEFAULT_SYSTEM_PROMPT=You are ChatGPT, a large language model trained by OpenAI. Follow the user's instructions carefully. Respond using markdown.

OPENAI_API_KEY=ANYTHING_WILL_DO
OPENAI_API_HOST=http://localhost:3000

GOOGLE_API_KEY=YOUR_API_KEY
GOOGLE_CSE_ID=YOUR_ENGINE_ID
EOF
```

Run the server:

```bash
npm run dev -- --port 3001
```

Demo:

![chatbot-ui Demo](/media/chatbot-ui.gif)

### Usage with [LangChain](https://langchain.com)

There are two examples of using LLMatic with LangChain in the
[`examples`](/examples) directory.

To run the Node.js example, first install the dependencies:

```bash
cd examples/node-langchain
npm install
```

Then run the main script:

```bash
npm start
```

<details>
  <summary>Expand this to see the sample output</summary>

```
[chain/start] [1:chain:llm_chain] Entering Chain run with input: {
  "humanInput": "Rememeber that this is a demo of LLMatic with LangChain.",
  "history": ""
}
[llm/start] [1:chain:llm_chain > 2:llm:openai] Entering LLM run with input: {
  "prompts": [
    "A chat between a curious user and an artificial intelligence assistant.\nThe assistant gives helpful, detailed, and polite answers to the user's questions.\n\n\nHuman: Rememeber that this is a demo of LLMatic with LangChain.\nAI:"
  ]
}
[llm/end] [1:chain:llm_chain > 2:llm:openai] [5.92s] Exiting LLM run with output: {
  "generations": [
    [
      {
        "text": " Yes, I understand. I am ready to assist you with your queries.",
        "generationInfo": {
          "finishReason": "stop",
          "logprobs": null
        }
      }
    ]
  ],
  "llmOutput": {
    "tokenUsage": {}
  }
}
[chain/end] [1:chain:llm_chain] [5.92s] Exiting Chain run with output: {
  "text": " Yes, I understand. I am ready to assist you with your queries."
}
[chain/start] [1:chain:llm_chain] Entering Chain run with input: {
  "humanInput": "What did I ask you to remember?",
  "history": "Human: Rememeber that this is a demo of LLMatic with LangChain.\nAI:  Yes, I understand. I am ready to assist you with your queries."
}
[llm/start] [1:chain:llm_chain > 2:llm:openai] Entering LLM run with input: {
  "prompts": [
    "A chat between a curious user and an artificial intelligence assistant.\nThe assistant gives helpful, detailed, and polite answers to the user's questions.\n\nHuman: Rememeber that this is a demo of LLMatic with LangChain.\nAI:  Yes, I understand. I am ready to assist you with your queries.\nHuman: What did I ask you to remember?\nAI:"
  ]
}
[llm/end] [1:chain:llm_chain > 2:llm:openai] [6.51s] Exiting LLM run with output: {
  "generations": [
    [
      {
        "text": " You asked me to remember that this is a demo of LLMatic with LangChain.",
        "generationInfo": {
          "finishReason": "stop",
          "logprobs": null
        }
      }
    ]
  ],
  "llmOutput": {
    "tokenUsage": {}
  }
}
[chain/end] [1:chain:llm_chain] [6.51s] Exiting Chain run with output: {
  "text": " You asked me to remember that this is a demo of LLMatic with LangChain."
}
```

</details>

<hr>

To run the Python example, first install the dependencies:

```bash
cd examples/python-langchain
pip3 install -r requirements.txt
```

Then run the main script:

```bash
python3 main.py
```

<details>
  <summary>Expand this to see the sample output</summary>

```
> Entering new LLMChain chain...
Prompt after formatting:
A chat between a curious user and an artificial intelligence assistant.
The assistant gives helpful, detailed, and polite answers to the user's questions.


Human: Rememeber that this is a demo of LLMatic with LangChain.
AI:

> Finished chain.
 Yes, I understand. I am ready to assist you with your queries.


> Entering new LLMChain chain...
Prompt after formatting:
A chat between a curious user and an artificial intelligence assistant.
The assistant gives helpful, detailed, and polite answers to the user's questions.

Human: Rememeber that this is a demo of LLMatic with LangChain.
AI:  Yes, I understand. I am ready to assist you with your queries.
Human: What did I ask you to remember?
AI:

> Finished chain.
 You asked me to remember that this is a demo of LLMatic with LangChain.
```

</details>
