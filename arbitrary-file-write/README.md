# Arbitrary File Write Research

## Motivation
[Watch Introduction](https://clips.twitch.tv/KindDifficultSushiBibleThump-1YdF0sW6bBO64ZCW)

Imagine you find a vulnerability where you can *delete an arbitrary file* or you can redirect log output to a root owned file, what can you do with that?

* [delete a file](file-delete/README.md)
* [append to a file](file-append/README.md)
* [overwrite a file](file-overwrite/README.md)
* [create a file](file-create/README.md)

## Research Goal

The goal is to create a resource for professionals where they can lookup most reliable and useful technique when they find a file related bug. Allowing them to turn an arbitrary file write bug into a full shell, escalate privileges, get remote code execution, ... etc.

If we find multiple techniques for a certain method, we want to prioritise techniques that work on the largest range of systems.

## Research Notes

#### Useful Tools
* `pspy` https://github.com/DominicBreuker/pspy
	* *"Monitor linux processes without root permissions"*
* `fswatch` https://github.com/emcrisostomo/fswatch
	* *"a cross-platform file change monitor"*
* `opensnoop`
* `processmonitor` https://docs.microsoft.com/en-us/sysinternals/downloads/procmon 
	* *"an advanced monitoring tool for Windows that shows real-time file system, Registry and process/thread activity"*
* `pssuspend` https://docs.microsoft.com/en-us/sysinternals/downloads/pssuspend
	* *"Rather than kill the process that's consuming the resource, suspending permits you to let it continue operation at some later point in time."*

#### Useful Snippets
* Write an empty file to every folder
	* `find / -type d -exec touch {}/liveoverflow \;`

## Defined Attacker Abilities

The following table provides an overview of the different scenarios we consider. Each scenario might have different techniques based on the target operating system.

| Attacker Ability | Privilege | Filename/Extension | Content | Example                                                                 |
|------------------|-----------|--------------------|---------|-------------------------------------------------------------------------|
| delete a file    | root      | -                  | -       | A VPN priviledged daemon can be tricked into deleting an arbitrary file |
|                  | user      | -                  | -       | A webserver running with limited privs can delete an arbitrary file     |
| append to a file | root      | full               | full    | Redirect log output of a root process to another file                   |
|                  |           |                    | none    |                                                                         |
|                  |           |                    | partial |                                                                         |
|                  | user      | full               | full    |                                                                         |
|                  |           |                    | none    |                                                                         |
|                  |           |                    | partial |                                                                         |
| overwrite file   | root      | full               | full    |                                                                         |
|                  |           |                    | none    |                                                                         |
|                  |           |                    | partial |                                                                         |
|                  | user      | full               | full    |                                                                         |
|                  |           |                    | none    |                                                                         |
|                  |           |                    | partial |                                                                         |
| create file      | root      | full               | none    | You can drop a file with the name `----checkpoint-action=exec=/bin/sh`  |
|                  |           | none               | none    | You can drop a file but cannot control it, for example `temp1234`       |
|                  |           | full               | full    |                                                                         |
|                  |           | none               | full    |                                                                         |
|                  |           | full               | partial |                                                                         |