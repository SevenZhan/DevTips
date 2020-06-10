# Caffe for CPU

To install OpenBLAS.

    sudo apt-get install libopenblas-dev

Now to install boost, run:

    sudo apt-get install libboost-all-dev

Now, let us install OpenCV. Go ahead and run:

    sudo apt-get install python3-pip

    pip3 install opencv-python

    sudo apt-get install libopencv-dev

Now let us install some dependencies of Caffe. Run the following:

    sudo apt-get install libleveldb-dev libsnappy-dev libhdf5-serial-dev

    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

    sudo apt-get install protobuf-compiler libprotobuf-dev
    
    sudo apt-get install python3-numpy

    pip3 install protobuf

Okay, that's it. Let us now download the Caffe. If you don't have git installed in your system yet, run this code really quick:

    sudo apt-get install git

We will clone the official [Caffe repository](https://github.com/BVLC/caffe "Caffe GitHub repo") from Github.

    git clone https://github.com/BVLC/caffe

Once the git is cloned, cd into caffe folder.

    cd caffe

We will edit the configuration file of Caffe now. We need to do it to specify that we are using a CPU-only system. (Tell compiler to disable GPU, CUDA etc). For this, make a copy of the Makefile.config.example.

    cp Makefile.config.example Makefile.config

Great ! Now go ahead and open the Makefile.config in your favourite text editor (vi or vim or gedit or ...). Change the following:

    1. Uncomment (No space in the beginning): 
    CPU_ONLY := 1

    2. Change:
    BLAS := atlas to BLAS := open

    3. Comment out:
    PYTHON_INCLUDE := /usr/include/python2.7 /usr/lib/python2.7/dist-packages/numpy/core/include
       
    4. Uncomment:
    PYTHON_LIBRARIES := boost_python3 python3.6m
    PYTHON_INCLUDE := /usr/include/python3.6m /usr/lib/python3.6/dist-packages/numpy/core/include
 

    5. Uncomment:
    PYTHON_LIB := /usr/lib

    6. Comment out:
    PYTHON_LIB := $(ANACONDA_HOME)/lib
    
    7. Add hdf5 path:
    INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
    LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/
    
    8. Uncomment:
    USE_PKG_CONFIG := 1

We will now install the libraries listed in the requirements.txt file.

    cd ~/caffe/python

    pip3 install -r requirements.txt

Now, we can safely build the files in the caffe directory. We will run the make process as 4 jobs by specifying it like <b>-j4</b>. More on it [here](http://www.tutorialspoint.com/unix_commands/make.htm " make command")

    cd ~/caffe

    make all -j4

I hope the make process went well. If not, please see which package failed by checking the logs or from terminal itself. Feel free to comment, I will help to the best of my knowledge. You can seek help from your go to friend Google or Stack Exchange as mentioned above.

Provided that the make process was successfull, continue with the rest of the installation process.

We will now make the Pycaffe files. Pycaffe is the Python interface of Caffe which allows you to use Caffe inside Python. More on it [here](http://caffe.berkeleyvision.org/tutorial/interfaces.html). We will also make distribute. This is explained in Caffe website.

    make pycaffe

Awesome! We are almost there. We just need to test whether everything went fine. For that make the files for testing and run the test.

    make test

    make runtest

If you succeed in all the tests then you've successfully installed Caffe in your system ! One good reason to smile !

Finally, we need to add the correct path to our installed modules. Using your favourite text editor, add the following to the <b>.bashrc</b> file in your <b>/home/user/</b> folder for Caffe to work properly. Please make sure you replace the < username > with your system's username.

```
#Caffe Root
export CAFFE_ROOT=/home/<username>/caffe/
export PYTHONPATH=/home/<username>/caffe/python:$PYTHONPATH
```

CHEERS ! You're done ! Now let's test if it really works.

Restart/reboot your system to ensure everything loads perfect.

    sudo reboot

Open Python and type:

    import caffe

You should be able to successfully load caffe.
Now let's start coding :)



# Appendix

## Makefile.config

    ## Refer to http://caffe.berkeleyvision.org/installation.html
    # Contributions simplifying and improving our build system are welcome!
    
    # cuDNN acceleration switch (uncomment to build with cuDNN).
    # USE_CUDNN := 1

    # CPU-only switch (uncomment to build without GPU support).
    CPU_ONLY := 1

    # uncomment to disable IO dependencies and corresponding data layers
    # USE_OPENCV := 0
    # USE_LEVELDB := 0
    # USE_LMDB := 0
    
    # uncomment to allow MDB_NOLOCK when reading LMDB files (only if necessary)
    #	You should not set this flag if you will be reading LMDBs with any
    #	possibility of simultaneous read and write
    # ALLOW_LMDB_NOLOCK := 1

    # Uncomment if you're using OpenCV 3
    # OPENCV_VERSION := 3

    # To customize your choice of compiler, uncomment and set the following.
    # N.B. the default for Linux is g++ and the default for OSX is clang++
    # CUSTOM_CXX := g++
    
    # CUDA directory contains bin/ and lib/ directories that we need.
    CUDA_DIR := /usr/local/cuda
    # On Ubuntu 14.04, if cuda tools are installed via
    # "sudo apt-get install nvidia-cuda-toolkit" then use this instead:
    # CUDA_DIR := /usr
    
    # CUDA architecture setting: going with all of them.
    # For CUDA < 6.0, comment the *_50 lines for compatibility.
    CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \
            -gencode arch=compute_20,code=sm_21 \
            -gencode arch=compute_30,code=sm_30 \
            -gencode arch=compute_35,code=sm_35 \
            -gencode arch=compute_50,code=sm_50 \
            -gencode arch=compute_50,code=compute_50

    # BLAS choice:
    # atlas for ATLAS (default)
    # mkl for MKL
    # open for OpenBlas
    BLAS := open
    # Custom (MKL/ATLAS/OpenBLAS) include and lib directories.
    # Leave commented to accept the defaults for your choice of BLAS
    # (which should work)!
    # BLAS_INCLUDE := /path/to/your/blas
    # BLAS_LIB := /path/to/your/blas
    
    # Homebrew puts openblas in a directory that is not on the standard search path
    # BLAS_INCLUDE := $(shell brew --prefix openblas)/include
    # BLAS_LIB := $(shell brew --prefix openblas)/lib
    
    # This is required only if you will compile the matlab interface.
    # MATLAB directory should contain the mex binary in /bin.
    # MATLAB_DIR := /usr/local
    # MATLAB_DIR := /Applications/MATLAB_R2012b.app
    
    # NOTE: this is required only if you will compile the python interface.
    # We need to be able to find Python.h and numpy/arrayobject.h.
    #PYTHON_INCLUDE := /usr/include/python2.7 \
            /usr/lib/python2.7/dist-packages/numpy/core/include
    # Anaconda Python distribution is quite popular. Include path:
    # Verify anaconda location, sometimes it's in root.
    # ANACONDA_HOME := $(HOME)/anaconda2
    # PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
             $(ANACONDA_HOME)/include/python2.7 \
             $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include 
    
    # Uncomment to use Python 3 (default is Python 2)
    PYTHON_LIBRARIES := boost_python3 python3.5m
    PYTHON_INCLUDE := /usr/include/python3.5m \
    #                 /usr/lib/python3.5/dist-packages/numpy/core/include
    
    # We need to be able to find libpythonX.X.so or .dylib.
    PYTHON_LIB := /usr/lib
    # PYTHON_LIB := $(ANACONDA_HOME)/lib
    
    # Homebrew installs numpy in a non standard path (keg only)
    # PYTHON_INCLUDE += $(dir $(shell python -c 'import numpy.core; print(numpy.core.__file__)'))/include
    # PYTHON_LIB += $(shell brew --prefix numpy)/lib
    
    # Uncomment to support layers written in Python (will link against Python libs)
    # WITH_PYTHON_LAYER := 1
    
    # Whatever else you find you need goes here.
    INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
    LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/
    
        # If Homebrew is installed at a non standard location (for example your home directory) and you use it for general dependencies
    # INCLUDE_DIRS += $(shell brew --prefix)/include
    # LIBRARY_DIRS += $(shell brew --prefix)/lib
    
    # Uncomment to use `pkg-config` to specify OpenCV library paths.
    # (Usually not necessary -- OpenCV libraries are normally installed in one of the above $LIBRARY_DIRS.)
    USE_PKG_CONFIG := 1
    
    BUILD_DIR := build
    DISTRIBUTE_DIR := distribute
    
    # Uncomment for debugging. Does not work on OSX due to https://github.com/BVLC/caffe/issues/171
    # DEBUG := 1
    
    # The ID of the GPU that 'make runtest' will use to run unit tests.
    TEST_GPUID := 0

    # enable pretty build (comment to see full commands)
    Q ?= @
