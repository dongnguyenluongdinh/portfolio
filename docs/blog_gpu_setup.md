# TensorFlow GPU Setup in WSL2

[Native GPU Support on Windows is Deprecated](https://www.tensorflow.org/install/pip#windows-wsl2): TensorFlow 2.10 is the last version to support GPU natively on Windows.

From 2.11 onward:

- Native GPU support was removed

- CUDA-based GPU usage only works on Linux or WSL2


## Installation

### ✅ Install WSL2 + Ubuntu 
``` sh
wsl --install -d Ubuntu
```

### ✅ Set Up NVIDIA GPU in WSL2

Prerequisites:

- NVIDIA GPU drivers 525.105 or newer

- WSL2 CUDA support installed

``` cmd
>>> nvidia-smi
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 537.13                 Driver Version: 537.13       CUDA Version: 11.8     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                     TCC/WDDM  | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA GeForce RTX 3060 ...  WDDM  | 00000000:01:00.0 Off |                  N/A |
| N/A   39C    P8               9W /  95W |      0MiB /  6144MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
+---------------------------------------------------------------------------------------+
```

### ✅ Install TensorFlow in WSL2 (with GPU)

``` sh
uv venv
source .venv/bin/activate
uv pip install tensorflow
```

The latest tensorflow from PyPI on Linux includes GPU support out of the box, but for stability select `tensorflow 2.15`.

### ✅ Test GPU Access

Create a `gpu_check.py` like this:

``` python
import tensorflow as tf

print("TensorFlow Version:", tf.__version__)
print("GPUs:", tf.config.list_physical_devices('GPU'))
```

Then execute it using:

``` sh
>>> python gpu_check.py
TensorFlow Version: 2.15.0
GPUs: [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

## Error handling

To be continue ...