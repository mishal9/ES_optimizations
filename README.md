# ES_optimizations

## refresh_interval:

### Why refresh interval is important and what shall we do about it?

Elasticsearch is near-realtime, in the sense that when you index a document, you need to wait for the next refresh for that document to appear in search. 
If you are planning to index a lot of documents and you don’t need the new information to be immediately available for search, you can optimize for indexing performance over search performance by decreasing refresh frequency until you are done indexing.
When documents are added to an ES index, they are divided into Shards. Shards of an index are in turn are composed of multiple segments.  

![alt text](https://fdv.github.io/running-elasticsearch-fun-profit/003-about-lucene/images/image2.svg "Lucene data structure")


These segments are created with every refresh and subsequently merged together over time in the background.
The lucene working behind the scenes is responsible for segment merging, but if not handled carefully it can be computationally very expensive and may cause Elasticsearch to automatically throttle indexing requests to a single thread.(ouch!)

If you are planning to index a lot of documents and you don’t need the new information to be immediately available for search, you can optimize for indexing performance over search performance by decreasing refresh frequency until you are done indexing.

### When does my data becomes searchable?

![alt text](https://yqintl.alicdn.com/6c7048b0f4747b67bddbf75f8b10bf97ded3a322.png "Data representation in Lucene")


