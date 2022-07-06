Note: You will need the virtualenv package installed to use venv. On a Mac, you may need to either pip install or conda install it depending on which python3 path you use.

Below are the steps again to refresh your memory! In the steps below, venv is the name of the environment created, but you can name the environment anything you want.

mk a dir and cd into it:
mkdir local_env

Create a Python3 virtual environment: 
python3 -m venv venv

which python3

Activate the virtual environment: 
source venv/bin/activate

which python3

Upgrade pip if you need to (optional): 
pip install --upgrade pip

Install pylint: 
pip install pylint

Install the code formatting tool black: 
pip install black

Install the testing library pytest: 
pip install pytest

Install the interactive interpreter ipython: 
pip install ipython

Test it out by typing ipython and running some code:

ipython
import os
os.listdir(".")

Setting up the environment!
Complete the steps below to set up your environment.
Create venv
Install pylint
Install black
Install pytest
Install ipython

