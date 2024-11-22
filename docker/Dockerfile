# Use an official PyTorch image with CUDA 11.8 support
FROM pytorch/pytorch:2.1.0-cuda11.8-cudnn8-devel AS base

# Set non-interactive frontend for tzdata configuration
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

# Install system dependencies, including git, python3-pip, etc.
RUN apt-get update && apt-get install -y \
    git \
    python3-pip \
    python3-dev \
    libgl1-mesa-glx \
    libglib2.0-0 \
    libglib2.0-dev \
    tzdata \
    && rm -rf /var/lib/apt/lists/*

# Ensure shared libraries are correctly linked
RUN ldconfig

# Upgrade pip
RUN pip install --upgrade pip

# Set the working directory to /anomalib
WORKDIR /anomalib

# Copy the pyproject.toml file from your local machine into the container at /anomalib
COPY pyproject.toml /anomalib/

# Install basic Python dependencies first to avoid issues during installation
RUN pip install --upgrade pip setuptools poetry

# Now copy the entire Anomalib directory content into the container at /anomalib
COPY . /anomalib/

# Install Python dependencies from pyproject.toml (this will install Anomalib and other dependencies)
RUN pip install .

# Install other necessary dependencies
RUN pip install "numpy==1.23.5" matplotlib>=3.7.1 Pillow==9.5.0 pytest==7.4.0 pyyaml==6.0 pandas scikit-learn timm einops wandb openvino-dev[onnx] FrEIA open_clip_torch

# Install a compatible version of pydantic to resolve the ModelMetaclass error
RUN pip install "pydantic==1.10.7"

# Install Lightning
RUN pip install lightning==2.1.2

# Install GitPython to use git.repo in the notebook
RUN pip install gitpython

# Install OpenCV
RUN pip install opencv-python

# Install Kornia for image processing tasks
RUN pip install kornia

# Install imgaug
RUN pip install imgaug

# Install websockets library
RUN pip install websockets

# Install Jupyter
RUN pip install jupyter

# Set entrypoint to bash to run your container interactively
CMD ["/bin/bash"]
