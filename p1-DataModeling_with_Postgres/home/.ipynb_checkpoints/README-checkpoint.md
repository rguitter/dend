# Project objectives

Sparkify is running its new music streaming app and so analyzing the activities is primordial to get insights about how the app is performing and how to improve it. In this context, song play is of primary interest, however the relatives data are spread in two different places making analysis complex. The purpose of the data is to make all the relevant data available in one place and organized them in an optimal manner for descriptive analytical queries.

# Design

## Database schema 

The analytical use case makes the DB extremely suitable for a star model. The analysis process is not a operational activity that operate over transactions which would include read but also write operations. The analysis process is an offline activity that meant to describe what happened by focusing on the song play in all its dimensions. 

So the first step of the DB conceptoin is to design the **song play** fact:
`songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent`
Then we model the relative dimensions: **users**, **songs**, **artist** and **time**

## ETL 

### Source of data

There's two sources of data:
* The song dataset from which we can extract data to feed **songs** and **users**
* The log dataset from which we can extract data to feed **users**, **time** and **song play**

This drives the definition of two pipelines:
* One data extract from song dataset followed by two transform and load
* One data extract from log dataset followed by three transform and load

# Examples

## Which artist is the most listened?

```
SELECT count(sp.artist_id) as count_play, a.name 
FROM songplays sp JOIN artists a ON sp.artist_id = a.artist_id 
GROUP BY sp.artist_id, a.name 
ORDER BY count_play DESC
```