# Cheatsheets for ML Engineer

## Table of Contents
- [Section 1: Python](#1)
  - [Section 1.1 Installation](#1_1)
- [Section 2: Virtual Environment](#2)
  - [Section 2.1 Installation](#2)
- [Section 3: PIP Usage](#3)
- [Section 4: Docker Usage](#4)
- [Section 5: Open-WebUI on Ollama](#5)
- [Section 6: Qdrant Database](#6)
- [Section 7: Jupyter Notebook on Remote](#7)
- - [Section 7.1: Automation by scripts](#7_1)
- - [Section 7.2: Avoid Notebook Token](#7_2)
- [Section 8: Huggingface](#8)
- [Section 9: GPU Inspection](#9)
- [Section 10: Formatting & Beutification](#10)
- [Section 11: Langchain with Ollama](#11)
- [Section 12: Package Dependency Resolution](#12)
  
## Section 1: Python Installation
<a id="1"></a>
As an example, install Python 3.11 on Ubuntu from the command line, follow these steps:

### Add ssl cert

```bash
sudo apt-get install -y ca-certificates
sudo cp local-ca.crt /usr/local/share/ca-certificates
sudo update-ca-certificates
```

### Update the system packages:

```bash
sudo apt update && sudo apt upgrade -y
```

### Add the deadsnakes PPA (if Python 3.11 is not available in your version of Ubuntu):
```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
```

### Install Python:
<a id="1_1"></a>

```bash
sudo apt install python3.11
```

### Verify the installation:
```
python3.11 --version
```

### (Optional) If you want python or python3 to point to Python 3.11, update the alternatives:
```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 1
```

### To select Python 3.11 as the default:
```
sudo update-alternatives --config python
sudo update-alternatives --config python3
```

## Section 2: Virtual Environment
<a id="2"></a>

### Creating a Virtual Environment with a Specific Python Version

To create a virtual environment with a specific version of Python, use the following command:
```
sudo apt install python3.11-venv python3.11-dev build-essential
python3.11 -m venv myenv
```

### Activating the Virtual Environment

```bash
source myenv/bin/activate
```

Once activated, the virtual environment's name will appear in your terminal prompt.

### Add ssl certificates

```bash
pip install certifi
```

```bash
python -c "import certifi; print(certifi.where())"
```

```
openssl x509 -in $your_ca.crt -text >> $(python -c "import certifi; print(certifi.where())")
```

```bash
sudo update-ca-certificates
```

Sometimes, when pip installation yields SSLError you need to use `--trusted-host`, like,

```
python -m pip install --trusted-host github.com
```

Sometimes, you need to set global cert fro pip
```
pip config set global.cert yourcert.crt
```

Sometimes, following code works from python:

```
import ssl
import certifi
ssl._create_default_https_context = lambda: ssl.create_default_context(cafile=certifi.where())
```

## Deactivating/Deleting a Virtual Environment
To remove a virtual environment, deactivate it and delete the directory:

```bash
deactivate
```
```
rm -rf myenv
```

## Creating new virtual env from old

This assures previous packages are included,

```bash
cp -rL ~/myenv/ ./my_newenv #OR
cp -RL ~/myenv/ ./my_newenv
```

## Section 3: PIP Usage
<a id="3"></a>
Once the virtual environment is active, install packages using pip:

```
pip install transformers
```

### Install from a Git Repository
```bash
pip install git+https://github.com/username/repo.git
```

For a specific branch/ commit:
```
pip install git+https://github.com/username/repo.git@branch_name
pip install git+https://github.com/username/repo.git@commit_hash
```

### Install with a trustes host/certificate
```
pip install package_name --cert path_to_cert.pem
pip install package_name --trusted-host pypi.org --trusted-host pypi.python.org --trusted-host files.pythonhosted.org
```

### Install with specific python version
```
python3.x -m pip install package_name
```

### Install without dependencies
```
pip install package_name --no-deps
```

### Freezing and Installing from requirements
```bash
pip list
pip freeze > requirements.txt
pip install -r requirements.txt
```

## Section 4: Docker Usage
<a id="4"></a>

### Installation Docker-Desktop

``` sudo apt-get update ```

Download docker-desktop and install with, 

``` sudo apt-get install ./docker-desktop-amd64.deb ```

### Installation of Docker
Try to install following the official link,

```bash
https://docs.docker.com/engine/install/ubuntu/
```

### Working with Docker/ Docker-Desktop

``` sudo docker pull qdrant/qdrant ```

``` sudo docker run -d --name qdrant -p 6333:6333 qdrant/qdrant ```

### Start/Attach/Stop

``` sudo docker start qdrant ```

``` sudo docker attach qdrant ```

``` sudo docker stop qdrant ```

## Section 5: Open-WebUI on Ollama
<a id="5"></a>

### Download info

1. Install ollama: https://github.com/ollama/ollama/blob/main/docs/linux.md
2. Install open-webUI: https://docs.openwebui.com/getting-started/

### Open-WebUI
Pull the Open WebUI Image
```
docker pull ghcr.io/open-webui/open-webui:main
```
Run the container with default settings. This command includes a volume mapping to ensure persistent data storage.
```
docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main
```
Optional, with GPU
```
docker run -d -p 3000:8080 --gpus all -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:cuda
```

Start open-webui in localhost (accessible from LAN)

```open-webui serve```

### Ollama

To install Ollama, run the following command:
```
curl -fsSL https://ollama.com/install.sh | sh
```

To make ollama publicly accessible by IP
```
sudo systemctl edit ollama.service
```
Then add following line:
```
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"

```
Save and reload the daemon using
```
sudo systemctl daemon-reload 
sudo systemctl restart ollama
```

Stop and kill ollama service
```
pgrep ollama
pkill ollama
systemctl stop ollama.service
```

Start ollama server
```
ollama serve
OLLAMA_HOST=IP:11434 ollama serve
```

Start ollama server in local PC (accessible from LAN)
```
OLLAMA_HOST=127.0.0.1:11434 ollama serve
```

## Section 6:Qdrant Vector DB
<a id="6"></a>

### Qdrant Setup:
**Qdrant site:** https://qdrant.tech/documentation/quick-start/
```
docker pull qdrant/qdrant

#default
sudo docker run -d --name qdrant -p 6333:6333 qdrant/qdrant

#with db location mapping
docker run -p 6333:6333 -p 6334:6334 \
    -v $(pwd)/qdrant_storage:/qdrant/storage:z \
    qdrant/qdrant

#starting existing
docker start qdrant

#removing existing
docker rm -f qdrant
```

### Qdrant on Python Client:

```
from qdrant_client import QdrantClient
client = QdrantClient(url="http://localhost:6333")
```

```
from qdrant_client.models import Distance, VectorParams

client.create_collection(
    collection_name="test_collection",
    vectors_config=VectorParams(size=4, distance=Distance.DOT),
)
```

```
search_result = client.query_points(
    collection_name="test_collection",
    query=[0.2, 0.1, 0.9, 0.7],
    with_payload=False,
    limit=3
).points

print(search_result)
```

## Section 7: Jupyter Notebook on Remote Machine
<a id="7"> </a>

### Binding ports with remote server
```
jupyter notebook --no-browser --port=8080 # in remote server
ssh -L 8080:localhost:8080 user@remote_host # local_port:remote_host:remote_port # in local client
```

### Section 7.1: Automation by scripts
<a id="7_1"> </a>

***Allow port in Firewall in remote server***
```
sudo ufw status
sudo ufw allow 8181
```

**Port usage lookup/ killing**
```
sudo fuser -n tcp 8080 #status
sudo lsof -i :11434 #status
sudo lsof -k :11434 #kill
```

**Script for the Remote Server (`remote_setup.sh`)**
```
#!/bin/bash

# Ensure logs directory exists
LOG_DIR="$HOME/logs"
mkdir -p "$LOG_DIR"
is_port_in_use() {
    PORT=$1
    if ss -ltn | grep -q ":$PORT "; then
        return 0  # Port is in use
    else
        return 1  # Port is free
    fi
}
kill_program_on_port() {
    PORT=$1
    # Find the process ID(s) using the specified port
    PROCESS_IDS=$(sudo fuser -n tcp $PORT)

    # Check if any process is found
    if [ -z "$PROCESS_IDS" ]; then
        echo "No process found running on port $PORT."
    else
        # Kill the process(es) identified by the process IDs
        sudo kill -9  $PROCESS_IDS
        echo "Killed process(es) running on port $PORT."
    fi
}

kill_program_on_port 8181
#kill_program_on_port 11434
kill_program_on_port 8080

sudo ufw allow 8181
sudo ufw allow 11434
sudo ufw allow 8080

# Check and start Jupyter Notebook
echo "Starting Jupyter Notebook..."
nohup jupyter-notebook --no-browser --port=8181> "$LOG_DIR/jupyter.log" 2>&1 &
echo "Jupyter Notebook is started in port 8181"
PATH="/home/hissain/Python-3.11.10/venv/open-webui/bin:$PATH"
source /home/hissain/Python-3.11.10/venv/open-webui/bin/activate > "$LOG_DIR/venv.log" 2>&1 &
echo "Staring Ollama server"
export OLLAMA_HOST="IP:11434"
nohup ollama serve>"$LOG_DIR/ollama.log" 2>&1 &
#echo "Staring qdrant server"

echo "Staring open-webui server"
nohup open-webui serve > "$LOG_DIR/open-webui.log" 2>&1 &
#nohup deactivate
echo "All checks and startups completed."
```

**Script for Local Machine (`remote_connect.sh`)**
```
#!/bin/bash

# Remote server credentials
REMOTE_USER="your_username"
REMOTE_HOST="remote.server.ip"
REMOTE_PORT=22  # Default SSH port
JUPYTER_REMOTE_PORT=8181  # Adjust if your Jupyter runs on a different port
JUPYTER_LOCAL_PORT=8181  # Port to map locally

# Run the setup script on the remote server
ssh -p $REMOTE_PORT $REMOTE_USER@$REMOTE_HOST "bash -s" < remote_setup.sh

# Forward Jupyter Notebook port to the local machine
echo "Forwarding port $JUPYTER_REMOTE_PORT on remote to $JUPYTER_LOCAL_PORT locally..."
ssh -N -f -L $JUPYTER_LOCAL_PORT:localhost:$JUPYTER_REMOTE_PORT $REMOTE_USER@$REMOTE_HOST

echo "Jupyter Notebook is accessible at http://localhost:$JUPYTER_LOCAL_PORT"

```

#### Deployment Steps

1. Save the remote_setup.sh script on your local machine, then upload it to the remote server:
```
scp remote_setup.sh your_username@remote.server.ip:~
```

2. Run the following commands locally and on the remote server:
```
chmod +x remote_setup.sh remote_connect.sh
```

3. On your local machine, execute:
```
./remote_connect.sh
```
## Section 7.2: Avoid Notebook Token
<a id="7_2"> </a>

### Method: Disabling Token and Password Access (Secured via SSH)
Edit Jupyter Configuration on the Remote Server, if you don’t already have a Jupyter configuration file, create one:
```
jupyter notebook --generate-config
```

Then edit the configuration file located at ~/.jupyter/jupyter_notebook_config.py:
```
vi ~/.jupyter/jupyter_notebook_config.py
```

Add or modify the following lines:
```
c.NotebookApp.token = ''          # Disable token authentication
c.NotebookApp.password = ''       # Disable password authentication
c.NotebookApp.open_browser = False # Prevent the server from opening a browser
c.NotebookApp.ip = '0.0.0.0'       # Listen on all network interfaces
```

Run the notebook server as you normally would:
```
jupyter-notebook --no-browser --port=8888
```

SSH Tunnel from Local Machine, forward the remote Jupyter port to your local machine securely:
```
ssh -N -f -L 8888:localhost:8888 your_username@remote.server.ip
```

Open your browser and navigate to:
```
http://localhost:8888
```

## Section 8: Hugginface
<a id="8"> </a>

### Configure CLI & Download a model
```
pip install -U "huggingface_hub[cli]"  
  
huggingface-cli login # use your HF token  
   
# Download the HuggingFace model to local folder "./Meta-Llama-3.1-8B-Instruct"  
huggingface-cli download meta-llama/Meta-Llama-3.1-8B-Instruct --local-dir ./Meta-Llama-3.1-8B-Instruct
```

**Sometimes, model downloading may fail due to SSL error:**

Following this solution https://stackoverflow.com/a/77583757/1084174 might be helpful.

### Installing latest package
```
pip install git+https://github.com/huggingface/transformers
```

### Model Convertion using Script

Clone the llama.cpp utils

```
git clone https://github.com/ggerganov/llama.cpp
pip install -r llama.cpp/requirements.txt
```

Convert HF to GGUF
```
llama.cpp/convert_hf_to_gguf.py /base/models/Meta-Llama-3.1-8B-Instruct/ --outfile Meta-llama3.1.gguf --outtype q8_0
```

Convert LoRA adapter to GGUF
```
llama.cpp/convert_lora_to_gguf.py --base /base/models/Meta-Llama-3.1-8B-Instruct --outfile ./lora_adaptor.gguf --outtype auto ./Meta-llama3-8b-SFT/
```

Merging LoRA with Base HF
```

```

### Model Convertion using Library

Try this. Basic steps are to:
1/ load the base model
2/ train the base model
3/ save the LoRA adapter
4/ reload the base model at half/full precision
5/ merge the LoRA weights with the base model
6/ save

```
import torch
from transformers import (
    AutoModelForCausalLM,
    AutoTokenizer,
    BitsAndBytesConfig,
    pipeline,
    logging,
)
from peft import LoraConfig, PeftModel, get_peft_model, prepare_model_for_kbit_training

base_model_id = "./base/models/Meta-Llama-3.1-8B-Instruct"
new_model = "./lora/models/Meta-llama3-8b-SFT"

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_use_double_quant=True,
)

model = AutoModelForCausalLM.from_pretrained(base_model_id, quantization_config=bnb_config, device_map="auto")
tokenizer = AutoTokenizer.from_pretrained(base_model_id)
peft_config = LoraConfig(task_type="CAUSAL_LM", r=2, lora_alpha=16, lora_dropout=0.01)
adapter_model = PeftModel.from_pretrained(model, new_model)
merged_model = adapter_model.merge_and_unload()

merged_model.save_pretrained('./merged/models/Meta-llama3-8b-SFT-merged')
tokenizer.save_pretrained('./merged//models/Meta-llama3-8b-SFT-merged')
```

### Create Quantized Model on Ollama
Download a model
```
hfdownloader -m NousResearch/Hermes-2-Theta-Llama-3-8B
```

Create model
```
ollama create -f Modelfile NousResearch-Hermes-2-Theta-Llama-3-8B
```

Create quantized model
```
ollama create -f Modelfile NousResearch-Hermes-2-Theta-Llama-3-8B:q4_0 --quantize q4_0
```

Run model
```
ollama run --verbose NousResearch-Hermes-2-Theta-Llama-3-8B:q4_0 \
  What would happen in a fight between a lion and a unicorn
```

## Section 9: GPU Inspection 
<a id=9> </a>

### Continuous nvdia-smi inspection
```
watch -n 1 nvidia-smi
```

### 

## Section 10: Formatting and Beautification
<a id="10"> </a>

```
from rerankers import Reranker
from rich.console import Console
from rich.table import Table

def pretty_print(item):
  console = Console()
  with console.pager(styles=True):
    console.print(item)
```

## Section 11: Langchain with Ollama
<a id="11"> </a>

```
from langchain_ollama import OllamaLLM
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough

llm = OllamaLLM(base_url='http://localhost:11434', model="llama3.2:latest")

template = """
context: {context}
question: {question}
answer:
"""
prompt = ChatPromptTemplate.from_template(template)

# Create the chain
qa_chain = (
    {
        "context": RunnablePassthrough(),
        "question": RunnablePassthrough(),
    }
    | prompt
    | llm
    | StrOutputParser()
)

context = "I am Hissain, an AI enthusiast."
question = "Who is Hissain?"

result = qa_chain.invoke({"context": context, "question": question})
print(result)

```


## Section 12: Package Dependency Resolution
<a id="12"> </a>

### Installing pyarrow in python 3.11

```
pip install --extra-index-url https://pypi.fury.io/arrow-nightlies/ \
        --prefer-binary --pre pyarrow
```

On requirements.txt

```bash
--extra-index-url https://pypi.fury.io/arrow-nightlies/
--prefer-binary
--pre
pyarrow
```
