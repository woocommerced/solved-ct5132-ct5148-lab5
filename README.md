Download Link: https://assignmentchef.com/product/solved-ct5132-ct5148-lab5
<br>
<h1>Creating an environment for Python scientific computing</h1>

1.Create a new Conda environment called myenv, and install some libraries in it:

module load conda/2 module load cuda/10.0 conda create –name myenv python=3.7 source activate myenv

conda install matplotlib cma # our fruit-picking example will use them conda install tensorflow-gpu conda scikit-learn

Installing Python packages using conda like this (or using pip) can only be done from the login node – not from a node you might acquire using srun as below. Also, it can only be done after running module load conda/2 and source activate myenv as shown above.

2.Try to acquire an interactive GPU node for 10 minutes like this (use your account_name from 3 or 4 above – don’t confuse this with your username): srun -p GpuQ -N 1 -A account_name -t 0:10:00 –pty bash If it brings you to a prompt such as [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f98c8a9c8b9798949cb997c8">[email protected]</a> ~]$ then you have an interactive node and can proceed. Otherwise it might wait until a node becomes available. In that case, if you want you can type Ctrl-C and try again later.

3.Test that Tensorflow can be imported and sees the GPU.

# Do these three commands once at the start of the session on a login node module load cuda/10.0 module load conda/2 source activate myenv

# From now on, you can run Python and import Tensorflow python -c ‘import tensorflow as tf; tf.test.gpu_device_name()’

4.Note: if you are using Tensorflow or another library that will take advantage of the GPU, then use GpuQ as above. Otherwise, use DevQ for normal CPUonly nodes, and omit the cuda line.

<h1>Batch jobs and Taskfarming</h1>

5.Above we acquired a node for short-term interactive use. For long-running jobs, we instead use <em><sub>batch jobs</sub></em>. That means we write a shell script and submit it to a queue. Many users submit to these queues and when there is capacity, your job is taken off the queue and run.

See submit.sh for an example batch job. It has a specific format: first it gives details such as your account name and the queue you are submitting to. Then it loads some modules and your Python environment. Then it runs one or more commands. These could be Python scripts, e.g. python hello.py, or <em><sub>taskfarming </sub></em>commands. In the example submit.sh, there is a single taskfarming command.

Several commands allow you to query the batch job system:

$ mybalance

$ sinfo

$ squeue # eg `squeue | grep username`

$ scancel # use `man scancel` to read options

In particular, if you have submitted a job and it doesn’t seem to have started, you can use squeue | grep username to look for it. Usually, you just have to wait a day or two. But you might see AssocGrpBillingMinutes in the final column: this means that the project has exceeded its resource limits and your job may not run. E.g. on nuig02, it will not run until the next month. If this happens on nuig02, you can consider applying for your own Class-C project – see later.

6.Taskfarming means running many similar commands, taking advantage of multiple CPUs on a single node, or even multiple nodes.

For example, if we have a machine learning algorithm with some hyperparameters, we could try many values for the hyperparameters using taskfarming. In the example, submit.sh runs the command taskfarm taskfarm.sh. See taskfarm.sh to see the format it should be in: a list of shell commands. Each line will be run independently, on its own CPU, so many will run at the same time. If there are more lines than CPUs, then a new line is run automatically whenever a CPU becomes free. It’s like a smaller version of the batch-job queueing system we already discussed, but now it refers to multiple <em><sub>tasks </sub></em>(commands) being queued on nodes which already “belong” to you, as opposed to multiple <em><sub>batch jobs </sub></em>from multiple users being queued for access to nodes.

7.Next we will run an example taskfarming batch job. On your laptop, edit submit.sh to include your NUI Galway email address and check the account_name is correct. Look at fruit_picking.py: you don’t need to understand this example experiment, but notice it doesn’t use Tensorflow, hence submit.sh requests DevQ, not GpuQ. Similarly, it doesn’t load cuda since that is for GPU only.

Use scp, Putty, or another remote file-copying program to copy the fruit-picking code and data files, and the submit.sh and taskfarm.sh files to kay.ichec.ie:/ichec/home/users/&lt;username&gt;. E.g.: scp -r fruit_picking <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d5a0a6b0a7bbb4b8b095beb4acfbbcb6bdb0b6fbbcb0">[email protected]</a>:/ichec/home/users/username.

8.Back on the kay login node (not on an interactive one), type ls to confirm your files are present. If you know how to use cd, mv, etc., then you can create a sensible working directory structure here. The files will be mirrored from the login node to interactive nodes and batch nodes.

Make sure you are in the directory where you have the files submit.sh, taskfarm.sh, fruit_picking.py, and fruit_picking.dat. Then type sbatch submit.sh. It will tell you that the batch job has been submitted. However, it might not run immediately. You will receive an email both when the job begins executing and when it ends (look at submit.sh to see where you requested this). This very small job will take only a couple of minutes, and we have requested 1 node for 10 minutes (look at submit.sh to see how). If you want, you can exit from kay by typing Ctrl-D at the shell prompt.

9.When you receive an email to say the job is finished, log back in to kay and have a look at the output files. Then copy all the output files and images back to your own machine.

<h1>Applying for your own Class-C project</h1>

For typical assignments, nuig02 condominium access should be enough. However, if you have a larger Capstone project or PhD research, you have the option to create your own Class-C project. It’s quite easy: you can do it in a couple of hours and receive the approval within a week. Talk to your supervisor first.

Go here: <a href="https://www.ichec.ie/academic/national-hpc/national-service-projects">https://www.ichec.ie/academic/national-hpc/national-service-projects, </a>read the part about Class-C, and download the project template, a Word doc. In the doc, describe your research project very briefly, justifying the need for high-end computing.

Use the Core Hour Calculator on the same page to create a table estimating your ICHEC usage, and add that to your doc.

Then go to <a href="https://register.ichec.ie/login/project_apply">https://register.ichec.ie/login/project_apply,</a> fill in the form and upload your doc.

<h1>ICHEC documentation for further reading</h1>

<ul>

 <li>Quick start         guide:      <a href="https://www.ichec.ie/academic/national-hpc/documentation">https://www.ichec.ie/academic/national-hpc/ </a><a href="https://www.ichec.ie/academic/national-hpc/documentation">documentation</a></li>

 <li>Tutorials <a href="https://www.ichec.ie/academic/national-hpc/tutorials">https://www.ichec.ie/academic/national-hpc/tutorials</a></li>

 <li>Kay user guide <a href="https://www.ichec.ie/academic/national-hpc/kay-user-guide">https://www.ichec.ie/academic/national-hpc/kay-user</a><a href="https://www.ichec.ie/academic/national-hpc/kay-user-guide">guide</a></li>

 <li>Python, Anaconda, and environments on ICHEC: <a href="https://www.ichec.ie/academic/national-hpc-service/software/python-conda">https://www.ichec.ie/ </a><a href="https://www.ichec.ie/academic/national-hpc-service/software/python-conda">academic/national-hpc-service/software/python-conda</a></li>

 <li>Conda environments:       <a href="https://www.ichec.ie/academic/national-hpc/documentation/tutorials/conda-environments">https://www.ichec.ie/academic/national</a><a href="https://www.ichec.ie/academic/national-hpc/documentation/tutorials/conda-environments">hpc/documentation/tutorials/conda-environments</a></li>

 <li>More on SLURM: <a href="https://www.ichec.ie/academic/national-hpc/kay-documentation/slurm-workload-manager">https://www.ichec.ie/academic/national-hpc/kay</a><a href="https://www.ichec.ie/academic/national-hpc/kay-documentation/slurm-workload-manager">documentation/slurm-workload-manager</a></li>

 <li>SLURM commands: <a href="https://www.ichec.ie/academic/national-hpc/kay-documentation/slurm-commands">https://www.ichec.ie/academic/national-hpc/kay</a><a href="https://www.ichec.ie/academic/national-hpc/kay-documentation/slurm-commands">documentation/slurm-commands</a></li>

 <li>SLURM for PBS users (still has some useful clues): <a href="https://www.ichec.ie/academic/national-hpc/kay-documentation/pbs-slurm">https://www.ichec.ie/ </a><a href="https://www.ichec.ie/academic/national-hpc/kay-documentation/pbs-slurm">academic/national-hpc/kay-documentation/pbs-slurm</a></li>

 <li>Task farming: <a href="https://www.ichec.ie/academic/national-hpc/documentation/task-farming">https://www.ichec.ie/academic/national-hpc/documentation</a>/ <a href="https://www.ichec.ie/academic/national-hpc/documentation/task-farming">task-farming</a></li>

 <li>Hardware available: <a href="https://www.ichec.ie/about/infrastructure/kay">https://www.ichec.ie/about/infrastructure/kay</a></li>

 <li>Software available (only relevant for hardcore HPC (simulation, weather forecasting, etc, not relevant to those using the Python scientific stack): <a href="https://www.ichec.ie/academic/national-hpc-service/software">https://www.ichec.ie/academic/national-hpc-service/software</a></li>

 <li>Applying for your own project: <a href="https://www.ichec.ie/academic/national-hpc/national-service-projects">https://www.ichec.ie/academic/national</a><a href="https://www.ichec.ie/academic/national-hpc/national-service-projects">hpc/national-service-projects</a></li>

</ul>