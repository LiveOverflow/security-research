# Collaborative Security Research


## Arbitrary File Write
Imagine you find a vulnerability where you can *delete an arbitrary file* or you can redirect log output to a root owned file, what can you do with that?

* [delete a file](arbitrary-file-write/file-delete/README)
* [append to a file](arbitrary-file-write/file-append/README)
* [overwrite a file](arbitrary-file-write/file-overwrite/README)
* [create a file](arbitrary-file-write/file-create/README)

### Defined Attacker Abilities

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