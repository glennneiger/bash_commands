# bash_commands


// all write file descriptors in use
```
sudo fs_usage |grep write |grep -Fv -e grep -e Chrom -e syslogd
```

// filter out stuff in an original file by the stuff in another file
```
awk 'FNR==NR{old[$0];next};!($0 in old)' libsystem.txt engage.txt > libsystem_removed.txt
```

// create a filter file
```
grep -e "libsystem_network.dylib" engage.txt > network.txt
```

// List all files by size greater than 1GB
```
find / -type f -size +1GB -exec ls -lh {} \; 2> /dev/null
```

// List all TCP connections at ipv4 at port 9001
```
lsof -n -i4TCP:9001
```

// tree a directory
```
#!/bin/bash

MAGIC='s;[^/]*/;|____;g;s;____|; |;g'

if [ "$#" -gt 0 ] ; then
   dirlist="$@"
else
   dirlist="."
fi

for x in $dirlist; do
     find "$x" -print | sed -e "$MAGIC"
done
```
