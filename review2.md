### Consider a simple file system like ext2. For simplicity, one inode in this FS occupies one FS block. Consider a directory "/home" inside which there are 10 directories named d1, d2,...d10. Each directory contains one file inside it.
### With an initial clean cache and no prefetching, roughly how many blocks must be read from the disk to complete a "find" command issued from "/home" for a file "foo"?
### Would this workload perform better or worse on BetrFS? Why? Make any required assumptions and state them.
---
Reading: [BetrFS](https://www.usenix.org/system/files/conference/fast15/fast15-paper-jannen_william.pdf), [B<sup>&#949;</sup>-trees](https://www.usenix.org/system/files/login/articles/login_oct15_05_bender.pdf)

Assuming that the file is located at path "/home/d10/foo", following are the blocks we will fetch in ext2 file system:
- Inode for root "/" followed by it's datablock (1+1)
- Inode for "/home" followed by it's datablock (1+1)
- Inode and datablock for each of the 10 directories under "/home" (10 + 10)
- Just the Inode for the file "foo" from directory "d10" because find just lists the metadata (1)

That gives us a total of **25 Blocks** accessed by the **ext2 file system** for the find operation on file "foo"

**BetrFS would be more efficient for this workload** as it utilises a B<sup>&#949;</sup>-tree. 
- It maintains a separate metadata index which has the metadata of the files and directories on the file system.
- This metadata index will be used by the *find* command.
- The keys in the index are relative pathnames sorted in a lexographic order. This enables same directories and their files metadata to be stored consecutively.
- This also means that the directory scans can be done as a range query.
- The tree root and the internal nodes are stored in memory. The leaf nodes containing the file metadata need to be fetched.
- In this case assuming each file path occupies one data block, **10 data blocks** need to be fetched.
- The advantage with B<sup>&#949;</sup>-tree is that each node (~ 1MB) is typically bigger than a data block (~ 4KB) size.
- Since multiple data blocks can be present in one node and the metadata is sequentially stored, we will need to fetch only one node from disk and perform sequential search for the required data block.

