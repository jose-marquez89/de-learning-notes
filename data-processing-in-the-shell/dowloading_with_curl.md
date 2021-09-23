# Downloading data with Curl

### What is curl
- short for 'client for Urls'
- unix command line tool
- can be used to download from HTTP and FTP servers
- check for installation with `man curl`

### Basic syntax
Saving with the original name:
```bash
curl -O https://somewebsite.com/sometextfile.txt
```
Saving with new name
```
curl -o renamed_file.txt https://somewebsite.com/sometextfile.txt
```
Getting all files at the endpoint
```bash
curl -O https://somesite.com/datafilename*.txt
```
Using a globbing parser
```
curl -O https://somesite.com/datafilename[001-100].txt

# grab every n-th element
curl -O http://somesite.com/datafilename[001-100:10].txt
```
Useful flags
- `-L` redirects HTTP url when there's a 300 error
- `-C` resumes a previous file transfer if it times out before completion

Option flags come before the URL
```bash
curl -L -O -C https://somesite.com/datafilename[001-100].txt
```