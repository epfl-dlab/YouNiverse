![Repository logo](images/logo.png)


_[Large-Scale Channel and Video Metadata from English-Speaking YouTube]_

- For  context and an explanation of how data collection was done see [the paper](todo).
- Please cite this dataset if you use it:
```
@dataset{manoel_horta_ribeiro_2020_4327607,
  author       = {Manoel Horta Ribeiro and
                  Robert West},
  title        = {{YouNiverse: Large-Scale Channel and Video Metadata 
                   from English-Speaking YouTube}},
  month        = dec,
  year         = 2020,
  publisher    = {Zenodo},
  version      = {1.0},
  doi          = {10.5281/zenodo.4327607},
  url          = {https://doi.org/10.5281/zenodo.4327607}
}
```
## Scripts

We provide two jupyter notebooks along with the dataset:

- `preprocessing.ipynb`: which replicates
- `analyses.ipynb`: which reproduces the plots in the associated paper.

## Channel Data

The file `df_channels_en.tsv.gz` contains data related to channels. 
It aggregates both basic stats from channels obtained from `channelcrawler.com`, 
as well as rankings obtained from `socialblade.com`.

- `category_cc`: category of the channel.
- `join_date`: join date of the channel.
- `channel`: channel id.
- `name_cc`: name of the channel.
- `subscribers_cc`: number of subscribers.
- `videos_cc`: number of videos.
- `subscriber_rank_sb`: rank in terms of number of subscribers.
- `weights`: weights cal

| category_cc | join_date  | channel                  | name_cc      | subscribers_cc | videos_cc | subscriber_rank_sb |  weights |
|:------------|:-----------|:-------------------------|:-------------|---------------:|----------:|-------------------:|---------:|
| Gaming      | 2015-08-26 | UCi_AR7WqvXa6LEnRn_7ES7A | Thunder Play |          11500 |       849 |             877395 |  11.175  |
| Sports      | 2016-02-04 | UCgVlxaBsBkmMh2SUgrzG1ZQ | Thunder Prod |          76000 |        61 |             198340 |  5.5295  |
| Music       | 2013-09-14 | UCNBYpqbD64tkuuFS-NNhkfQ | Thunder Rain |          33700 |        58 |             382980 |  6.5855  |
​
Some facts about it:

- This dataframe has 136,470 rows, where each one corresponds to a different channel.
- We obtained all channels with >10k subscribers and 1>0 videos from `channelcrawler.com` in the 27 October 2019.
- Additionally we filtered all channels that were not in english given their video metadata (see `Raw Channels').

## Time Series Data

The file `df_timeseries_en.csv.gz` contains data related to time series. 
We have a data point for each channel and each week:

- `channel`: channel id.
- `category`: category of the channel as assigned by `socialblade.com` according to the last 10 videos at time of crawl.
- `datetime`: Week related to the data point.
- `views`: Total number of views the channel had this week.
- `delta_views`: Delta views obtained this week.
- `subs`: Total number of subscribers the channel had this week.
- `delta_subs`: Delta subscribers obtained this week.
- `videos`: Total number of videos the channel had this week.
- `delta_videos`: Delta videos obtained this week.
- `activity`: Number of videos published in the last 15 days.

| channel                  | category           | datetime   | views   | delta_views | subs | delta_subs | videos | delta_videos | activity |
|:-------------------------|:-------------------|:-----------|--------:|------------:|-----:|-----------:|-------:|-------------:|---------:|
| UCBJuEqXfXTdcPSbGO9qqn1g | Film and Animation | 2017-07-03 | 202495  |           0 |  650 |   0        |      5 |            0 |        3 |
| UCBJuEqXfXTdcPSbGO9qqn1g | Film and Animation | 2017-07-10 | 394086  |      191591 | 1046 | 396        |      6 |            1 |        1 |
| UCBJuEqXfXTdcPSbGO9qqn1g | Film and Animation | 2017-07-17 | 835394  |      441308 | 1501 | 456        |      6 |            0 |        1 |
| UCBJuEqXfXTdcPSbGO9qqn1g | Film and Animation | 2017-07-17 | 835394  |      441308 | 1501 | 456        |      6 |            0 |        1 |

Some facts about it:

- This file contains 18,872,499 data points belonging to 153,550 channels. 
- In average, it contains 2.8 years of data for each channel
- Data goes from early January 2015 to the end of September 2019. Not all channels have the complete time frame.
- Additionally we filtered all channels that were not in english given their video metadata (see `Raw Channels').

## Video Metadata

The file `df_videos_raw.jsonl.gz` contains metadata data related to ~73M videos from ~137k channels.
Below we show the data recorded for each of the video

    {
        'categories': 'People & Blogs', 
        'channel_id': 'UCzzYnZ8GIzfB1Vr3hk2Nj9Q', 
        'crawl_date': '2019-11-02 09:01:05.328421', 
        'description': 'See more at http://www.standstrongcompany.com Fitness Keep it healthy at (...)', 
        'dislike_count': 8, 
        'display_id': 'x72dBgcVPFI', 
        'duration': 187, 
        'like_count': 91,
        'tags': 'Tiger Fitness,TigerFitness,fitness,workout,diet,health,pre workout,ab workout,(...)', 
        'title': 'Slingshot for Squats? | Tiger Fitness', 
        'upload_date': '2019-04-21 00:00:00', 
        'view_count': 2559
    }
    
Some facts about it:
- This data was crawled from YouTube between 2019-10-29 and 2019-11-23.
- It contains 72,924,794 videos created between 2005-05-24 to 2019-11-20.

## Raw files (pre-language filtering)

Additionally, we provide raw files. 
These have the same names as the remaining files but:
a) have the prefix `_raw_` attached to them and;
b) do not have the suffix `_en` before the name extension.

## Helper file

The large `.json` file associated with video metadata can be quite painful to deal with. 
With that in mind, we also provide a helper (`yt_metadata_helper.feather`). 
This DataFrame contains the same fields as `df_videos_raw.jsonl.gz`, 
except `description`, `tags`, and `title` (the largest fields).
Feather is a language-agnostic portable file that can be easily loaded in Python or R (see [here][feather]).

[feather]: https://arrow.apache.org/docs/python/feather.html#:~:text=Feather%20is%20a%20portable%20file,Python%20(pandas)%20and%20R.