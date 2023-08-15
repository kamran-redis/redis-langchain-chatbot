**Only tested on Mac M1 with metal support**

# Build an Ecommerce Chatbot with Redis, LangChain,Hugging Face and LLama2

Modified `https://github.com/kamran-redis/redis-langchain-chatbot.git` privacy focused to work offline. Changed to use  llama-2 13b and Hugging face local embeddings.  If you have less RAM you can also use llama-2 7b.  

* You can use this totally offline
* A lot of innovation is happening on llama-2 
* Privacy focused

 ---- 
>*Powered by [Redis](https://redis.io), [LangChain](https://python.langchain.com/en/latest/) [llama-2](https://ai.meta.com/llama/) and [HuggingFace](https://huggingface.co/blog/getting-started-with-embeddings)*

In this tutorial we build a conversational retail shopping assistant that helps customers find items of interest that are buried in a product catalog. Our chatbot will take user input, find relevant products, and present the information in a friendly and detailed manner.

The source code here goes along with [this Redis blog post](https://redis.com/blog/build-ecommerce-chatbot-with-redis/). Try various prompt-engineering techniques to improve on this prototype for your use case!
----

## Getting Started
* Clone the repo
* Probalby you will have most trouble installing [python bindings](https://github.com/abetlen/llama-cpp-python) for [llama.cpp](https://github.com/ggerganov/llama.cpp)
* You will need redis stack. I just deployed redis in docker listening on local port 6379
* Download qunatized llama-2 from huggingface. 
For 32 GB RAM use  [llama-2-13b.ggmlv3.q6_K.bin](https://huggingface.co/TheBloke/Llama-2-13B-GGML/blob/main/llama-2-13b.ggmlv3.q6_K.bin) for 16 GB RAM  use [llama-2-7b.ggmlv3.q6_K.bin](https://huggingface.co/TheBloke/Llama-2-7B-GGML/blob/main/llama-2-7b.ggmlv3.q6_K.bin) the results were awful with 7b.

If you are new to Jupyter etc some commands below

```
python3.10 -m venv venv
. ./venv/bin/activate
pip install notebook
jupyter notebook
```

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
