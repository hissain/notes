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

## Virtual Environment
<a id="section-2-venv"></a>

### 1. Creating a Virtual Environment with a Specific Python Version

To create a virtual environment with a specific version of Python, use the following command:

```
python3.x -m venv myenv
```

```
python3.10 -m venv myenv
```

### 2. Activating the Virtual Environment
On Linux/macOS:

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

## 8. Deleting a Virtual Environment
To remove a virtual environment, deactivate it and delete the directory:

```bash
deactivate
```
```
rm -rf myenv
```

### 3. Deactivating the Virtual Environment
To deactivate the virtual environment, simply run:

```bash
deactivate
```

## 3. PIP Usage
<a id="section-3-pip"></a>
Once the virtual environment is active, install packages using pip:

```
pip install package_name
```
Example:
```
pip install numpy
```

### 5. Freezing Installed Packages
To generate a list of all installed packages and their versions in your virtual environment:

```bash
pip freeze > requirements.txt
```

### 6. Installing Packages from requirements.txt
To install all the dependencies listed in a requirements.txt file, run:

```bash
pip install -r requirements.txt
```

This will create or update the requirements.txt file with the package list.

### 7. Uninstalling Packages
To uninstall a package, use:

```bash
pip uninstall package_name
```
Example:

```
pip uninstall numpy
```
