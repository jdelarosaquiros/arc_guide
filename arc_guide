# Arc Video Guide

https://youtu.be/PXJh-3_tDX0

1. Create a conda environment for your repo
2. Export your env to a `environment.yml`
3. Log in to arc & grab GPU node.
4. Download your code from github
5. Create environment from `environment.yml`
6. Run code.
7. A note on using branching in git

# Logging into arc and starting an interactive session with a single v100 GPU node
```bash
ssh abc123@arc.utsa.edu
srun -p gpu1v100 -n 1 -t 01:30:00 -c 40 --pty bash

# make sure your code goes to /work/abc123 dir (it can also goto /home/abc123 if you want)
# but your data MUST go to /work/abc123
cd /work/abc123

git clone https://github.com/SecureAIAutonomyLab/python-package.git
cd python-package # you will now be ready to set up a conda environment
```

# Creating new conda environment, and then installing what you need manually
```bash
conda create -n my_env python=3.10

conda activate ./env

# install pytorch, go here for other versions (https://pytorch.org/get-started/locally/)
# The following are needed for this example.
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
conda install -c huggingface transformers
conda install -c conda-forge datasets

# install any other dependies with conda, if they don't exist, you can use pip install instead
# limit the use of pip install unelss it is necessary. If you have a requirements.txt you can do the following

pip install -r requirements.txt # okay to do, but try to avoid

conda env export > environment.yml

```
# Installing miniconda on /home/abc123
```bash
# The miniconda install will persist in your home directory, you need only do this once
mkdir -p ~/miniconda3 && \
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh && \
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3 && \
rm -rf ~/miniconda3/miniconda.sh && \
~/miniconda3/bin/conda init bash && \
exec bash

# Need to run this everytime you log in, unelss you add the export to your ~/.bashrc
# Important to avoid exceeding disk quota
export CONDA_PKGS_DIRS=/work/zwe996/.conda/pkgs && \
mkdir -p /work/zwe996/.conda/pkgs
```

# In general, if you run into a `disk quota exceed` error it just means you need to change an environment variable.

For example, anytime you download very large models or datasets, you will likely have to make a change to avoid too much data being temporarily cached in your home directory

```bash
# Need to run this everytime you log in, unelss you add the export to your ~/.bashrc
export HF_HOME=/work/zwe996/.cache/huggingface && \
mkdir -p /work/zwe996/.cache/huggingface
```


# Creating existing conda environment from `environment.yml` file
Use this section if you already have a conda `environment.yml` file that specifys all your dependencies

```bash
conda env create --prefix ./env --file environment.yml

conda activate ./env

```

Now your environment is set up, and you are ready to run your code.

# Guide for Slurm Scripting

For long-running jobs, you may want to consider using a slurm script. However, before using a slurm script, you should make sure to set up your computing environment in an interactive session (as shown above with `srun`) to make sure that your code will run (set up your conda environment, move your data to `/work/abc`, etc).

Below is a sample script which covers the following steps:

1. Requests resources from Slurm
2. Activates a conda environment
3. Executes Python code

Example `job_script.slurm`
```bash
#!/bin/bash
#SBATCH --job-name=python_job       # Create a short name for your job, required
#SBATCH --nodes=1                   # Number of nodes, required
#SBATCH --ntasks=40                 # Number of CPU cores to use, required
#SBATCH --time=01:00:00             # Time limit hrs:min:sec, required
#SBATCH --output myjob.o%j          # Name of the stdout output file, required
#SBATCH --error myjob.e%j           # Name of stderr error file 
#SBATCH --partition=gpu1v100        # Specify the name of the GPU partition, required

eval "$(conda shell.bash hook)"
conda activate /work/abc123/my_env/  # Activate your conda environment from dir
export HF_HOME=/work/abc123/.cache/huggingface # You may not need to do these exports
export CONDA_PKGS_DIRS=/work/abc123/.conda/pkgs # Only if you're having disk quota exceeded errors

python /work/abc123/python-package/script.py      # Execute your Python script
```
run the script by doing:

`[abc123@login003 ~]$ sbatch job_script.slurm`

Once you've started the job, see more documentation here to view its progress.

https://hpcsupport.utsa.edu/foswiki/pub/ARC/WebHome/Running_Jobs_On_Arc.pdf

# Arc Documentation Link

https://hpcsupport.utsa.edu/foswiki/bin/view/ARC/WebHome

# Allowed GPUs

| Name        | GPU Node Name |
| ----------- | ----------- |
| nafis_islam      | gpu4v100       |
| dylan_manuel   | gpu2v100, gpu1v100	|
| paul_young   | gpu2v100, gpu1v100		|
| isaac_corley   | gpu4v100	|
| mazal_bethany   | gpu2v100, gpu1v100	|
| emet_bethany   | gpu2v100, gpu1v100		|
| rinu_joseph   | gpu1v100	|
| ana_nhunez   | gpu1v100	|
| javier_quiros   | gpu1v100	|
| abdalwahab_almajed   | Off	|
