---
created: 2025-06-19 14:09
updated: 2025-06-23 09:24
---


```shell
sbatch -h
Usage: sbatch [OPTIONS(0)...] [ : [OPTIONS(N)...]] script(0) [args(0)...]

Parallel run options:
  -a, --array=indexes         job array index values
  -A, --account=name          charge job to specified account
      --bb=<spec>             burst buffer specifications
      --bbf=<file_name>       burst buffer specification file
  -b, --begin=time            defer job until HH:MM MM/DD/YY
      --comment=name          arbitrary comment
      --cpu-freq=min[-max[:gov]] requested cpu frequency (and governor)
  -c, --cpus-per-task=ncpus   number of cpus required per task
  -d, --dependency=type:jobid[:time] defer job until condition on jobid is satisfied
      --deadline=time         remove the job if no ending possible before
                              this deadline (start > (deadline - time[-min]))
      --delay-boot=mins       delay boot for desired node features
  -D, --chdir=directory       set working directory for batch script
  -e, --error=err             file for batch script standard error
      --export[=names]        specify environment variables to export
      --export-file=file|fd   specify environment variables file or file
                              descriptor to export
      --get-user-env          load environment from local cluster
      --gid=group_id          group ID to run job as (user root only)
      --gres=list             required generic resources
      --gres-flags=opts       flags related to GRES management
  -H, --hold                  submit job in held state
      --ignore-pbs            Ignore #PBS and #BSUB options in the batch script
  -i, --input=in              file for batch script standard input
  -J, --job-name=jobname      name of job
  -k, --no-kill               do not kill job on node failure
  -L, --licenses=names        required license, comma separated
  -M, --clusters=names        Comma separated list of clusters to issue
                              commands to.  Default is current cluster.
                              Name of 'all' will submit to run on all clusters.
                              NOTE: SlurmDBD must up.
      --container             Path to OCI container bundle
  -m, --distribution=type     distribution method for processes to nodes
                              (type = block|cyclic|arbitrary)
      --mail-type=type        notify on state change: BEGIN, END, FAIL or ALL
      --mail-user=user        who to send email notification for job state
                              changes
      --mcs-label=mcs         mcs label if mcs plugin mcs/group is used
  -n, --ntasks=ntasks         number of tasks to run
      --nice[=value]          decrease scheduling priority by value
      --no-requeue            if set, do not permit the job to be requeued
      --ntasks-per-node=n     number of tasks to invoke on each node
  -N, --nodes=N               number of nodes on which to run (N = min[-max])
  -o, --output=out            file for batch script standard output
  -O, --overcommit            overcommit resources
  -p, --partition=partition   partition requested
      --parsable              outputs only the jobid and cluster name (if present),
                              separated by semicolon, only on successful submission.
      --power=flags           power management options
      --priority=value        set the priority of the job to value
      --profile=value         enable acct_gather_profile for detailed data
                              value is all or none or any combination of
                              energy, lustre, network or task
      --propagate[=rlimits]   propagate all [or specific list of] rlimits
  -q, --qos=qos               quality of service
  -Q, --quiet                 quiet mode (suppress informational messages)
      --reboot                reboot compute nodes before starting job
      --requeue               if set, permit the job to be requeued
  -s, --oversubscribe         over subscribe resources with other jobs
  -S, --core-spec=cores       count of reserved cores
      --signal=[[R][B]:]num[@time] send signal when time limit within time seconds
      --spread-job            spread job across as many nodes as possible
      --switches=max-switches{@max-time-to-wait}
                              Optimum switches and max time to wait for optimum
      --thread-spec=threads   count of reserved threads
  -t, --time=minutes          time limit
      --time-min=minutes      minimum time limit (if distinct)
      --uid=user_id           user ID to run job as (user root only)
      --use-min-nodes         if a range of node counts is given, prefer the
                              smaller count
  -v, --verbose               verbose mode (multiple -v increase verbosity)
  -W, --wait                  wait for completion of submitted job
      --wckey=wckey           wckey to run job under
      --wrap[=command string] wrap command string in a sh script and submit

Constraint options:
      --cluster-constraint=[!]list specify a list of cluster constraints
      --contiguous            demand a contiguous range of nodes
  -C, --constraint=list       specify a list of constraints
  -F, --nodefile=filename     request a specific list of hosts
      --mem=MB                minimum amount of real memory
      --mincpus=n             minimum number of logical processors (threads)
                              per node
      --reservation=name      allocate resources from named reservation
      --tmp=MB                minimum amount of temporary disk
  -w, --nodelist=hosts...     request a specific list of hosts
  -x, --exclude=hosts...      exclude a specific list of hosts

Consumable resources related options:
      --exclusive[=user]      allocate nodes in exclusive mode when
                              cpu consumable resource is enabled
      --exclusive[=mcs]       allocate nodes in exclusive mode when
                              cpu consumable resource is enabled
                              and mcs plugin is enabled
      --mem-per-cpu=MB        maximum amount of real memory per allocated
                              cpu required by the job.
                              --mem >= --mem-per-cpu if --mem is specified.

Affinity/Multi-core options: (when the task/affinity plugin is enabled)
                              For the following 4 options, you are
                              specifying the minimum resources available for
                              the node(s) allocated to the job.
      --sockets-per-node=S    number of sockets per node to allocate
      --cores-per-socket=C    number of cores per socket to allocate
      --threads-per-core=T    number of threads per core to allocate
  -B  --extra-node-info=S[:C[:T]]  combine request of sockets per node,
                              cores per socket and threads per core.
                              Specify an asterisk (*) as a placeholder,
                              a minimum value, or a min-max range.

      --ntasks-per-core=n     number of tasks to invoke on each core
      --ntasks-per-socket=n   number of tasks to invoke on each socket

GPU scheduling options:
      --cpus-per-gpu=n        number of CPUs required per allocated GPU
  -G, --gpus=n                count of GPUs required for the job
      --gpu-bind=...          task to gpu binding options
      --gpu-freq=...          frequency and voltage of GPUs
      --gpus-per-node=n       number of GPUs required per allocated node
      --gpus-per-socket=n     number of GPUs required per allocated socket
      --gpus-per-task=n       number of GPUs required per spawned task
      --mem-per-gpu=n         real memory required per allocated GPU

Help options:
  -h, --help                  show this help message
      --usage                 display brief usage message

Other options:
  -V, --version               output version information and exit

```


提交任务：

```shell
sbatch -J name -p fat -c 4 -N 1 -o name.o -e name.e --account=chenyan_group sleep.sh
```

