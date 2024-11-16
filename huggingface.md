### Download a model
```
pip install -U "huggingface_hub[cli]"  
  
huggingface-cli login # use your HF token  
   
# Download the HuggingFace model to local folder "./Meta-Llama-3.1-8B-Instruct"  
huggingface-cli download meta-llama/Meta-Llama-3.1-8B-Instruct --local-dir ./Meta-Llama-3.1-8B-Instruct
```
### Installing latest package
```
pip install git+https://github.com/huggingface/transformers
```
