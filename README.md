**Only tested on Mac M1 with metal support**

# Build an Ecommerce Chatbot with Redis, LangChain,Hugging Face and LLama2

Modified  `https://github.com/kamran-redis/redis-langchain-chatbot.git` to work with local LLM model and HuggingFace local embeddings.  Once setup the sample can run without internet connectivity. 

* Run the demo offline
* A lot of innovation is happening on llama-2 integration
* Privacy focused

----
>*Powered by [Redis](https://redis.io), [LangChain](https://python.langchain.com/en/latest/) [llama-2](https://ai.meta.com/llama/) and [HuggingFace](https://huggingface.co/blog/getting-started-with-embeddings)*

In this tutorial we build a conversational retail shopping assistant that helps customers find items of interest that are buried in a product catalog. Our chatbot will take user input, find relevant products, and present the information in a friendly and detailed manner.

The source code here goes along with [this Redis blog post](https://redis.com/blog/build-ecommerce-chatbot-with-redis/). Try various prompt-engineering techniques to improve on this prototype for your use case!

----


## Getting Started

There are 2 jupyter notebooks
 * **redis-langchain-ecommerce-chatbot** Uses openApi compatible service. There are a few options available to run openapi compatible 

 * redis-langchain-llma2-ecommerce-chatbot** uses [python bindings](https://github.com/abetlen/llama-cpp-python) for [llama.cpp](https://github.com/ggerganov/llama.cpp)

* You will need other than the python, jupyter 
	
	* Redis Stack `docker run -it --name redis-stack -p 6379:6379 -p 8001:8001  --rm redis/redis-stack:7.2.0-v0`
	* LLM model
	* Inference Engine
	
	If you are new to Jupyter etc some commands below
	
	```
  python3.10 -m venv venv
	. ./venv/bin/activate
  pip install notebook
  jupyter notebook
	```
	
  ### LLM Model
  
  Meta have releases  [Llama-2](https://ai.meta.com/llama/) model. Running the models require [GPU and memory](https://finbarr.ca/how-is-llama-cpp-possible/) and APPL M1/M2 for consumers is the most convenient option to run a local model. You can download the models from HuggingFace. I used the follwong models. 
	
  
  | Model                                                        | Notes                                                        |      |
  | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
  | [llama-2-7b.ggmlv3.q6_K.bin](https://huggingface.co/TheBloke/Llama-2-7B-GGML/blob/main/llama-2-7b.ggmlv3.q6_K.bin) | If you do not have a powerful GPU or 8GB RAM (M1 Max performance ~30 tokens/second). This is the smalles llama-2 model |      |
  | [llama-2-13b.ggmlv3.q6_K.bin](https://huggingface.co/TheBloke/Llama-2-13B-GGML/blob/main/llama-2-13b.ggmlv3.q6_K.bin) | 16GB RAM and GPU (M1 Max performance ~16  tokens/second)     |      |
  | [llama-2-70b-chat.ggmlv3.q4_K_S.bin](https://huggingface.co/TheBloke/Llama-2-70B-Chat-GGML/blob/main/llama-2-70b-chat.ggmlv3.q4_K_S.bin) | 64GB RAM and GPU( M1 Max performance ~5 tokens/second). Always requires  `gqa 8` flag |      |
  
  ### Inference Engine
  
  * [llama2.cpp](https://github.com/ggerganov/llama.cpp). The inference engine used by every  other tool. To Run as open api server see [server example](https://github.com/ggerganov/llama.cpp/blob/master/examples/server/README.md). I used the following commands to compile and run the server
  ```
  LLAMA_METAL=1 make
  
  # Run the server
  ./server  -m ~/.cache/lm-studio/models/thebloke/Llama-2-70B-Chat-GGML/llama-2-70b-chat.ggmlv3.q4_K_S.bin -ngl 1 -gqa 8   --ctx-size 4000  -v
  
  
  #In a separte temrinal run the open api server
  python3.10 -m venv venv
  . ./venv/bin/activate
  pip install flask
  pip install requests
  python examples/server/api_like_OAI.py
  
  # and you can test by
   curl http://localhost:8081/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {  "role": "user", "content":"Tell me about the capabilities of redis enterprise and when to use it" }
    ],
    "temperature": 0.2,
    "max_tokens": 100,
    "stream": false
  }'
  ```
  
* [LM Studio](https://lmstudio.ai/) Provides  Chat UI and  OpenAI compatible local server.

* **llama.cpp** [python bindings](https://github.com/abetlen/llama-cpp-python#web-server) also provide an openapi server but seems to be not well tested.

## Versions as of testing
```
Python 3.10   
langchain=0.0.264
redis=4.5.3  
numpy=1.25.2
pandas=2.0.3
gdown=4.7.1
transformers=4.31.0  
sentence_transformers=2.2.2
```
