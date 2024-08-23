# BME Lab

This is part of the code for BME Lab at Mahidol University.
Lab II teaches 3rd year student where we aim to have them run 

## Lab II: Running Large Language Model

**Task 1: Running Open WebUI**

- Download [Ollama](https://ollama.com/) on your laptop, then run `ollama run llama3.1`

Here, you can run prompt Llama3.1 on your local machine. After running Ollama, you can connect with Open WebUI:

- Download [docker](https://www.docker.com/products/docker-desktop/) on your laptop
- Run [Open WebUI](https://github.com/open-webui/open-webui) using `docker` and goes to http://localhost:3000/ to prompt! You can check the section "If Ollama is on your computer, use this command: ..."

**Task 2: Connect Python with Ollama**

You can connect Python using [Langchain](https://www.langchain.com/) with Ollama.
Use [`00_Connect_Ollama_with_Langchain`](https://github.com/biodatlab/bme-labs/blob/main/notebooks/00_Connect_Ollama_with_Langchain.ipynb) to try it out after running Ollama.

```py
%%capture
!pip install langchain
!pip install langchain_community
```

```py
# Code from https://stackoverflow.com/a/78430197/3626961
from langchain_community.llms import Ollama
from langchain import PromptTemplate # Added

llm = Ollama(model="llama3.1", stop=["<|eot_id|>"]) # Added stop token

def get_model_response(user_prompt, system_prompt):
    # NOTE: No f string and no whitespace in curly braces
    template = """
        <|begin_of_text|>
        <|start_header_id|>system<|end_header_id|>
        {system_prompt}
        <|eot_id|>
        <|start_header_id|>user<|end_header_id|>
        {user_prompt}
        <|eot_id|>
        <|start_header_id|>assistant<|end_header_id|>
        """

    # Added prompt template
    prompt = PromptTemplate(
        input_variables=["system_prompt", "user_prompt"],
        template=template
    )
    
    # Modified invoking the model
    response = llm(prompt.format(system_prompt=system_prompt, user_prompt=user_prompt))
    
    return response

# Example
user_prompt = "What is 1 + 1?"
system_prompt = "You are a helpful assistant doing as the given prompt."
get_model_response(user_prompt, system_prompt)
```

**Task 3: Transcribe Youtube video and summarize the transcriptions**

- Use [`01_Thonburian_Whisper_Longform_Youtube`](https://github.com/biodatlab/bme-labs/blob/main/notebooks/01_Thonburian_Whisper_Longform_Youtube.ipynb) to transcribe the text from Youtube URL (select one URL) and [`02_Ollama_Summarize_Transcript`](https://github.com/biodatlab/bme-labs/blob/main/notebooks/02_Ollama_Summarize_Transcript.ipynb) to write a summarization prompt
- Your task is to write a prompt to summarize the text from your selected Youtube URL
