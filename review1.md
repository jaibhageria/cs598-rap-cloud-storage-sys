### B-trees, logging, and LSMs : Compare these different ways of organizing data (in terms of performance) for updates, reads, and range queries. Arrange the methods for each workload (from best performing to worst performing). Keep your answers short.
---
Reading: [Wisckey](https://www.usenix.org/system/files/conference/fast16/fast16-papers-lu.pdf), [LSMs](https://www.cs.umb.edu/~poneil/lsmtree.pdf) [optional]
||Updates|Reads|Short Range Queries|Long Range Queries|
|:-:|:-:|:-:|:-:|:-:|
|**Best**|LSMs|B-trees|B-trees|LSMs|
|**Better**|Logging|LSMs|LSMs|B-trees|
|**Good**|B-trees|Logging|Logging|Logging|

The reasoning for the above answer is below

#### Updates
- Logging: A single line is appended to end of log file. Writes are done sequentially to the disk. It is a O(1) operation.
- LSMs: Write optimized. Writes to in-memory buffer and are flushed in batches, sequentially to disk. It is a O(log n) operation. Since writes are in memory and flushing is deferred, it is slightly faster than B-trees. 
- B-trees: Involves random disk writes, may involve several pages/blocks and entire page overwrite.

#### Reads
- B-trees: Read optimized. The index is a self balanced tree which allows access to value in O(log n) time.
- LSMs: Worst case have to iterate through every SSTable starting from topmost level on disk to find value. Can take O(n log n) time
- Logging: Have to iterate through all log lines from bottom. Will take O(n) time.

#### Range Queries
Considering 2 cases below:
##### Short range queries
- B-Trees: It reduces to a read operation. The data will mostly be within a page which allows access in O(log n) time.
- LSMs: In the worst case, have to search through multiple levels of SSTables on the disk. Might take O(n) time. Can be improved through bloom filters and compaction techniques.
- Logging: Have to seek through entire log file several times. Requires O(n) time complexity.
##### Long range queries
- LSMs: Since data may be spread across multiple SSTables which are written sequentially on disk, less disk seeks are required.
- B-Trees: Data is not stored sequentially and requires more random I/O. Range query could be spread across multiple pages which will require more disk seeks.
- Logging: In this case too the the entire log file has to be read and will require O(n) time.
