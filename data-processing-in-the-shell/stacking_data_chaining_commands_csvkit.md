# Stacking data and chaining commands with csvkit

### Stacking
This seems like it's essentially appending tables

```bash
csvstack Spotify_Rank6.csv Spotify_Rank7.csv > Spotify_AllRanks.csv

# if you want to add an identification column
csvstack -g "Rank6","Rank7" \
Spotify_Rank6.csv Spotify_Rank7.csv > Spotify_AllRanks.csv

# rename the identification column
csvstack -g "Rank6","Rank7" -n "source" \
Spotify_Rank6.csv Spotify_Rank7.csv > Spotify_AllRanks.csv
```

**Chaining command line commands**
- `;` links commands together
- `&&` links commands together but only runs 2nd command if first succeeds 
- `>` re-directs the output from 1st command to location in 2nd  
- `|` "pipes" the output of the 1st command into the 2nd command

```bash
# using the semicolon
csvlook SpotifyData_All.csv; csvstat SpotifyData_All.csv

# passing commands for better data formatting
csvcut -c "track_id","danceability", Spotify_Popularity.csv | csvlook
```