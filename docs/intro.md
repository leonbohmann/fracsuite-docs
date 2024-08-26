---
title: Setup
sidebar_position: 1
---
## Installation

To install fracsuite and use it, the following prerequisites have to be installed:

1. python 3.10 or higher [from here](https://www.python.org/downloads/)
2. Rust [from here](www.rustup.rs)
3. (optional, if development state is to be used) git [from here](https://git-scm.com/downloads)

After this has been done, you have two options:

### Use development state

For this, navigate to a folder of your liking and start a command line. Then use

```
git clone https://github.com/leonbohmann/fracture-suite
```

Then, navigate into the folder using `cd fracture-suite`. Now, to setup the repository:

1. Checkout the development branch: `git checkout develop`
2. With the same command line, run: `py -m venv .venv`
3. Activate the environment: `.venv/Scripts/activate` (or `.../activate.bat` if in cmd)
4. Install all dependencies: `pip install -r requirements.txt` 

:::info
Most of the time there are requirements missing from the requirements.txt file in the development branch. You will have to install them manually one by one. When running fracsuite you will be notified of missing packages and their name.
:::

5. Run fracsuite once from the current console: `py -m fracsuite help`
6. Now, you can call fracsuite in every console. You may have to restart the current terminal session for the changes to apply.

### Use the latest package release

This is easier but not always up-to-date. 

1. python 3.10 or higher [from here](https://www.python.org/downloads/)
2. Rust [from here](https://www.rustup.rs)
3. Install fracsuite using pip: `pip install fracsuite --upgrade`
4. Run fracsuite once using: `python -m fracsuite help`
5. Now you can call fracsuite from every console

## Introduction

### Groups and Commands
fracsuite is structured into groups and commands. Groups are specific to tasks, such as splinter analysis, scalp data, acceleration analysis, and so on. The contained commands act on these tasks.

To reach a command, one must first specify the group, then the command. For example, to run the `list` command in the `splinters` group, one would write `fracsuite splinters list`.

To get additional help on any command, one can use the `--help` flag after the command. For example, `fracsuite splinters list --help`.
   
### Options and Arguments
Options are flags that can be used to modify the behavior of the command. For example, the `--debug` flag can be used to enable debug mode. Options can be specified before the command or after.
For example, `fracsuite --debug splinters list` and `fracsuite splinters list --debug` are equivalent.

Arguments are values that are passed to the command. For example, `fracsuite splinters list --path "C:/path/to/folder"` would pass the value "C:/path/to/folder" to the `list` command in the `splinters` group.
When specifying arguments, the argument name must be followed by the value. For example, `--path "C:/path/to/folder"` is correct, while `--path"C:/path/to/folder"` is not.
    

### Folder structure
During the first start, some configuration is setup. The two main folders, [DATABASE](#database) and [OUTPUT](#output), have to be defined.

#### DATABASE
The pseudo-database is a folder that contains all the data for the specimens. The database is specified in the configuration file, and can be changed by modifying the `database_path` value or by calling the `config set database_path "C:/path/to/database"` command. The database folder is created if it does not exist. The database folder contains a folder for each specimen, which in turn contains the data for that specimen.     

```
[specimen] = [Thickness].[Nominal Pre-Stress].[Support Condition].[Lf-Nr.]
```


```
[specimen]
└── scalp
└── fracture
  └── morphology
  └── splinters
  └── acceleration
└── anisotropy
```

Images are saved as `.bmp` files and must contain 'Transmission' to be recognized correctly. Acceleration data is saved as `.bin` files. The scalp folder contains a pickled stress data file.
    

#### OUTPUT

Every command inside of fracsuite gets its own output directory where intermediate output as well as the results are stored. If the result is specimen-specific, a copy of the output is also copied to the specimen folder.