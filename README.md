# Cheatsheets for ML Engineer

## Table of Contents
- [Section 1: Python](#section-1-python)
  - [1.1 Installation](#section-1-python-installation)
- [Section 2: Virtual Environment](#section-2-venv)
  - [2.1 Installation](#installation)
- [Section 3: PIP Usage](#section-3-pip)
- [Section 4: Docker Usage](#section-4-docker)
- [Section 5: Ollama on Open-WebUI](#section-5-openwebui)
- [Section 6: Qdrant Database](#section-6-qdrant)

## Section 1: Python Installation
<a id="section-1-python"></a>
As an example, install Python 3.11 on Ubuntu from the command line, follow these steps:

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
<a id="section-1-python-installation"></a>

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
<a id="section-2-venv"></a>

### Creating a Virtual Environment with a Specific Python Version

To create a virtual environment with a specific version of Python, use the following command:
```
python3.10 -m venv myenv
```

### Activating the Virtual Environment

```bash
source myenv/bin/activate
```

Once activated, the virtual environment's name will appear in your terminal prompt.

### Add custom ssl certificates

```bash
pip install certifi
python -c "import certifi; print(certifi.where())"
```
```
openssl x509 -in $specific_ca.crt -text >> $virtualenv/lib/python2.7/site-packages/certifi/cacert.pem
```

Sometimes when pip installation yields SSLError you need to use `--trusted-host`, like,

```
python -m pip install --trusted-host github.com
```

## Deactivating/Deleting a Virtual Environment
To remove a virtual environment, deactivate it and delete the directory:

```bash
deactivate
```
```
rm -rf myenv
```

## Section 3: PIP Usage
<a id="section-3-pip"></a>
Once the virtual environment is active, install packages using pip:

```
pip install package_name
```
Example:
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
<a id="section-4-docker"></a>

### Installation Docker-Desktop

``` sudo apt-get update ```

Download docker-desktop and install with, 

``` sudo apt-get install ./docker-desktop-amd64.deb ```

### Installation of Docker
``` sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo systemctl start docker
sudo systemctl enable docker
```

### Working with Docker/ Docker-Desktop

``` sudo docker pull qdrant/qdrant ```

``` sudo docker run -d --name qdrant -p 6333:6333 qdrant/qdrant ```

### Start/Attach/Stop

``` sudo docker start qdrant ```

``` sudo docker attach qdrant ```

``` sudo docker stop qdrant ```

## Section 5: Ollama on Open-WebUI
<a id="section-5-openwebui"></a>

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

### Ollama Installation

To install Ollama, run the following command:
```
curl -fsSL https://ollama.com/install.sh | sh
```

Start ollama server
```
ollama serve
```

Start ollama server in local PC (accessible from LAN)
```
OLLAMA_HOST=127.0.0.1:11434 ollama serve
```

## Section 6:Qdrant Vector DB
<a id="section-6-qdrant"></a>

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
from qdrant_client.models import PointStruct

operation_info = client.upsert(
    collection_name="test_collection",
    wait=True,
    points=[
        PointStruct(id=1, vector=[0.05, 0.61, 0.76, 0.74], payload={"city": "Berlin"}),
        PointStruct(id=2, vector=[0.19, 0.81, 0.75, 0.11], payload={"city": "London"}),
        PointStruct(id=3, vector=[0.36, 0.55, 0.47, 0.94], payload={"city": "Moscow"}),
        PointStruct(id=4, vector=[0.18, 0.01, 0.85, 0.80], payload={"city": "New York"}),
        PointStruct(id=5, vector=[0.24, 0.18, 0.22, 0.44], payload={"city": "Beijing"}),
        PointStruct(id=6, vector=[0.35, 0.08, 0.11, 0.44], payload={"city": "Mumbai"}),
    ],
)

print(operation_info)
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
