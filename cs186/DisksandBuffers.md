# Disks and Buffers

## Architecture of a DBMS: SQL Client

- Parsing & Optimization
- Relational Operators 
- Files and Index Management
- Buffer Management
- Disk Space Management
- ![image-20220201090356386](https://gitee.com/dongramesez/typora-img/raw/master/img/202202010903494.png)
- Concurrency & Recovery: Two cross-cutting issues related to storage and memory management.
- ![image-20220201090635932](https://gitee.com/dongramesez/typora-img/raw/master/img/202202010906975.png)

## Review: STORAGE MEDIA

- Disks: slow, cheap, as Database and backup/logs, Secondary & tertiary storage

- Components of a Disk

  ![image-20220201092123105](https://gitee.com/dongramesez/typora-img/raw/master/img/202202010921155.png)

  - Only one head reads/writes at any one time
  - Block/page size is a multiple of (fixed) sector

- Notes on Flash(SSD)

  Read is fast and predictable

  - Single read access time: 0.03 ms
  -  4KB random reads: ~500MB/sec
  - Sequential reads: ~525MB/sec
  -  64K: 0.48 ms

  BUT write is not! Slower for random

  - Single write access time: 0.03 ms
  - 4KB random writes: ~120 MB/sec
  -  Sequential writes: ~480 MB/sec

- Disk Space Management: Implementation:

  Proposal:

  - Talk to the storage device directly
  - Run over filesystem (FS)

- ![image-20220201101502179](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011015227.png)

## Representations

![image-20220201101649264](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011016315.png)

1. Files of Pages of Records

   ![image-20220201104250665](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011042703.png)

2. DB FILE: A collection of pages, each containing a collection of records.

   - API for higher layers of the DBMS: 1. Insert/delete/modify record 2. Fetch a particular record by record id
   - Could span multiple OS files and even machines
     • Or “raw” disk devices

3. ![image-20220201105315871](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011053912.png)

4. Heap File Implemented as List

5. Better: Use a Page Directory

   ![image-20220201110722637](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011107684.png)

## Page Layout

1. The Header may contain:
   - Number of records
   - Free space
   - Maybe a next/last pointer
   - Bitmap, Slot Table
2. ![image-20220201111505543](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011115596.png)
3. ![image-20220201111833433](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011118482.png)
4. ![image-20220201112307446](https://gitee.com/dongramesez/typora-img/raw/master/img/202202011123494.png)

## Record Layout

Record, easy case: Fixed length Fields, interesting case: variable length fields

1. Fixed Length
   - Field types same for all records in a file.
   - On disk byte representation same as in memory
   - Finding i’th field (done via arithmetic)(fast)
   - Compact? (Nulls?)
2. Variable Length
   - Could use delimiters
   - ![image-20220201160833959](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201160833959.png)

![image-20220201165242123](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201165242123.png)

![image-20220201165943802](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201165943802.png)

## File Organizations

![image-20220201170234063](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201170234063.png)

### Cost Model for Analysis

- B: The number of data blocks in the file
-  R: Number of records per block
-  D: (Average) time to read/write disk block

![image-20220201171131484](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201171131484.png)

![image-20220201171632731](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201171632731.png)