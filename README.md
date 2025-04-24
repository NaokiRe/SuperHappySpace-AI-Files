# SuperHappySpace-AI-Files

## Overview

This is a repository containing all the AI components of the Super Happy Space project created by team 1 of the 2024/2025 UCL 
Systems Engineering module in partnership with the UCL IXN program, National Autistic Society and Intel.

Below is a youtube link to the final presentation of our project, which includes a demo of the product.
\(https://www.youtube.com/watch?v=kQ6ZY-i36DI&t=24s\)

This repository contains the AI components of the project, which are used to generate automated images and categorisation based on any audio file input specified by the user. The AI components are built using OpenVINO and HuggingFace libraries, and are designed to be used in conjunction with the main application.

## Project Structure

The required files for the project are structured as follows:
    
```shell
SuperHappySpace-AI-Files
├── AiResources
│   ├── distil-whisper-large-v3-int8-ov
│   ├── gemma-2-9b-it-int4-ov
│   ├── sdxs-512-dreamshaper
│   └── openvino_2025
│
├── assets
│
├── src
│   ├── Cpp
│   │  ├── project
│   │  └── build
│   ├── python
│   │  ├── SD.py
│   │  └── requirements.txt
│
├── cppVer.exe
├── SD.exe
└── README.md
```

Out of the files, `distil-whisper-large-v3-int8-ov`, `gemma-2-9b-it-int4-ov`, `sdxs-512-dreamshaper` and `openvino_2025` are the AI models and libraries used in the project. The `cppVer.exe` and `SD.exe` files are the compiled versions of the C++ and Python implementations of the AI components, respectively. The `assets` folder contains any additional resources needed for the project. The `src` folder contains the source code for both the C++ and Python implementations of the AI components.

The following files are not included in the github repository as they are too large to be uploaded to github, so require to be installed separately, as detailed in the installation section:
1. `distil-whisper-large-v3-int8-ov` - OpenVINO model for speech recognition
2. `gemma-2-9b-it-int4-ov` - OpenVINO model for text generation
3. `sdxs-512-dreamshaper` - stable diffusion model for image generation
4. `openvino_2025` - OpenVINO library for running the models
5. `SD.exe` - Stable diffusion executable for image generation


The majority of options in the code are meant to be used in conjunction with our main application, hence some of the options may not work as expected if run independently in this repo. Some demo songs are included in the assets folder, along with the use case commands detailed in the demo section below. 


## Installation 


### Github installation

1. Clone the repository
```shell
git clone https://github.com/NaokiRe/SuperHappySpace-AI-Files.git
```

2. Install dependencies


#### AI dependencies

Download the models in the `AiResources` directory either manually or using git lfs
```shell
cd AiResources
# download and move all 3 models from huggingface here
```
If using git to install the models:
1. download Git LFS following the instructions [here](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage)
2. Run the following commands
```shell
git lfs install
git clone https://huggingface.co/OpenVINO/distil-whisper-large-v3-int8-ov
git clone https://huggingface.co/OpenVINO/gemma-2-9b-it-int4-ov
git clone https://huggingface.co/IDKiro/sdxs-512-dreamshaper
```

Otherwise, download the models from the following links:
1. [distil-whisper-large-v3-int8-ov](https://huggingface.co/OpenVINO/distil-whisper-large-v3-int8-ov)
2. [gemma-2-9b-it-int4-ov](https://huggingface.co/OpenVINO/gemma-2-9b-it-int4-ov)
3. [sdxs-512-dreamshaper](https://huggingface.co/IDKiro/sdxs-512-dreamshaper)

And place them in the `AiResources` directory.

Finally, download the openvino package from the zip file [here](https://storage.openvinotoolkit.org/repositories/openvino_genai/packages/2025.0/windows).
Unzip the file and rename it to `openvino_2025` and place it in the `AiResources` directory.

## Compiling from Source

### OpenVINO C++ 

The C++ implementation using OpenVINO and OpenVINO GenAI library has 2 ways to compile, using a dynamic library format or a static library format. The dynamic library format is recommended.

#### Dynamic Build 

Download the openvino package from the zip file [here](https://storage.openvinotoolkit.org/repositories/openvino_genai/packages/2025.0/windows).
Unzip the file and rename it to `openvino_2025` and place it in the `AiResources` directory.

Run the setup script from the root directory
```shell
./AiResources/openvino_2025/setupvars.bat
# or setupvars.ps1 for powershell systems
```

From the root directory, move to the external directory and create a build directory
```shell
cd src/Cpp
mkdir build
``` 

Run the following commands to build the project
```shell
cmake -S ./project -B ./build
cmake --build ./build --config Release
```

The executable will be in under `./src/Cpp/build/Release/cppVer.exe`. 
Move this file to `./` (root directory) to be used by the main application.


#### Static Build 

It is possible to create static build of openvino from the source files. However, this is not a recommended method as it is more complex and the nuumber of errors the user encounters heavily depends on their setup.

First clone the necessary repositories and submodules
```shell
git clone https://github.com/openvinotoolkit/openvino.git
git clone https://github.com/openvinotoolkit/openvino.genai.git
git submodule update --init --recursive
mkdir build & cd build
```

Run the following commands to build the project, assuming you have Visual Studio 2019 installed, choose the correct generator for your system.
```shell
cmake -G "Visual Studio 16 2019" -DBUILD_SHARED_LIBS=OFF -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE={path to openvino}/cmake/toolchains/mt.runtime.win32.toolchain.cmake -DOPENVINO_EXTRA_MODULES={path to openvion.genai} ../openvino 
```

Compile the cmake project
```shell
cmake --build . --target openvino --config Release 
```

Install the project onto your machine
```shell
cmake -DCMAKE_INSTALL_PREFIX=C:\Users\billy\Documents\coding\temp\install -P .\build\cmake_install.cmake 
```

After these steps, the openvino library will be installed on your machine and it should be possible to link it to the C++ project using cmake.


### Diffusers Python Pyinstaller 
Go to the external directory and run the following command:
```shell
cd src/python
pip install requirements.txt
pyinstaller –F SD.py 
```
The build was done with python version 3.10.0. The executable will be in the `dist` folder. Move the executable to the `./` (root) folder to be used by the main application.


## Demo commands
WARNING: make sure the all required files are in the correct directories before running the commands. The AI models and libraries should be in the `AiResources` directory, and the executable files should be in the root directory. (structure shown in the project structure section above).

There is 1 song in the asset by default "Do you want to build a snowman", with the id `TeQ_TTyLGMs` as a demo song. 