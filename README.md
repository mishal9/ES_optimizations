# ES_optimizations

### Why refresh interval is important and what shall we do about it?

Elasticsearch is near-realtime, in the sense that when you index a document, you need to wait for the next refresh for that document to appear in search. 
If you are planning to index a lot of documents and you don’t need the new information to be immediately available for search, you can optimize for indexing performance over search performance by decreasing refresh frequency until you are done indexing.
When documents are added to an ES index, they are divided into Shards. Shards of an index are in turn are composed of multiple segments.  

![alt text](https://fdv.github.io/running-elasticsearch-fun-profit/003-about-lucene/images/image2.svg "Lucene data structure")


These segments are created with every refresh and subsequently merged together over time in the background.
The lucene working behind the scenes is responsible for segment merging, but if not handled carefully it can be computationally very expensive and may cause Elasticsearch to automatically throttle indexing requests to a single thread.(ouch!)

If you are planning to index a lot of documents and you don’t need the new information to be immediately available for search, you can optimize for indexing performance over search performance by decreasing refresh frequency until you are done indexing.

### When does my data becomes searchable?

![Lucene data representation](images/lucene_data_representation.png)

The data is first ingested into an in-memory buffer which when overflows flushes the data and creates a sizeable segment. 
When a segment size grows up, different segments are clustered together thereby actually deleting the documents marked as `deleted` earlier. It is when inside segment that the data becomes searchable.

During a merge, Lucene takes 2 segments, and moves the content into a third, new one. Then the old segments are deleted from the disk. It means Lucene needs enough free space on the disk to create a segment the size of both segments it needs to merge.

A problem can arise when force merging a huge shard. If the shard size is > half of the disk size, you provably won’t be able to fully merge it, unless most of the data is made of deleted documents.


