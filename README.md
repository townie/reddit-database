# reddit-database

This respository contains MongoDB dumps of the reddit structure that were data minned with this [crawler](https://github.com/federicocalendino/reddit-sub-crawler).


Each document in **subreddits** contains the following information:
  * The **_id** is the name of the subreddit (in lowercase).
  * The date and time when the subreddit was analyzed is stored in a **timestamp**.
  * The amount of **subscribers** the subreddit had at the moment.
  * the **type** can be one of these: 
    * *public*: most common type, it has the over18 flag off.
    * *nsfw*: most used for research, has the over18 flag on.
    * *private*: you need to be invited in by a mod to read it.
    * *banned*: got hammered down.
    * *nonexistent*: it was deleted or never created.
  * The **description** has the contents of *wiki/config/description* for public/nsfw subreddits and the message of the private subreddits landing page.
  * Finally, a list of **keywords** are extracted from the description when possible (using [rake-nltk](https://github.com/csurfer/rake-nltk)).
  
To build the graph, what the crawler did was analize the sidebar and wiki of each subreddit looking to match the regex /r/.* . Each string found this was added to a queue and a relation was inserted in the **relations** collection.
  
On each document in the **relations** collection you'll find the following fields:
  * **sub_a** is the first subreddit (lexicographically).
  * **sub_b** is the second subreddit (lexicographically).
  * The **_id** is a hash of the concatenation sub_a/sub_b.
  * The date and time when this relations was found is stored in a **timestamp**.

### Database: v1
Date: November 2, 2017

Subreddits: 69600
Relations: 301565


### FAQ:

Q: Why would you use MongoDB for storing a Graph?
A: Because the crawler is running 24/7 on a heroku worker and the MongoDB add-on has a lot of space (compared to Heroku Postgres). Also, it's free just perfect for my budget.

Q: Will you push new dumps of this graph in the future?
A: I'll probably will, as long i keep tinkering with the crawler to add new information.
