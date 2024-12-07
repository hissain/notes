### 1. Check GPU Memory ###
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

### 2. Multiple GPU Accessibility ###

#### Allow GPU Access to All Users:
By default, GPU access is granted to users in the video group. Add all relevant users to this group:

```bash
sudo usermod -aG video <username>
```

Verify the user is added:

```bash
groups <username>
```

#### Configure Environment Variables:
Set up the environment for all users via a global configuration:

Add the following to /etc/environment or a similar system-wide configuration file:

```bash
CUDA_VISIBLE_DEVICES=0,1
PATH=$PATH:/usr/local/cuda/bin
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
```

Reload the environment variables:

```bash
source /etc/environment
```

#### Ensure that SSH sessions can access GPUs:
Set up the CUDA_VISIBLE_DEVICES variable for users automatically: Add the following to /etc/profile:

```bash
export CUDA_VISIBLE_DEVICES=0,1
```
