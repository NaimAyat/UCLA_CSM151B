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
