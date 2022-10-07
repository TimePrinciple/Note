1. Install Python

    We recommend installing the Anaconda (Links to an external site.) Python distribution, as it's well-maintained, easy to install on most operating systems, and allows you to avoid conflicts with system libraries. You can download Anaconda for your operating system here (Links to an external site.), and follow the instructions here (Links to an external site.) to install.
    
    Note: On Windows we recommend choosing the default installation options, i.e. installing as a user (non-admin), and not adding Anaconda to your system's PATH environment variable. 

2. Install OpenCV and TensorFlow Using Anaconda

    (1) For Mac: Open terminal
        For Windows: Launch the Anaconda prompt from the start menu.
        For Linux: ?
    
    (2) Create a new environment: Since we need to install OpenCV and TensorFlow outside the conda package manager, it's important to use a virtual environment (Links to an external site.) to avoid polluting the base environment. To do this, enter the following command:
    ```
    conda create --name CV python=3.7
    ```
    This will create a new virtual environment called "CV" using Python version 3.7. Note that the CV environment is isolated from other environments (e.g. the base environment), so you'll need to reinstall any packages that you'd like to use with CV later on.

    (3) Activate the new environment
    ```
    conda activate CV
    ```
    Any commands you run will now reference the Python binaries and packages installed in the CV enviornment.

    (4) install OpenCV, Tensorflow and other libraries:
    ```
    conda install -c conda-forge opencv

    conda install -c anaconda tensorflow

    conda install -c anaconda numpy

    conda install pandas

    conda install -c conda-forge matplotlib

    conda install scikit-image

    conda install scikit-learn
    ```

3. Running Jupyter
    Jupyter offers two web-based user interfaces for opening notebooks: the classic Jupyter Notebook (Links to an external site.) interface and the new JupyterLab (Links to an external site.) interface. We will use Jupyter Notebook.

    If you're using Anaconda, you can start JupyterLab from Anaconda Navigator. Alternatively, you can open an Anaconda prompt, change to the directory where your notebooks reside and enter
    ```
    jupyter notebook
    ```

    The JupyterNote interface should open in your default web browser.

-----------------

Note: If you fail in installing the libraries,  remove the environment and and set it up again from step 2.(2).

    conda env remove -n CV

-----------------