# TensorFlow GPU Setup in WSL2

[Native GPU Support on Windows is Deprecated](https://www.tensorflow.org/install/pip#windows-wsl2): TensorFlow 2.10 is the last version to support GPU natively on Windows.

From 2.11 onward:

- Native GPU support was removed

- CUDA-based GPU usage only works on Linux or WSL2

## Installation

This guide focuses on using `TensorFlow 2.10` in combination with `CUDA 11.8` for optimal GPU acceleration. For further information on compatibility between TensorFlow and CUDA versions, you can refer to [NVIDIA Optimized Frameworks - TensorFlow Release Notes](https://docs.nvidia.com/deeplearning/frameworks/tensorflow-release-notes/index.html).

### ✅ Install WSL2 + Ubuntu 
``` sh
wsl --install -d Ubuntu
```
For a successful installation, output should be like this:

``` sh
>>> wsl --install -d Ubuntu
Installing: Ubuntu
Ubuntu has been successfully installed.
Press any key to continue...
```

### ✅ Set Up NVIDIA GPU

Prerequisites:

- Windows 10 19044 or higher (64-bit).

- NVIDIA® GPU card with CUDA® architectures 3.5, 5.0, 6.0, 7.0, 7.5, 8.0 and higher: [CUDA®-enabled GPU cards](https://developer.nvidia.com/cuda-gpus)

- NVIDIA GPU drivers 525.105 or newer

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
- WSL2 CUDA support packages installed

Add CUDA 11.8 APT Repo for WSL2:

``` sh
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600

wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
```

Install CUDA Toolkit 11.8
``` sh
sudo apt install -y cuda-toolkit-11-8
```

Update `.bashrc`:

``` sh
export PATH=/usr/local/cuda-11.8/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH
```
Refresh `.bashrc` and apply changes to the shell:

``` sh
source ~/.bashrc
```
Test CUDA 11.8 Installation

``` sh
git clone https://github.com/NVIDIA/cuda-samples.git 
cd cuda-samples/Samples/1_Utilities/deviceQuery
cmake . # (1)!
make 
./deviceQuery 
```

1.  Before running `make`, you need to generate the Makefiles using `CMake`. If `Cmake` not available, install with:
    ``` sh
    sudo apt install cmake
    ```

Then run the built `deviceQuery` with:

``` cmd
>>> ./deciveQuery
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "NVIDIA GeForce RTX 3060 Laptop GPU"
  CUDA Driver Version / Runtime Version          12.2 / 11.8
  CUDA Capability Major/Minor version number:    8.6
  Total amount of global memory:                 6144 MBytes (6441926656 bytes)
  (030) Multiprocessors, (128) CUDA Cores/MP:    3840 CUDA Cores
  GPU Max Clock rate:                            1425 MHz (1.42 GHz)
  Memory Clock rate:                             7001 Mhz
  Memory Bus Width:                              192-bit
  L2 Cache Size:                                 3145728 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total shared memory per multiprocessor:        102400 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  1536
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 1 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device supports Managed Memory:                Yes
  Device supports Compute Preemption:            Yes
  Supports Cooperative Kernel Launch:            Yes
  Supports MultiDevice Co-op Kernel Launch:      No
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 12.2, CUDA Runtime Version = 11.8, NumDevs = 1
Result = PASS
```

### ✅ Install TensorFlow

``` sh
uv venv
source .venv/bin/activate
uv pip install tensorflow # (1)!
```

1.  The latest tensorflow from PyPI on Linux includes GPU support out of the box, but for stability select `tensorflow 2.10`.
   
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
TensorFlow Version: 2.10.0
GPUs: [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

## Error handling

To be continue ...