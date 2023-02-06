# Hugo Podcast Theme

A theme for generating multiple podcast feeds.

If you have two main categories, `A` and `B`, and deliver audio in `opus` and `mp3`, the following feeds will be generated:


- `/categories/A/mp3feed.xml`
- `/categories/B/mp3feed.xml`
- `/categories/A/opusfeed.xml`
- `/categories/B/opusfeed.xml`
- `/episode/index.xml` as a combined feed, containing all episodes.

Further, for each episode, metadata for the podlove player is created (currently only stubbed) as a json.

## How to get this to work

This theme is intended as a "mixin".
You would configure your themes in the hugo site's `config.toml` as follows:
```
theme = [ 'ananke', 'podcast']
```
Here `ananke` is just a placeholder for any other theme you wish to have.

You want to configure some parameters regarding the feeds in your `config.toml`:

```
[params]

[params.hosting]
media_directory = "https://cdn.your-podcast.example.com/"

[params.feed]
  copyright = "Copyright foo" #do not use markdown in this field; it is used in the feed
  language = "en-us"
  itunes_summary = "description a"
  itunes_author =  "Hans Wurst"
  itunes_owner_name = "Hans Wurst"
  itunes_owner_email = "foo@example.com"
  itunes_image = "https://example.com/sample.png" #fqdn to the image art for your podcast
  itunes_top_category = "Technology"
  itunes_first_sub_category = "Software"
```



A last thing is to configure the output formats you want to have:

```
[outputFormats]
  [outputFormats.episodeData]
    baseName = "episode"
    isPlainText = true
    mediaType = "application/json"
  [outputFormats.opusfeed]
    MediaType             = "application/rss+xml"
    BaseName              = "opusfeed"
    IsHTML                = false
    IsPlainText           = true
    noUgly                = true
    Rel                   = "alternate"
  [outputFormats.mp3feed]
    MediaType             = "application/rss+xml"
    BaseName              = "mp3feed"
    IsHTML                = false
    IsPlainText           = true
    noUgly                = true
    Rel                   = "alternate"

[outputs]
  home = ['HTML', 'RSS']
  page = ['HTML', 'episodeData']
  term = [ "opusfeed", "mp3feed"]
```

The `term = [ "opusfeed", "mp3feed"]` line allows you to select which feeds to publish for your terms in the taxonomy (aka which audio formats for each category).

## Content

The theme expects a frontmatter simmilar to the following.
The theme expects them to live under `/content/episodes`. 
This allows you to still publish elements in `post` as "updates" and announcements.

```
---
date: 2023-02-01T16:10:18+01:00
description: "Foo Bar"
featured_image: "/images/episodes/thumbnails/001-thumb.jpg"
tags: ["A", "B", "C"]
title: "001 Foo"

# audiofile name without file ending
audiofile: "foo"
podcast_duration: "10:01:10"
episode_image: "foo.png"

categories: ["X", "Y"]

aliases:
    - /e/001
    - /whatever/foo/bar/bazz-boo.html

---
```