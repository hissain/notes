# Python Virtual Environment Quick Notes

## 1. Creating a Virtual Environment with a Specific Python Version

To create a virtual environment with a specific version of Python, use the following command:

```
python3.x -m venv myenv
```

```
python3.10 -m venv myenv
```

## 2. Activating the Virtual Environment
On Linux/macOS:

```bash
source myenv/bin/activate
```

On Windows:
```bash
myenv\Scripts\activate
```

Once activated, the virtual environment's name will appear in your terminal prompt.

## Add custom ssl certificates
-----------------------------------
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

## 3. Deactivating the Virtual Environment
To deactivate the virtual environment, simply run:

```bash
deactivate
```

## 4. Installing Packages from pip
Once the virtual environment is active, install packages using pip:

```
pip install package_name
```
Example:
```
pip install numpy
```

## 5. Freezing Installed Packages
To generate a list of all installed packages and their versions in your virtual environment:

```bash
pip freeze > requirements.txt
```

## 6. Installing Packages from requirements.txt
To install all the dependencies listed in a requirements.txt file, run:

```bash
pip install -r requirements.txt
```

This will create or update the requirements.txt file with the package list.

## 7. Uninstalling Packages
To uninstall a package, use:

```bash
pip uninstall package_name
```
Example:

```
pip uninstall numpy
```

## 8. Deleting a Virtual Environment
To remove a virtual environment, deactivate it and delete the directory:

```bash
deactivate
```
```
rm -rf myenv
```


## Notebook Kernel Creation

```
pip install ipykernel
python -m ipykernel install --user --name LLamaFineTuningKernel

```
