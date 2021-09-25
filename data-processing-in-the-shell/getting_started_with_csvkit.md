# Getting started with csvkit

### Installation
- you can pip install csvkit
- upgrade with `--upgrade` flag

### in2csv
`in2csv` seems to be part of the csvkit package
```bash
in2csv SpotifyData.xlsx > SpotifyData.csv
```

Specifying sheet names
- use `-n` or `--names` to print sheet names to console
- then use `--sheet` to specify sheet name
```
in2csv SpotifyData.xlsx --sheet "Worksheet1_Popularity" > Spotify_Popularity.csv

# do a sanity check in the directory to check for new files
ls
```
### csvlook
Renders csv to the command line in a markdown compatible format
```bash
csvlook Spotify_Popularity.csv
```

### csvstat
Similar to pandas describe
```bash
csvstat Spotify_Popularity.csv
```


