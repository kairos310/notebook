# VirtualFlow for Virtual Screenings (VFVS)

## General

VirtualFlow is a versatile, parallel workflow platform for carrying out virtual screening related tasks on Linux-based computer clusters of any type and size which are managed by a batch system (resource manager).

### Prerequisites

There are no prerequisites to running Virtual-Flow, you can simply clone the current directory and get it running. Please note that obtaining the source from [virtual-flow.org](https://virtual-flow.org) will not run on St. Jude HPC. You have to use this modified version to submit your jobs to St. Jude HPC.

## Installation

There is not installation required to get VFVS running. Simply clone this directory at a desired location.
```bash
git clone https://git.stjude.org/scm/hprc/vfvs.git
```

## Usage

There are multiple steps that are involved in getting VFVS running.

* First, you have to prepare the directory with your settings. To do that, there is a command in `tools` that will help you.
```bash
cd vfvs/tools
./vf_prepare_folders.sh
```

## Setting up the workflow

### Preparing the `tools/templates/all.ctrl` File

[virtual-flow docs](https://docs.virtual-flow.org/tutorials-vf1/vfvs-tutorial-1-bash/setting-up-the-workflow)

As a next step, the file `tools/templates/all.ctrl` needs to be edited. It needs to be adjusted according to the cluster/batch system which will be used. In particular, the following settings are cluster dependent:
- `batchsystem`: The resource manager which is used by your cluster. 
	- `batchsystem=LSF`
- `partition`: The partition queue to be used for running the jobs of this workflow/tutorial. 
	- use `bqueues` to list available queues
	- default `partition=rhel8_standard`
- `timelimit`: This parameter specifies the `timelimit` parameter of the jobs which are submitted by to batch system by VFVS. Each partition/queue has normally a maximum partition time-limit. The `timelimit` parameter should be set to a value below or equal to the partition time-limit. Otherwise, the batchsystem might terminate the job without VFVS being able to start a successor job to continue the work of the current job.
	- default. `timelimit=07:00`

#### Using Entire Nodes (optional)

Very few clusters require that full nodes are used, and sometimes even a minimum number of the them. In this case, the following settings need to be adjusted as well:
- `steps_per_job`: This equals the number of nodes which should be used per job, and be at least as large as the minimum number of nodes per job since one job step is used for each node.
- `cpus_per_step`: In this case where entire nodes are used, this should be set to the number of CPUs per node to utilize them fully.
- `queues_per_step`: This should be set to the number of `cpus_per_step`, since in this case we have one cpu per queue (i.e. the docking programs which are executed within the queues are set to use just one CPU core).
- `cpus_per_queue`: This is the number of CPU cores which are used per queue, and thus also per docking program instance. Normally, it is most efficient to set up the workflow such that each docking program uses one CPU core, and that there is one queue per core.


### Preparing the Job File

For submitting jobs into batch systems on clusters job files are needed. How they look like depends on the type of batch system. For most batch systems VirtualFlow has already pre-configured job files stored in the `tools/templates/` folder:
- **LFS:** `template1.lfs.sh`

## Preparing the `workflow` and `output-files` Folders

All other files are already set up, in particular the `tools/templates/todo.all`, which contains the ligand collections which should be processed/screened, as well as the docking input files in the folder `input-files` which also contains the input ligand database.

This means, we are ready to prepare the folders, which can be done with the command `vf_prepare_folders.sh`. The command will delete old files from previous runs if present, and prepare the workflow folder which is used by VirtualFlow during the runtime to organize and log the workflow. To prepare the folders, in the `tools` folder simply run the command

`./vf_prepare_folders.sh`

The command will ask you if you really want to reset/prepare the relevant folders, since this may delete files from previous runs.

## Starting the workflow

After preparing the workflow, it can be started with the command `vf_start_joblines.sh`

ex. To start 2 jobs in parallel on a LSF cluster, the following command can be used (within thetools directory): 
`./vf_start_jobline.sh 1 2 templates/template1.lsf.sh submit 1`

## Monitoring the workflow

The workflow can for instance be monitored by the command `vf_report.sh`.

To get information about the workflow in general, within the tools folder one can run
`./vf_report.sh -c workflow`

To get preliminary virtual screening results for the docking scenario *qvina02_rigid_receptor1*, one can run the command (within the `tools` folder):

`./vf_report.sh -c vs -d qvina02_rigid_receptor1 -n 10`
The -n option here specifies that we would also like to have the top 10 compounds listed.
