# CSE330 Storage Management

In this directory, there are two scripts available to students for testing convenience.

## test_module.py

Before using this script, you must compile your kernel module and create two virtual disks that match the descriptions described in the document. This script can be used to test the kernel module. It will do the following when provided the path to your compiled kernel module (i.e., a .ko file kernel object), the path to the source block device, and the path to the cache block device, and a target hit rate:
1. The script will load your kernel module
2. The script will create an instance of the `dmcache` device mapper target using the cache and source devices you have provided
3. The script will run various workloads using the flexible IO tester (fio)
4. The script will finally check the stats from the cache and the amount of I/O's submitted to the block devices to verify the correctness of your code.

### Usage and expected output

Usage:
 - Note that you should only provide `0.0`, `0.5`, or `1.0` for the `target_hit_rate` argument.
```
Usage: sudo ./test_module.py /path/to/dmcache.ko /path/to/src_dev /path/to/cache_dev target_hit_rate
```

Expected output for test case 3:
```bash
[log]: Module loaded successfully
[log]: Creating cache
[log]: └─ echo 0 4194304 cache /dev/devX /dev/devY 8 262144 | sudo dmsetup create cache
[log]: Issue a 1024MB read workload
[log]: └─ sudo fio --filename=/dev/mapper/cache --name=test --rw=read --direct=1 --size=1024MB --numjobs=1
[log]: Checking cache status
[log]: └─ sudo dmsetup status cache
[log]: Issue a 1024MB read workload
[log]: └─ sudo fio --filename=/dev/mapper/cache --name=test --rw=read --direct=1 --size=1024MB --numjobs=1
[log]: Checking cache status
[log]: └─ sudo dmsetup status cache
[log]: Checking data read from cache device
[log]: └─ 1024.0MB read from cache device expected 1024MB
[log]: Checking number of cache hits
[log]: └─ Found 262144 hits expected 262144
[log]: Removing cache
[log]: └─ sudo dmsetup remove cache
[log]: Module unloaded successfully
[dmcache]: Passed (33.33/33.33)
```

## test_zip_contents.sh

This script is used to ensure that the final submission adheres to the expected format specified in the project document. It will do the following:
1. The script will unzip your submission into a directory `unzip_<unix_timestamp>`
2. The script will check for all the expected files within the `source_code` directory
3. The script will remove the directory it created `unzip_<unix_timestamp>`

Once the script is done running, it will inform you of the correctness of the submission by showing you anything it could not find.

Usage:
```bash
Usage: ./test_zip_contents.sh </path/to/your/submission.zip>
```

Expected output:
```bash
[log]: Look for directory (source_code)
[log]: ─ file /home/vboxuser/git/GTA-CSE330-Fall2024/Project6/testing/unzip_1733182339/source_code found
[log]: Look for source file (dm_lru.c)
[log]: ─ file /home/vboxuser/git/GTA-CSE330-Fall2024/Project6/testing/unzip_1733182339/source_code/dm_lru.c found
[test_zip_contents]: Passed
```
