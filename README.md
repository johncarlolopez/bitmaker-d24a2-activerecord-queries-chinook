![Bitmaker](https://github.com/johncarlolopez/bitmaker-reference/blob/master/bitmakerlogo.svg)
# 02 - Intro to ActiveRecord Queries
### Friday, Jan 12th

## README

[See instructions in Alexa.](https://alexa.bitmaker.co/wdi/67/assignments/2038/latest)

1. Find the albums recorded by the artist Queen.
```
Album.where("artist_id = (?)",Artist.where("name = ?",'Queen').ids)
```
2. Count how many tracks belong to the media type "Protected MPEG-4 video file".
```
Track.where("media_type_id = (?)",MediaType.where("name = ?",'Protected MPEG-4 video file').ids).count
```
3. Find the genre with the name "Hip Hop/Rap".
```
Genre.where("name = ?",'Hip Hop/Rap').first
```
4. Count how many tracks belong to the "Hip Hop/Rap" genre
```
Track.where("genre_id = (?)",Genre.where("name = ?",'Hip Hop/Rap').ids)
```
5. Find the total amount of time required to listen to all the tracks in the database.
```
Track.sum("milliseconds")
```
6. Find the highest price of any track that has the media type "MPEG audio file".
```
Track.where("media_type_id = (?)",MediaType.where("name = ?",'MPEG audio file').ids).maximum("unit_price").to_f
```
7. Find the name of the most expensive track that has the media type "MPEG audio file".
```
Track.where("unit_price = (?)",Track.where("media_type_id = (?)",MediaType.where("name = ?",'Protected MPEG-4 video file').ids).maximum("unit_price")).pluck(:name)
```
8. Find the 2 oldest artists.
```
Artist.order(created_at: :asc).limit(2)
```
9. Find the least expensive track that has the genre "Electronica/Dance".
```
Track.where("genre_id = (?)",Genre.where("name = ?",'Electronica/Dance').ids).where("unit_price = ?",Track.where("genre_id = (?)",Genre.where("name = ?",'Electronica/Dance').ids).minimum("unit_price"))

# OR

Track.where("genre_id = (?)",Genre.where("name = ?",'Electronica/Dance').ids).order(unit_price: :asc).limit(1)
```
10. Find all "MPEG audio file" tracks in the genre "Electronica/Dance".
```
Track.where("media_type_id = (?)",MediaType.where("name = ?",'MPEG audio file').ids).where("genre_id = (?)",Genre.where("name = ?",'Electronica/Dance').ids)
```
## Submitting
___
To submit, record the code you use to answer each question in a text file and upload it to Github.

## Stretch: More Active Record Query Interface
___
1. Find all the albums whose titles start with B.
```
Album.where("title like ?", 'B%')
```
2. Find the all the artists whose names start with A.
```
Artist.where("name like ?", 'A%')
```
## Stretch: Querying the Chinook Database
___
### using SQL
___
psql
From the command line,run:

psql chinook_development
To quit, type ctrl + d.

Check out W3Schools' SQL Reference as a reference, and don't forget to end each SQL statement with a semicolon (;).

Write one or more SQL queries to retrieve the requested data for each of the following:

1. Find the albums recorded by the artist Queen.
```
SELECT * FROM albums WHERE artist_id = (SELECT id FROM artists where name = 'Queen');
```
2. Count how many tracks belong to the media type "Protected MPEG-4 video file".
```
SELECT * FROM tracks where media_type_id = (SELECT id FROM media_types WHERE name = 'Protected MPEG-4 video file')
```
3. Find the least expensive track that has the genre "Electronica/Dance".
```
SELECT * FROM tracks WHERE genre_id = (SELECT id FROM genres where name = 'Electronica/Dance') AND unit_price = (SELECT min(unit_price) FROM tracks WHERE genre_id = (SELECT id FROM genres where name = 'Electronica/Dance'));
```
4. Find the all the artists whose names start with A.
```
SELECT * FROM artists where name like 'A%';
```
5. Find all the tracks that belong to playlist 1.
```
SELECT tracks.name, playlists_tracks.track_id, playlists_tracks.playlist_id FROM tracks, playlists, playlists_tracks WHERE tracks.id = playlists_tracks.track_id AND playlists.id = playlists_tracks.playlist_id AND playlists.id = 1 ;
```
