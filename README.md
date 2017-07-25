# bash_commands

Enabling Push Status Messages on OS X

To obtain the current state of the APNs daemon on OS X, use this command:

$ /System/Library/PrivateFrameworks/ApplePushService.framework/apsctl status

To enable logging on OS X, use the following commands:

$ sudo touch /Library/Logs/apsd.log
$ sudo defaults write /Library/Preferences/com.apple.apsd APSWriteLogs -bool TRUE
$ sudo defaults write /Library/Preferences/com.apple.apsd APSLogLevel -int 7
$ sudo killall apsd
The logs are stored in /Library/Logs/apsd.log


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
