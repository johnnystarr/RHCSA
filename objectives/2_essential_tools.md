# objectives: 2_essential_tools

##Understand and use essential tools

###Archive, compress, unpack and uncompress files:

|  Tool      |   Archive                    |  Extract                  |
|:----------:|:----------------------------:|:-------------------------:|
| tar (gzip) | `tar cfzv archive.tar.gz`    | `tar xvf archive.tar.gz .`|
| tar (bzip) | `tar cfjv archive.tar.bz`    | `tar xvf archive.tar.bz .`|
| star       | `star -c -f=archive.tar dir/`| `star -x -f=archive.tar .`|
| gzip       | `gzip file`                  | `gunzip file.gz`          |
| bzip2      | `bzip2 file`                 | `bunzip file.bz2`         |

###List, set and change standard ugo/rwx permissions
####Create a test directory with 3 files
`mkdir test && touch test/{file1,file2,file3}`
####Listing
`ls -l test/`
####Changing Ownership
- Recursively change owner & group
- `chown -R nobody:wheel test/`

####Changing Modes
- Change mode to 0755 octal way
- `chmod 0755 test/file1`
- Change mode to 0755 ugo way
- `chmod u=rwx,g=rx,o=rx test/file{2,3}`
- List changes to verify
- `ls -l test/`

####Special Perms
- `chmod 1750 test/file1` (Sticky Bit)
- `chmod 2750 test/file2` (Set GID)
- `chmod 4750 test/file3` (Set UID)

