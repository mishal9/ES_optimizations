# ES_optimizations

## refresh_interval:

### Why refresh interval is important and what shall we do about it?

Elasticsearch is near-realtime, in the sense that when you index a document, you need to wait for the next refresh for that document to appear in search. 
If you are planning to index a lot of documents and you don’t need the new information to be immediately available for search, you can optimize for indexing performance over search performance by decreasing refresh frequency until you are done indexing.
When documents are added to an ES index, they are divided into Shards. Shards of an index are in turn are composed of multiple segments.  

![alt text](https://fdv.github.io/running-elasticsearch-fun-profit/003-about-lucene/images/image2.svg "Logo Title Text 1")


These segments are created with every refresh and subsequently merged together over time in the background.
The lucene working behind the scenes is responsible for segment merging, but if not handled carefully it can be computationally very expensive and may cause Elasticsearch to automatically throttle indexing requests to a single thread.(ouch!)
We can avoid this problem by taking care of the `index.refresh_interval` parameter. This parameter can be set on individual index level. Because refreshing is expensive, one way to improve indexing throughput is by increasing refresh_interval. Less refreshing means less load, and more resources can go to the indexing threads.  
If we don't the documents in a particular index to be available in search as soon as it is ingested, we can increase the `index.refresh_interval` to be more than 1 and vice versa. 



