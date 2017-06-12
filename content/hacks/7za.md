% 7Zip Archiver Commands
% Ravikiran K.S.
% January 1, 2006

7Zip archiver for Linux supports following options:

  - `-p` - secure the archive with given password.

  - `-mhe=on` - additionally encrypt headers

  - `-mhc=on` - additionally compress headers.

  - `-t` - type of archive to create. Known types: 7z, gzip, zip, bzip2,
    tar, iso, udf.

  - `-mx` - optimization level. 0 (min), 1, 3, 5, 7, 9 (max).

  - `-md` - specify dictionary size. Boosts compression speed.

  - `-mmt` - enable multi-threading (for multi-cpu/core computers)

  - `-mmem=24m` - control amount of memory used. units are b, k, m.

  - `-ms=on` - create a solid archive. Provides best compression, but
    you can't update archive using -u.

  - Note: Solid archive is best for golden/pristine copies that are
    never meant to be changed.

  - `-ssc` - enable case-sensitive mode (linux). -ssc- for
    case-insensitive mode (windows).

  - `-aoa` - overwrite all destination files.

  - `-aos` - skip over existing files without overwriting.

  - `-aou` - avoid name collisions.

  - `-aot` - rename the existing
files.

### Add set of files or complete directory recursively (no -r option) to archive:

`7za a -t<type> -mx9 <archive>.<type-ext> -p<secret-pass>
[<directory-name>|<file-list>]`

### Add set of files with given pattern recursively into archive:

`7z a archive.zip *.vim -r`

### List files in archive:

`7za l new.7z`

### Delete set of files with given pattern recursively from archive:

`7za d archive.zip *.bak -r`

### Extract all archive contents preserving the directory structure.

`7za x archive.zip`

### Test the integrity of given pattern files in archive:

`7za t archive.zip *.doc -r`

### Update particular file in archive with new one.

`7za u archive.zip *.doc`

Note: Avoids decompress and compress of entire archive. Doesn't work
with solid archives.

