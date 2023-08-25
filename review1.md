### B-trees, logging, and LSMs : Compare these different ways of organizing data (in terms of performance) for updates, reads, and range queries. Arrange the methods for each workload (from best performing to worst performing). Keep your answers short.
---
Reading: [Wisckey](https://www.usenix.org/system/files/conference/fast16/fast16-papers-lu.pdf), [LSMs](https://www.cs.umb.edu/~poneil/lsmtree.pdf) [optional]
||Updates|Reads|Range Queries|
|---|---|---|---|
|**Best**|LSMs|B-trees|LSMs|
|**Better**|Logging|LSMs|B-trees|
|**Good**|B-trees|Logging|Logging|

The reasoning for the above answer is below

#### Updates
- LSMs: Write optimized. Writes to in-memory buffer and are flushed in batches, sequentially to disk.
- Logging: A single line is appended to end of log file. Writes sequentially directly on disk.
- B-trees: Involves random disk writes, may involve several pages/blocks and entire page overwrite

#### Reads
- B-trees: Read optimized. The index is a self balanced tree which allows access to value in O(log n) time.
- LSMs: Worst case have to iterate through every SSTable starting from topmost level on disk to find value. Can take O(n log n) time
- Logging: Have to iterate through all log lines from bottom. Will take O(n) time.

#### Range Queries
- LSMs: Faster because data is sequentially written to disk and is levelled and compacted to take less space.
- B-Trees: Data is not stored sequentially and requires more random I/O. Range query could be spread across multiple pages which will require more disk seeks.
- Logging: Have to seek through entire log file several times. Requires O(n) time complexity.
