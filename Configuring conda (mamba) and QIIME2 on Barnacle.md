# Configuring conda (mamba) and QIIME2 on Barnacle

Owner: Monica Dai, Lucas Patel
Tags: Guides and Processes

Conda is a package management system used to create development environments for Python. It’s allows the existence of different versions of the same package through different environments, which greatly simplifies compatibility between different project package requirements. Mamba is a reimplementation of conda, which is generally faster than conda and thus preferred over it.

This guide outlines the process for installing Mamba and configuring a QIIME2 environment on the Barnacle cluster

## Installing mamba

After logging onto Barnacle, copy and paste the following commands into the terminal:

```bash
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
```

After completing installation, you should see `(base)` appear in front of the command prompt. This indicates that the current active conda environment is your base environment.

## Setting up QIIME2 environment

QIIME2 currently has four different distributions available for installation:

- Amplicon: designed specifically for amplicon-based data analysis
- Metagenome: focuses on whole-metagenome sequencing (WMS) data
- Pathogenome: intended for pathogen-associated sequencing data
- Tiny: designed for lightweight use, such as teaching, testing, or developing

For this guide, we will install the metagenome distribution. Enter the following command to get the yml file needed for creating our environment:

```bash
wget https://data.qiime2.org/distro/metagenome/qiime2-metagenome-2024.10-py310-linux-conda.yml
```

Note that we are installing the Linux version because we are using Barnacle instead of your local computer.

Now if you call `ls`, which is a command that lists all directories and files in the current working directory, you should be able to see a file called `qiime2-metagenome-2024.10-py310-linux-conda.yml`.

This file contains all the dependencies and their versions needed for the environment, and we have to edit it such that the `c-ares` dependency uses version 1.32.3 instead of 1.34.2. We will use Vim to do so, which allows us to read and write files through the terminal. To open the yml file, enter `vim` then the name of the file like this:

```bash
vim qiime2-metagenome-2024.10-py310-linux-conda.yml
```

<aside>
💡

Tip: Instead of typing the entire file name, you can press `TAB` for bash to autofill for you.

</aside>

Once the file is open, you can scroll using your cursor, move around with the arrow keys, or use these keys:

- j: down
- k: up
- h: right
- l: left
- i: insert (enters write mode)

To find the line with our target dependency, type the following command:

```bash
/c-ares
```

This will take you to a line that says `- c-ares=1.34.2`. Then, press `i` to enter insert mode, and modify the version to be 1.32.3 instead of 1.34.2. To save changes and quit, follow these steps:

```bash
# [PRESS ESC] to exit insert mode

# [PRESS ESC] again

:wq # this indicates that you want to write and then quit.
# if you want to quit without writing, just type ':q'

# [PRESS ENTER]
```

And you will be taken back to your terminal. Now we are ready to create the environment.  We will use mamba since it’s usually faster than conda, but it may still take a while since we are installing many dependencies. Call this command to initiate installation, and make sure to stay connected to UCSD Wi-Fi and do not close your terminal nor your computer:

```bash
mamba env create -n qiime2-metagenome-2024.10 --file qiime2-metagenome-2024.10-py310-linux-conda.yml
```

After the process completes, you should be able to see `qiime2-metagenome-2024.10` in your list of environments when calling `conda env list`. To activate it, simply call:

```bash
mamba activate qiime2-metagenome-2024.10
```
