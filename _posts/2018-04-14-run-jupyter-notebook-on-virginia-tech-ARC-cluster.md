---
title: "Run Jupyter Notebook on Virginia Tech ARC Cluster"
categories:
  - Programming
tags:
  - Python
  - parallel computing
---
A method of running jupyter on VT ARC cluster was already posted in [this page](https://www.arc.vt.edu/jupyter-notebook/). However, using ssh X forwarding can be very slow and laggy. To make the web interface more responsive, a better solution is to use ssh tunneling. After some experiments, I finally figured out how to set up jupyter notebook on a computing node of VT ARC cluster. All the steps are listed below
## 1. Submit a Job to Request a Computing Node on ARC
Below is my shell script for requesting a computing node ARC. This shell script requested 24 hours of wall time and 24 cores on one node. Note that the command `sleep 24h` was intentionally used to keep this job running during the entire wall time. 
```shell
# request 1 node with 24 cores and 5GB memory for each core.
#PBS -l nodes=1:ppn=24,pmem=5gb

#### Walltime ####
#PBS -l walltime=24:00:00

#### Queue ####
#PBS -q normal_q

#### Account ####
#PBS -A my_allocation

# Access group. Do not change this line.
#PBS -W group_list=newriver

# Uncomment and add your email address to get an email when your job starts, completes, or aborts
# #PBS -M alexsong@vt.edu
# #PBS -m bea

# Change to the directory from which the job was submitted
cd $PBS_O_WORKDIR

# Keep the job occupying the computing node during walltime
sleep 24h

exit;
```
Submit this shell script by `qsub` at the login node
## 2. Start Jupyter Notebook in the Computing Node
Use `checkjob -v job_id` to check the status of the submitted job. Once it starts running, the screen output of this command should tell you the host name of the computing node on which the job is running.

Next, log into the computing node by 
```shell
ssh -L 9999:localhost:8888 username@newriver1.arc.vt.edu -t ssh -L 8888:localhost:8888 alexsong@hostname
```
`-L 9999:localhost:8888` tells ssh on my machine to build a tunnel with the ARC login node `newriver1.arc.vt.edu` and bind the port `9999` to the port `8888` on login node. The second half of `ssh` is doing the same thing between the login node and the computing node we have just requested. This is called multi-hop ssh tunneling.

Once inside computing node, start the jupyter notebook by
```shell
nohup jupyter notebook --no-browser > jupyter.output &
```
## 3. Connect to Jupyter Notebook
Start any browser and type in `localhost:9999`, if everything was set up properly, you will see the jupyter interface :-)!  

