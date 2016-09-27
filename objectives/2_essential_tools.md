# objectives: 2_essential_tools

###Archive, compress, unpack and uncompress files:

|  Tool      |   Archive                    |  Extract                  |
|:----------:|:----------------------------:|:-------------------------:|
| tar (gzip) | `tar cfzv archive.tar.gz`    | `tar xvf archive.tar.gz .`|
| tar (bzip) | `tar cfjv archive.tar.bz`    | `tar xvf archive.tar.bz .`|
| star       | `star -c -f=archive.tar dir/`| `star -x -f=archive.tar .`|
| gzip       | `gzip file`                  | `gunzip file.gz`          |
| bzip2      | `bzip2 file`                 | `bunzip file.bz2`         |

####Create a test directory with 3 files, list, change ownership
- `mkdir test && touch test/{file1,file2,file3}` - create some directories
- `ls -l test/`                                  - list in long mode
- `chown -R nobody:wheel test/`                  - recursively change owner & group

####Changing Modes (octal & ugo)
- `chmod 0755 test/file1`                - change mode to 0755 octal way
- `chmod u=rwx,g=rx,o=rx test/file{2,3}` - change mode to 0755 ugo way
- `ls -l test/`                          - list changes to verify

####Set Special Permissions (files / directories)
- `chmod 1750 test/file1` - Sticky Bit
- `chmod 2750 test/file2` - Set GID
- `chmod 4750 test/file3` - Set UID

