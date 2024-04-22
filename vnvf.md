# Setting up the workflow

[virtual-flow docs](https://docs.virtual-flow.org/tutorials-vf1/vfvs-tutorial-1-bash/setting-up-the-workflow)

## Preparing the `tools` Folder

### Preparing the `tools/templates/all.ctrl` File

As a next step, the file `tools/templates/all.ctrl` needs to be edited. It needs to be adjusted according to the cluster/batch system which will be used. In particular, the following settings are cluster dependent:
- `batchsystem`: The resource manager which is used by your cluster. 
	- `batchsystem=LSF`
- `partition`: The partition queue to be used for running the jobs of this workflow/tutorial. 
	- use `bqueues` to list available queues
	- default `partition=rhel8_standard`
- `timelimit`: This parameter specifies the `timelimit` parameter of the jobs which are submitted by to batch system by VFVS. Each partition/queue has normally a maximum partition time-limit. The `timelimit` parameter should be set to a value below or equal to the partition time-limit. Otherwise, the batchsystem might terminate the job without VFVS being able to start a successor job to continue the work of the current job.
	- default. `timelimit=07:00`

Everything else is set up in a way which should work on most clusters. The jobs are pre-configured such that each job uses one CPU-core on one node.

#### Using Entire Nodes (optional)

Very few clusters require that full nodes are used, and sometimes even a minimum number of the them. In this case, the following settings need to be adjusted as well:
- `steps_per_job`: This equals the number of nodes which should be used per job, and be at least as large as the minimum number of nodes per job since one job step is used for each node.
- `cpus_per_step`: In this case where entire nodes are used, this should be set to the number of CPUs per node to utilize them fully.
- `queues_per_step`: This should be set to the number of `cpus_per_step`, since in this case we have one cpu per queue (i.e. the docking programs which are executed within the queues are set to use just one CPU core).
- `cpus_per_queue`: This is the number of CPU cores which are used per queue, and thus also per docking program instance. Normally, it is most efficient to set up the workflow such that each docking program uses one CPU core, and that there is one queue per core.


# Starting the workflow




### Preparing the Job File

For submitting jobs into batch systems on clusters job files are needed. How they look like depends on the type of batch system. For most batch systems VirtualFlow has already pre-configured job files stored in the `tools/templates/` folder:   
- **LFS:** `template1.lfs.sh`

## Preparing the `workflow` and `output-files` Folders

All other files are already set up, in particular the `tools/templates/todo.all`, which contains the ligand collections which should be processed/screened, as well as the docking input files in the folder `input-files` which also contains the input ligand database.

This means, we are ready to prepare the folders, which can be done with the command `vf_prepare_folders.sh`. The command will delete old files from previous runs if present, and prepare the workflow folder which is used by VirtualFlow during the runtime to organize and log the workflow. To prepare the folders, in the `tools` folder simply run the command

`./vf_prepare_folders.sh`

The command will ask you if you really want to reset/prepare the relevant folders, since this may delete files from previous runs.

To get preliminary virtual screening results for the docking scenario _qvina02_rigid_receptor1_, one can run the command (within the `tools` folder):

To get preliminary virtual *screening* results for the docking scenario _qvina02\_rigid\_receptor1_, one can run the command (within the    `tools` folder):

 1 

83 `./vf_report.sh -c vs -d qvina02**_**rigid**_**receptor1 -n 10`