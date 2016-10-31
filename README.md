# qbsub
qsub replacement to make it behave more similarly to bsub

Replacement for qsub to make it behave more rationally and similar to bsub. For
now it is also handles dotkits, although in principle it could be used to also
handle Environment Modules.

In addition to adding `-cwd` and `-V` by default (passing current working directory
and environment variables), also by default joins stdout and stderr files if
only a `-o` option is specified. If you want separate stdout and stderr, simply
add the `-e` option.

The dotkits of the local environment are 'reuse'd at the beginning of the
wrapper script sent to qsub, in the same order as locally loaded. This can be
disabled with `--nouse`.

The convenience argument -n for number of processors will automatically set the
multicore arguments `-pe $QSUB_PE n -binding linear:n`.

The `--profile` option will prepend your job logs with host information before job
execution and append resource utilization data at the conclusion of your job.
This is useful to keep track of things like cpu utilization and maximum memory
used during the course of the job.

Use `--verbose` to see the qsub command and accompanying wrapper script.

All other arguments are passed through to qsub.

Usage
-----
`qbsub -q short python test.py`

Options
-------
```
-n: Number of processors - equivalent -pe $QBSUB_PE
--nocwd: Don't pass -cwd
--noenv: Don't pass -V
--nouse: Don't reuse dotkits from current environment
--nonotify: Don't apply the -notify flag which sends SIGUSR2 before jobs are
  killed (aka send SIGKILL directly on qdel)
--profile: Profile cpu/mem usage and log jobid/host info.
--verbose: Print out qsub command and generated wrapper script
--help: Print the message
*: Other commands same as qsub
```

Environment Variables
---------------------
* `DK_INIT`: The path of the dotkit init script - required.
* `QBSUB_PROJECT_NAME`: Project name to pass to qsub. Can be overriden by the `-P`
    option.
* `QBSUB_PE`: Default `-pe` environment to use - for use with `-n`.
