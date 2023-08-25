### B-trees, logging, and LSMs : Compare these different ways of organizing data (in terms of performance) for updates, reads, and range queries. Arrange the methods for each workload (from best performing to worst performing). Keep your answers short.
---
Reading: [Wisckey](https://www.usenix.org/system/files/conference/fast16/fast16-papers-lu.pdf), [LSMs](https://www.cs.umb.edu/~poneil/lsmtree.pdf) [optional]
|Updates|Reads|Range Queries|
|---|---|---|
|LSMs|B-trees|LSMs|
|Logging|LSMs|B-trees|
|B-trees|Logging|Logging|
