---
title: What to expect out of MongoDB 2.4
layout: post
date: 2013-02-02
---

Yesterday, 10gen released version 2.2.3 and the first release candidate for 2.4
of its document database, MongoDB. If you want to get an early taste of what's
to come, you can check it out before 2.4 officially hits. Here's a rough update 
of the major changes.

### New JavaScript Engine

Google’s V8 will be replacing Mozilla’s SpiderMonkey as the underlying
JavaScript engine throughout the application. While there may be some benefits
in specific cases (deep recursion), there may not be any noticeable differences
for most MongoDB users. The idea for the switch was to help out with
parallelism, as SpiderMonkey was restricting MongoDB to a single thread of
execution for each mongos in a sharded environment. This may be barely
noticeable to most users, thanks to the combination of high-speed modern
processors and the short length of the operations sent from MongoDB. It will be
nice to see some benchmarking done to see where V8 becomes the clear victor
over SpiderMonkey.

### Three(read 'one') new type of indexes

You’ll now be able to use three new types of indexes to optimize your queries,
but you’ll probably only ever use one of the three. A hash-based index is new,
but its been mainly added to ease use of the new hash-based sharding(see
below). Hashed-based indexing has little to no benefit over using a regular
index on an ObjectId field, so unless you’re planning on using hash-based
sharding, there’s nothing to see here. A new geospatial index helps out with
spherical geometry. By a show of hands who needs better indexing for their geo
queries involving spherical geometry? For the three of you that raised your
hands, you’ll be using GeoJSON to define your geo fields, but only a small
subset is supported for this release. You also have the ability to use the new
query operator $geoIntersects, although the release notes don’t go into much
detail about this. When I was at MongoDB Chicago, there was talk about being
able to query a collection with a specific point where the returned documents
would have polygons that contained that point. If the $geoIntersects query
operator turns out to do this, it looks like a nice solution to find out which
restaurants will deliver to me, but only more detailed documentation will
clarify. The new index you’ll most likely be able use starting today is the
“text” index, and accompanying query. You can specify one or more fields to
index, and each of those fields can be assigned a weight MongoDB will use to
sort your search results. You’re not very limited to the types of fields you
can specify, either, so long as the field has a string, or is an array of
strings, or is a sub-document that has one of those two options, its fair to
build an index on it.  This will make it tremendously easy to do a full-text
search that includes your user’s ‘About Me’ field and all of their comments and
weigh the results based on what you think is more important. Here’s an example
of creating a text-based index:

{% highlight javascript %}
db.collection.ensureIndex(
  {
    aboutMe: "text",
    "comments.body": "text"
  },
  {
    name: "TextIndex",
      weights: {
        aboutMe: 5,
        "comments.body": 2
      }
  }
);
{% endhighlight %}

The example above creates a text index that includes two fields: aboutMe and
comments.body. Note the double-quotes when indexing on the field of a
sub-document. The second argument names the index, because the default isn’t
that great: “aboutMe_text_comments.body_text”. Imagine using 6 fields in your
text index. You’ll quickly be running up on the index name length restriction
of 128 characters. Even with only a few fields in your index, the default isn’t
very friendly, so get into the habit of naming these. I’ve also put weights on
the indexes, which will be reflected in the relevant score(position) of each
document in the result set. The text query structure is different than what you
may be used to, and you’re returned a document representing the results as
opposed to a cursor, so it may not integrate as nicely as it could with your
existing code. A query on the index I created above would look like:

{% highlight javascript %}
db.collection.runCommand( “text”, 
    {
        search: "\"prototypal inheritance\" -JavaScript"
        filter: { experiencePoints: { $gt: 1000 },
        limit: 15
    }
});
{% endhighlight %}

The above query will search your text index for documents that have the exact
phrase “prototypal inheritance” and that do not include the word “JavaScript”.
Notice the escaped double-quotes around the exact phrase string we want, and
the hyphen to represent a negation. The results will then be filtered by
documents that have an experiencePoints field that is greater than 1000, and
only 15 documents will be returned. Again, the result of a text query will not
return a cursor, but a special document. The mongo shell is a great place to
explore this new functionality. All things considered, the new text indexes and
text queries are simple and powerful additions that will enjoy widespread
adoption.

### Hash-based sharding

Particularly beneficial for highly active production environments, hashed-based
sharding provides a more reliable way to evenly distribute data across your
shards. This means you can spend less time worrying about rebalancing your data
at the end of the day. To aid the ease of use, the MongoDB team implemented a
new "hashed" index type, which makes it trivial to add this type of sharding on
a particular collection. The simple two-step process looks like:

{% highlight javascript %}
// Create your hashed index beforehand if your collection already has data
db.collection.ensureIndex( { someField: "hashed" } );

// Shard your collection, using the new key
sh.shardCollection( "db.collection", { someField: "hashed" } );
{% endhighlight %}

Now at some point there was a problem with people hashing their own keys using
MD5: the data was so evenly distributed that read queries were taking too long
to scan through the shards. I haven’t seen any benchmarks here, but there is a
specific commit on this issue that mentions patching a function whose job it is
to calculate the relevant shards for a given query. Hopefully that solves the
problem of the longer queries while not touching the nice, even distribution.

For the most part, there are some incremental performance changes and
improvements that help niche markets. Full-text search is going to be the big
deal here superficially: it’s easy to set up and a handy feature. Be that as it
may, hash-based sharding has the potential to take the cake for the most
valuable addition: it could help MongoDB continue to be adopted in the
enterprise world, where high-volume is a given and rebalancing is a costly
procedure to be avoided at all costs. If you haven’t grabbed the new release
yet, [download it here](http://www.mongodb.org/downloads) and start playing!
