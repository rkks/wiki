% Unix Nits
% Ravikiran K.S.
% January 1, 2006

## Symbolic Links

The 3 different types of links on unix systems are used to connect the 3
components of a file.

* names (stored in directory entries) -- symlinks
$ cp --symbolic-link <src-file> <dst-file>

symlink reference is usually stored in a specially tagged inode, and hence
timestamps for a symlink can be set on some systems. Permissions are ignored in
symlink inodes. main use for symlinks is to create aliases.

* meta-data like permissions (stored in inodes) -- hardlinks
$ cp --link <src-file> <dst-file>

hardlink are used to provide multiple names to an inode. Since hardlinks
reference inodes directly, they're restricted to the same file system. main use
for multiply hardlinked files is to create efficient backups.

* the data blocks themselves -- reflinks

supported by BTRFS and OCFS2 and support transparent copy on write which is
especially useful for snapshotting. separate inodes are used, and hence one can
have different permissions to access the same data.

While creating a file generally involves all 3, one can just create files using
either of 3.

