# FastDDM tutorials

## Installation

Follow the [installation guide](https://fastddm.readthedocs.io/en/stable/) on the documentation.
FastDDM was tested on different OSs and python versions.
If you experience troubles installing the library, this might be due to incompatibilities in the system path variables or with the compiler used.

If you still want to test the software, we provide the following two workarounds: Google colab notebook and Docker container.

### Google colab

To use [Google colab](https://colab.research.google.com/), open a new notebook.
If you want to use the GPU provided by the servers, change the runtime type as shown below

![Google colab runtime example](google_colab_runtime.png "Google colab runtime example")

You can check what GPU is available by executing inside a cell

```python
!nvidia-smi
```

Then, you want to download and install FastDDM to be used in the notebook.
Run the following lines in a cell:

```python
!git clone https://github.com/somexlab/fastddm.git
%cd fastddm
```

If you want to use the GPU, enable the CUDA option before installing the library

```python
%env ENABLE_CUDA=ON
```

Since the RAM available on colab is limited, we also suggest to enable the single precision option.

```python
%env SINGLE_PRECISION=ON
```

Install the library

```
!python3 -m pip install .
```

Run the tests

```python
python3 -m pip install pytest-regtest
pytest -v
```

Now, you will be able to use FastDDM from the notebook.

### Dockerfile

The dockerfile

Create the following ``Dockerfile``

```dockerfile
# Use ubuntu 22.04
FROM ubuntu:22.04

# Set the working directory in the container
WORKDIR /work

# Install dependencies
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata
RUN apt-get install -y vim build-essential git cmake net-tools gdb clang python3-dev python3-pip
RUN python3 -m pip install --upgrade pip setuptools wheel pytest pytest-regtest

# Clone and install FastDDM
RUN git clone --depth 1 https://github.com/somexlab/fastddm.git /work/fastddm
WORKDIR /work/fastddm
ENV ENABLE_CPP ON
# Uncomment this if you want to enable single precision
#ENV SINGLE_PRECISION ON
RUN python3 -m pip install .
RUN rm -rf build fastddm.egg-info
WORKDIR /work
```

Create the image executing the following line

```bash
docker build -t my-fastddm-image .
```

Run the container

```bash
docker run -it --name my-fastddm-container my-fastddm-image
```

**Missing instructions to enable jupyter notebooks from docker.**