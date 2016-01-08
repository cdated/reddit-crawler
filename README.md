reddit-crawler
=================

### Description:

In short, this project can obtain subreddit data, report progress, and display the data graphically.

#### Crawling

`subreddit_crawler.py` - Builds a database of related subreddits

`show_progress.py`- Periodically checks the database and reports crawler progress

#### Analysis

`generate_graph.py` - Creates a graph of all subreddits that are connected through recommendations section

An interactive subset of the data can be found here: https://github.com/cdated/subredditor

### Usage:

To use `generate_graph.py` one must either run `subreddit_crawler.py` to populate the MongoDB database, or use mongorestore on the bson in data/dump/reddit.  Once a dataset has been loaded/generated two types of graphs can be constructed; a full network with filters or a region with child node limits.

#### Loading Database

There's already a database (approx 8Mb) in the repo for those who don't want to run the crawler to see the connections.  To load it just run the `restore_db.sh` script.

#### Crawling

`subreddit_crawler.py` starts at a user defined subreddit and collects all the recommendations.  It uses the parsed recommendations to get the more until is recurses through all possible subreddits linked.  Previously explored subreddits are not revisited.  A backlog of subreddits to be visited, and subreddit relationships are stored in MongoDB and loaded on application start if available.

```
usage: subreddit_crawler.py [-h] -s SUBREDDIT

optional arguments:
  -h, --help            show this help message and exit
  -s SUBREDDIT, --subreddit SUBREDDIT
                        Subreddit seed
```

`show_progress.py` checks the backlog every 2 seconds and prints the current subreddit being crawled, the number visited, and the number currently in the backlog.

```
./show_progress.py
Checking truepoetry
Checked: 14
Remaining: 159

Checking badarthistory
Checked: 15
Remaining: 158
```

#### Analysis

`generate_graph.py` generates a full graph of recommended subreddits.  By default it hides nodes featuring explicit content, but can generate a censored graph (default), full graph, and the difference of the two.  One may also filter out subreddits with subscriber counts below a specified number with the minimum flag.  Output is a graphviz file.

```
usage: generate_graph.py [-h] [-c] -m MINIMUM [-n] [-v]

optional arguments:
  -h, --help            show this help message and exit
  -c, --censored        Hide over 18 subreddits
  -m MINIMUM, --minimum MINIMUM
                        Min subcribers to be added
  -n, --nsfw            Only over 18 subreddits
  -v, --verbose         Show debugging
```
