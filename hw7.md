# 5.3
For a direct-mapped cache design with a 32-bit address, the following bits of the address are used to access the cache:
  * Tag 31-10
  * Index 9-5
  * Offset 4-0
* 1: What is the cache block size (in words)?
  * Block size in bytes can be determined from the number of offset bits. 5 offset bits means block size is 2^5 or 32 bytes. 32 bytes/block * (1 word / 4 bytes) = 8 words/block
* 2: How many entries does the cache have?
  * The cache is direct mapped, which means there is one entry per set. The number of sets can be determined by the number of index bits. 5 index bits means 2^5 or 32 sets. Because there is 1 entry per set, there are 32 entries total. 
* 4: Starting from power on, the following byte-addressed cache references are recorded: 0, 4, 16, 132, 232, 160, 1024, 30, 140, 3100, 180, 2180. How many blocks are replaced?
  * Go through each row in table
    * If index has never been used before:
      * Miss
    * If index has been used before:
      * Does the tag also match the tag for the last time this index was used?
        * If yes, hit
        * If no, miss and replace new tag
# 5.5
* Media applications that play audio or video files are part of a class of workloads called “streaming” workloads; i.e., they bring in large amounts of data but do not reuse much of it. Consider a video streaming workload that accesses a 512 KiB working set sequentially with the following address stream: 0, 2, 4, 6, 8, 10, 12, 14, 16, ...
* 1: Assume a 64 KiB direct-mapped cache with a 32-byte block. What is the miss rate for the address stream above? How is this miss rate sensitive to the size of the cache or the working set? How would you categorize the misses this workload is experiencing, based on the 3C model?
  * Because the block size is 32 bytes, the block corresponds to 32 contiguous addresses. Thus, for this particular access pattern of 0, 2, 4, … where each access is incremented by 2, the pattern will span a single cache block in 16 accesses. Based on this access pattern and the block size of 32 bytes, for every 16 accesses, there will be one miss. This results in a miss rate of 1/16. This miss occurs because the block has not previous been seen in the cache, making it a cold or compulsory miss. 
* 2: Re-compute the miss rate when the cache block size is 16 bytes, 64 bytes, and 128 bytes. What kind of locality is this workload exploiting?
  * For a 16 byte block size:
    * 8 accesses will span a single block with a single cold miss for every 8 accesses.
    * Miss rate: 1/8
  * For a 64 byte block size:
    * 32 accesses will span a single block.
    * Miss rate: 1/32
  * For a 128 byte block size:
    * Miss rate: 1/64
  * This is exploiting spatial locality, or the idea that if an item is referenced, nearby items are likely to be referenced in the near future. 
  
