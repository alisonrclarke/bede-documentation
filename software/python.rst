Python
------

PyTorch Quickstart
~~~~~~~~~~~~~~~~~~
The following should get you set up with a working conda environment (replacing <project> with your project code):

::

    export DIR=/nobackup/projects/<project>/$USER
    # rm -rf ~/.conda .condarc $DIR/miniconda # Uncomment if you want to remove old env
    mkdir $DIR
    pushd $DIR

    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-ppc64le.sh

    sh Miniconda3-latest-Linux-ppc64le.sh -b -p $DIR/miniconda
    source miniconda/bin/activate
    conda update conda -y
    conda config --set channel_priority strict

    conda config --prepend channels \
            https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda/

    conda config --prepend channels \
            https://opence.mit.edu

    conda create --name opence pytorch=1.7.1 -y
    conda activate opence


This has some limitations such as not supporting large model support. If you require this you can try the instructions below, these provide an older version of PyTorch however.
