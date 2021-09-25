# Downloading with wget

## What is it
- derives name from 'www' and 'get'
- native to linux
- used for http(s) and ftp
- better than curl at recursive downloading
- also supports sftp

### Checking for installation
- `which wget`
- manual is `man wget`

## Downloading single file
```bash
# flags
# -b: go to background after startup
# -q: quiet output
# -c: resume broken download

# you can string together options flags like so
wget -bqc https://www.somewebsite.com/datafilename.txt
```

## Advanced downloading with wget
You can store url location in a text file and pass the file to wget
```bash
# if you need other options flags in this case,
# put them _before_ -i
wget -i url_list.txt
```
You can set an upper bandwidth limit so that wget doesn't overwhelm your bandwidth
```bash
# syntax: --limit-rate={rate}k {file_location}
wget --limit-rate=200k -i url_list.txt 
```

Sometimes with smaller files, limiting bandwidth won't be as effective.
In these cases, to avoid over-taxing the host server, you can set a wait time:
```bash
# wait time is in seconds
wget --wait=2.5 -i url_list
```

### Comparing curl and wget
**curl**
- supports many protocols
- easier to install across operating systemts

**wget**
- lots of functionality for handling multiple file downloads
- handles various file formats very well


