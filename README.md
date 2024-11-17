# Cheatsheets for ML Engineer

## Table of Contents
- [Section 1: Python](#section-1-python)
  - [1.1 Installation](#installation)
  - [1.2 Updating](#objectives)
- [Section 2: Virtual Environment](#section-2-venv)
  - [2.1 Installation](#installation)
- [Section 3: PIP Usage](#section-3-pip)

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

### Install Python 3.11:

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

## 2. Section 2: Virtual Environment
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
