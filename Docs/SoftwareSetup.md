# Software Setup

## System Requirements

This setup requires **Debian** or **Ubuntu** as the operating system.

## Initial System Setup

### Update System Packages

```bash
sudo apt update
sudo apt upgrade
```

### Install other dependencies  

If not already installed:

```bash
 sudo apt-get install jq
```

## Conda Installation (ARM Architecture)

Install Miniconda on ARM-based systems:

```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
```

## Clone Repository

Clone the project repository:

```bash
git clone --recurse-submodules https://github.com/NicoJoerger/Embedded-CPU-LLM.git
```

Navigate to the TinyChatEngine directory:

```bash
cd 
Embedded-CPU-LLM/Code/TinyChatEngineAPI
```

Create conda env and requirements 

```bash
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
conda create -n TinyChatEngine python=3.10 pip -y
conda activate TinyChatEngine
pip install -r requirements.txt
```

Download model

```bash
cd llm
python tools/download_model.py --model LLaMA_3_8B_Instruct_awq_int4 --QM QM_ARM
```

Run model

```bash
make chat -j
./chat

```
