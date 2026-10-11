#  Manage Compressed tar Archives
###  Archive
Archive = Multiple files combined into one file.
==Combining multiple files and folders into a single file.==

Compressed Archive = Archive made smaller.
```
file1.txt  ┐
file2.txt  ├─→ archive.tar ─→ archive.tar.gz (smaller)
file3.txt  ┘
```

### tar
tar = Tape Archive

Originally for backup tapes, now used everywhere.

**tar does two things:**
- Combine files into one archive
- Compress the archive (optional)

#### Common Archive Extensions

| Extension           | Meaning                       |
| :------------------ | :---------------------------- |
| `.tar`              | Archive only (not compressed) |
| `.tar.gz` or `.tgz` | Compressed with gzip          |
| `.tar.bz2`          | Compressed with bzip2         |
| `.tar.xz`           | Compressed with xz            |
| `.zip`              | Zip format                    |

#### Compression Comparison

| Type      | Speed  | Compression | Extension |
| :-------- | :----- | :---------- | :-------- |
| **gzip**  | Fast   | Good        | `.gz`     |
| **bzip2** | Medium | Better      | `.bz2`    |
| **xz**    | Slow   | Best        | `.xz`     |

For quick transfers: Use gzip
For smaller size: Use xz

### Create Archives
Create Simple Archive (no compression)
```
tar -cvf archive.tar file1 file2 file3
```

**Breakdown:**
- -c = Create
- -v = Show files being added
- -f archive.tar = Output file name

#### Create Archive from Directory
```
tar -cvf backup.tar /home/john/documents/
```
Archives entire directory.

### create Compressed Archive (gzip)
```
tar -czvf archive.tar.gz file1 file2 file3
```
The -z adds gzip compression.


### View Archive Contents
Without extracting:
```
tar -tvf archive.tar
tar -tvf archive.tar.gz
tar -tvf archive.tar.bz2
```

### Extract Archives
Extract to Current Directory
```
tar -xvf archive.tar
tar -xzvf archive.tar.gz
tar -xjvf archive.tar.bz2
tar -xJvf archive.tar.xz
```

### Other Compression Commands
gzip / gunzip

Compress single file:
```
gzip file.txt
```

#### Decompress:
```
gunzip file.txt.gz
```

### zip / unzip

**Create zip:**
```
zip archive.zip file1 file2 file3
```

**Extract zip:**
```
unzip archive.zip
```


```
bzip2 file.txt
```

```
bunzip2 file.txt.bz2
```