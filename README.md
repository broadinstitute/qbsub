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
