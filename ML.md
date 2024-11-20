### Check GPU Memory ###
``` python
import torch

if torch.cuda.is_available():
    num_gpus = torch.cuda.device_count()
    for i in range(num_gpus):
        gpu_device = torch.cuda.get_device_name(i)
        gpu_memory = torch.cuda.get_device_properties(i).total_memory / (1024 ** 3)
        allocated_memory = torch.cuda.memory_allocated(i) / (1024 ** 3)
        cached_memory = torch.cuda.memory_reserved(i) / (1024 ** 3)
        
        print(f"GPU {i}: {gpu_device}")
        print(f"Total Memory: {gpu_memory:.2f} GB")
        print(f"Allocated Memory: {allocated_memory:.2f} GB")
        print(f"Cached Memory: {cached_memory:.2f} GB")
        print()
else:
    print("No GPU available.")
```
