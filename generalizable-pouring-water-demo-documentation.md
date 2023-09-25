# Generalizable Pouring Water Demo Documentation

## Generalizable Pouring Water Demo

### Introduction

TODO

### Installation Dependencies

Click the links below to see the installation instructions for each dependency.

<details>

<summary>numpy=1.23.0</summary>

NumPy, short for "Numerical Python," is a fundamental Python library that provides support for large, multi-dimensional arrays and matrices, as well as a collection of high-level mathematical functions to operate on these arrays. It serves as a cornerstone for numerical and scientific computing in Python, enabling efficient data manipulation, computation, and analysis. NumPy's array objects are more memory-efficient and faster than Python's built-in lists, making it a popular choice for tasks involving numerical data, such as data analysis, machine learning, and scientific research. Its versatility and extensive ecosystem of libraries make it an essential tool for data scientists, engineers, and researchers in various fields.

```bash
# Install numpy
pip install numpy
```

For more details, please refer to [numpy](https://numpy.org/).

</details>

<details>

<summary>opencv=4.8.0.76</summary>

OpenCV, or Open Source Computer Vision Library, is an open-source computer vision and machine learning software library widely used in various fields, including image and video processing, computer vision tasks, and robotics. It provides a comprehensive set of tools and functions for tasks such as image and video manipulation, object detection and tracking, facial recognition, and more. OpenCV is written in C++ and offers bindings for popular programming languages like Python, making it accessible to a broad community of developers and researchers for creating applications that involve visual data analysis and processing.

```bash
# Install opencv
pip install opencv-python
```

For more details, please refer to [opencv](https://opencv.org/).

</details>

<details>

<summary>mmcv=1.7.0</summary>

MMCV, short for "Multimodal Computing and Vision Library," is a fundamental and versatile library that serves as a cornerstone for computer vision research. It encompasses a wide range of essential functionalities, including image and video processing, image and annotation visualization, image transformation, support for various convolutional neural network (CNN) architectures, and high-quality implementations of common CUDA (Compute Unified Device Architecture) operations. By offering these comprehensive tools and capabilities, MMCV empowers researchers and developers to efficiently work on computer vision tasks, from data preprocessing and model development to performance optimization, making it an indispensable resource in the field of computer vision.

```bash
# Install mmcv
pip install -U openmim
mim install mmcv==1.7.0
```

```bash
# Check mmcv installation
pip show mmcv
```

For more details, please refer to [mmcv](https://github.com/open-mmlab/mmcv).

</details>

<details>

<summary>CUDA=11.7</summary>

NVIDIA CUDA (Compute Unified Device Architecture) is a parallel computing platform and application programming interface (API) developed by NVIDIA. It enables developers to harness the immense computational power of NVIDIA GPUs (Graphics Processing Units) for a wide range of tasks beyond traditional graphics rendering. CUDA allows programmers to write code that can execute in parallel on the GPU, making it well-suited for computationally intensive workloads such as scientific simulations, deep learning, and data processing. It provides a framework for creating highly efficient and scalable GPU-accelerated applications, making it a cornerstone technology in the field of high-performance computing and accelerating a wide range of scientific and industrial applications.

You can download CUDA from [here](https://developer.nvidia.com/cuda-toolkit), then install it according to the instructions. After installation, you can check the CUDA version by running the following commands:

```bash
export CUDA_PATH=/usr/local/cuda

# The following command should print a CUDA compiler version =11.7
${CUDA_PATH}/bin/nvcc --version

# The following command should output a valid gcc version
gcc --version

# Set CUDA environment variables because mani_skill2 soft-body simulation depends on only one GPU.
# Select the first CUDA device. Change 0 to other integer for other device.
export CUDA_VISIBLE_DEVICES=0
```

For more details, please refer to [CUDA](https://developer.nvidia.com/cuda-toolkit-archive).

</details>

<details>

<summary>torch=2.0.1</summary>

PyTorch, is an open-source machine learning library primarily developed by Facebook's AI Research lab (FAIR). It provides a flexible and efficient framework for building and training deep neural networks. Torch is particularly popular among researchers and practitioners in the fields of artificial intelligence and deep learning due to its dynamic computation graph, which makes it easier to define and modify complex neural network architectures. It also offers support for GPU acceleration, enabling faster training of deep learning models. PyTorch's user-friendly interface, extensive documentation, and active community make it a valuable tool for various machine learning tasks, including image and speech recognition, natural language processing, and reinforcement learning.

```bash
# Install torch 2.0.1 with CUDA 11.7 in conda[Recommended]
conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
```

```bash
# Install torch 2.0.1 with CUDA 11.7 in pip[Not Recommended]
pip3 install torch torchvision torchaudio
```

For more details, please refer to [PyTorch](https://pytorch.org/).

</details>

<details>

<summary>vulkan</summary>

Vulkan is a high-performance, cross-platform graphics and compute API (Application Programming Interface) developed by the Khronos Group. It provides developers with low-level access to the GPU (Graphics Processing Unit), enabling them to efficiently leverage the full potential of modern graphics hardware. Vulkan offers greater control and flexibility compared to older graphics APIs like OpenGL, allowing for more efficient multi-threading, reduced driver overhead, and improved performance. It has gained popularity in the gaming and graphics development industry as it allows for better utilization of hardware resources, making it a preferred choice for creating visually stunning and high-performance applications and games on various platforms.

```bash
sudo apt-get install libvulkan1
sudo apt-get install vulkan-utils
vulkaninfo
```

For more details, please refer to [Vulkan](https://vulkan.lunarg.com/).

</details>

<details>

<summary>SAPIEN=2.2.2</summary>

SAPIEN is a state-of-the-art simulated environment designed for robotics research and development. Developed collaboratively by researchers from UCSD, Stanford, and SFU, SAPIEN offers a realistic and physics-rich setting to simulate complex robotic scenarios, particularly those involving articulated objects. It provides a comprehensive platform for tasks related to robotic vision and interaction, enabling researchers and developers to explore and test advanced algorithms and control strategies in a virtual environment before implementing them in the physical world. The ease of installation through pip makes it accessible to a wide range of users interested in advancing robotics technology and applications.

```bash
# Install SAPIEN
pip install sapien
```

For more details, please refer to [SAPIEN](https://sapien.ucsd.edu/).

</details>

<details>

<summary>mplib=0.0.8</summary>

mplib is a lightweight Python package designed for motion planning in robotics, notable for its independence from the Robot Operating System (ROS) and its straightforward setup process. With just a few lines of Python code, users can access a wide range of motion planning functionalities for robot manipulation tasks. MPlib simplifies the implementation of motion planning algorithms, allowing researchers and developers to quickly and efficiently design and test robotic motion plans, making it a valuable tool for those working on robotic manipulation projects.

```bash
# Install motion planning module
pip install mplib
```

For more details, please refer to [mplib](https://github.com/haosulab/MPlib).

</details>

<details>

<summary>mani_skill2=0.4.2</summary>

ManiSkill2 is a comprehensive benchmark for developing and evaluating robotic manipulation skills, supported by SAPIEN. It offers a versatile set of 20 predefined task categories, encompassing over 2000 diverse object models and a vast dataset of 4 million+ demonstration frames. What sets it apart is its ability to facilitate rapid learning of visual inputs, enabling a CNN-based policy to generate samples at an impressive rate of around 2000 frames per second using just one GPU and 16 processes on a standard workstation. Researchers and developers can utilize ManiSkill2 to explore and advance a wide spectrum of robotic learning algorithms, including 2D and 3D vision-based reinforcement learning, imitation learning, and sense-plan-act strategies.

```bash
# Install mani_skill2 with pip [NOT RECOMMENDED]
pip install mani-skill2
```

```bash
# Install mani_skill2 from source of Hao Shen[RECOMMENDED]
cd ManiSkill2 && pip install -e .
```

```bash
# Download the assets [optional]
python -m mani_skill2.utils.download_asset all
```

```bash
# Test after installation
# Wouldn't be working if you use source code from Hao Shen
python -m mani_skill2.examples.demo_random_action
```

It should show results like this:

```bash
info {'elapsed_steps': 197, 'is_obj_placed': False, 'is_robot_static': False, 'success': False}
reward 0.13719598497686514
terminated False
truncated False
info {'elapsed_steps': 198, 'is_obj_placed': False, 'is_robot_static': False, 'success': False}
reward 0.13144633949944368
terminated False
truncated False
info {'elapsed_steps': 199, 'is_obj_placed': False, 'is_robot_static': False, 'success': False}
reward 0.12609898907990486
terminated False
truncated True
info {'elapsed_steps': 200, 'is_obj_placed': False, 'is_robot_static': False, 'success': False}
```

* Move assets to package folder\[IF YOU USE PIP, NOT RECOMMENDED]

```bash
# Copy your assets under /generalizable_pouring_water_demo/ManiSkill2/mani_skill2/assets to folder below
cd ~/.local/lib/python3.8/site-packages/mani_skill2/assets
```

For more details, please refer to [ManiSkill2](https://github.com/haosulab/ManiSkill2).

* NVIDIA wrap for mani\_skill2 soft-body simulation

```bash
# Build warp.so
# If you encounter "ModuleNotFoundError: No module named 'warp'", please add warp_maniskill to the python path. 
export PYTHONPATH=/path/to/ManiSkill2/warp_maniskill:$PYTHONPATH
# warp.so is generated under warp_maniskill/warp/bin
python -m warp_maniskill.build_lib
```

For more details, please refer to [NVIDIA wrap](https://github.com/NVIDIA/warp) and [wrap for mani\_skill2](https://haosulab.github.io/ManiSkill2/getting\_started/installation.html#warp-maniskill2-version).

</details>

<details>

<summary>gymnasium=0.29.1</summary>

Gymnasium is an open source Python library for developing and comparing reinforcement learning algorithms by providing a standard API to communicate between learning algorithms and environments, as well as a standard set of environments compliant with that API.

```bash
# Install gymnasium
pip install gymnasium
pip install gym
```

For more details, please refer to [gymnasium](https://github.com/farama-foundation/gymnasium).

</details>

<details>

<summary>timm=0.9.7</summary>

Timm (short for "PyTorch Image Models") is an open-source library and repository of pre-trained computer vision models for deep learning tasks, primarily focused on image classification and related tasks. It is designed to work seamlessly with PyTorch, a popular deep learning framework. Timm provides a wide range of state-of-the-art convolutional neural networks (CNNs) and efficient model architectures, making it easier for researchers and practitioners to access and use powerful pre-trained models for various computer vision applications. This library simplifies the process of experimenting with and deploying deep learning models in PyTorch, helping users achieve better results and save time in their image analysis projects.

```bash
# Install timm
pip install timm
```

For more details, please refer to [timm](https://github.com/huggingface/pytorch-image-models).

</details>

<details>

<summary>ROS1 Noetic</summary>

```bash
# Install ROS1 Noetic
wget -O $HOME/ros1_noetic_install.sh https://raw.githubusercontent.com/auromix/ros-install-one-click/main/ros1_noetic_install.sh && sudo chmod +x $HOME/ros1_noetic_install.sh && sudo bash $HOME/ros1_noetic_install.sh && rm $HOME/ros1_noetic_install.sh
```

```bash
# Install Moveit
wget -O $HOME/moveit1_install.sh https://raw.githubusercontent.com/auromix/ros-install-one-click/main/moveit1_install.sh && sudo chmod +x $HOME/moveit1_install.sh && sudo bash $HOME/moveit1_install.sh && rm $HOME/moveit1_install.sh
```

For more details, please refer to [ROS](https://www.ros.org/).

</details>

<details>

<summary>pyrealsense2=2.54.1.5217</summary>

Pyrealsense2 is a software framework and API (Application Programming Interface) developed by Intel for their RealSense depth-sensing cameras and sensors. It provides developers with a set of tools and libraries to interact with RealSense devices, enabling them to capture depth, color, and infrared data, as well as perform various computer vision tasks such as object tracking, gesture recognition, and 3D scanning. Pyrealsense2 allows for the integration of RealSense technology into a wide range of applications, including robotics, augmented reality, virtual reality, and computer vision projects, making it a valuable resource for creating immersive and intelligent systems that can perceive and interact with the surrounding environment.

```
# Install pyrealsense2
pip install pyrealsense2
# For ROS
sudo apt install ros-noetic-realsense2-* 
```

For more details, please refer to [pyrealsense2](https://github.com/IntelRealSense/librealsense/blob/master/wrappers/python/readme.md).

</details>

<details>

<summary>galbot_ros &#x26; rm_robot</summary>

Dowload galbot\_ros & rm\_robot to your catkin workspace.

````bash
# Create a catkin workspace
mkdir -p galbot_ws/src
cd galbot_ws/src
# Download galbot_ros & rm_robot

```bash
# Install dependencies
rosdep install --from-paths src --ignore-src -r -y
# Build
catkin_make
````

</details>

### Getting Started

<details>

<summary>Test CUDA with PyTorch</summary>

#### Test CUDA with PyTorch

```bash
python3 demos/test_cuda_pytorch.py
```

</details>

<details>

<summary>Test mani_skill2</summary>

#### Test mani\_skill2

This code snippet accomplishes the following tasks: It sets up a simulated environment using Gym and a custom "mani\_skill2" environment for a robotic cube-picking task. The environment is configured with RGBD observations and PD joint position control. It then resets the environment with a specified random seed, initializes variables to track termination and truncation, and enters a loop where it randomly samples actions from the action space, executes them in the environment, and renders the visual display. This loop continues until the environment either terminates or is truncated. Finally, the code closes the environment. Essentially, this code demonstrates the basic interaction between an agent and a robotic simulation environment, where the agent takes random actions, observes the environment's responses, and renders the simulation for visualization.

```bash
# Wouldn't be working if you use source code from Hao Shen
# Would be working if you use pip install for mani_skill2

python3 demos/maniskill2_getting_start_test.py
```

</details>

### TODO

### Troubleshooting

```bash
Q: Error occurs when running demos/maniskill2_getting_start_test.py

A: Use source code from Hao Shen instead of pip install for mani_skill2
```

```bash
Q: Could not find a package under 'mani_skill2' 

A: export PYTHONPATH=$PYTHONPATH:/home/hermanye20/generalizable_pouring_water_demo/ManiSkill2
```
