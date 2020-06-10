# build pytorch from source

common dependencies

    conda install numpy ninja pyyaml mkl mkl-include setuptools cmake cffi typing
    conda install -c pytorch magma-cuda90 # or [magma-cuda92 | magma-cuda100 ] depending on your cuda version
get pytorch from source

    git clone --recursive https://github.com/pytorch/pytorch
    cd pytorch
if you are updating an existing checkout

    git submodule sync
    git submodule update --init --recursive
building

    export CMAKE_PREFIX_PATH=${CONDA_PREFIX:-"$(dirname $(which conda))/../"}
    python setup.py install



# build torchvision from source

get torchvision from source
    git clone https://github.com/pytorch/vision
    cd vision
if you are updating an existing checkout
    git submodule sync
    git submodule update --init --recursive
building
    python setup.py install



# build ignite from source

get ignite from source
    git clone https://github.com/pytorch/ignite
    cd ignite
if you are updating an existing checkout
    git submodule sync
    git submodule update --init --recursive
building
    python setup.py install



# build fastai from source

get fastai from source
    git clone https://github.com/fastai/fastai
    cd fastai
building
    tools/run-after-git-clone
    pip install -e ".[dev]"



# some tips for accelerating
1. jpeg -----> libjpeg-turbo
2. Pillow -----> Pillow-SIMD
