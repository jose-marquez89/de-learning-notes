# Pushing data back to database
`csvsql` can push data back to the database

### Syntax
```bash
csvsql --db "sqlite:///SpotifyDatabase.db" \
       --insert Spotify_MusicAttributes.csv
```

Options
- `--no-inference`: disable type inference
- `--no-constraints`: no length limits or null checks for schema