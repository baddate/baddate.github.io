---
title: NOHUP command details
date: 2019-07-10
lastmod: 2020-01-29 
weight: 2
url:
    /linux/nohup
tags:
    - Tips  
    - Linux
    - CMD
    - Redirecting
categories:
    - 2019-07

---

## Nohup Command
Nohup, short for _NO HANG UP_ is a command in Linux systems that keep processes running even after exiting the shell or terminal.

Nohup prevents the processes or jobs from receiving the SIGHUP (Signal Hang UP) signal. This is a signal that is sent to a process upon closing or exiting the terminal.

## Nohup Command Syntax
Nohup command syntax is as follows:
```bash
nohup command arguments
```
OR
```bash
nohup options
```

## Usage

### Starting a process using Nohup

To redirect to a file and to standard error and output use the `> filename 2>&1` attribute:
```bash
nohup command > file 2>&1
```

### Starting a process in the background using Nohup
To start a process in the background use the & symbol at the end of the command:
```bash
nohup ping google.com &
```
To check the process when resuming the shell use the `pgrep` command:
```bash
pgrep -a ping
``` 


## Redirection in linux

|Handle	|Name	|Description|
|-------|-------|-----------|
|0	|stdin	|Standard input|
|1	|stdout	|Standard output|
|2	|stderr	|Standard error|

Examples:
```bash
# executes command, directing the standard input stream to file
command > file
command 1> file
# executes command, directing the standard error stream to file
command 2> file
# merge standard error into standard output so error messages can be processed together with (or alternately to) the usual output.
command > file 2>&1
```

By the way, `command 2>&1 > file`&`command > file 2>&1` is [different](https://en.wikipedia.org/wiki/Redirection_(computing)#Redirecting_to_and_from_the_standard_file_handles).

## Reference
https://www.journaldev.com/27875/nohup-command-in-linux

https://en.wikipedia.org/wiki/Nohup

(https://en.wikipedia.org/wiki/Redirection_(computing))