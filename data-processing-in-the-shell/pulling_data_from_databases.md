# Pulling Data From Databases

### sql2csv
You can use `sql2csv` to pull data from sql sources

```bash
# --db for connection string
# do not use line breaks for the sql query here
sql2csv --db "sqlite:///SpotifyDatabase.db" \
        --query "SELECT * FROM Spotify_Popularity"
        > Spotify_Popularity.csv
```
_Note: if you don't redirect the output to a csv file, it will write the output to the console_

