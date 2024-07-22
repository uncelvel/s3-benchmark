# Introduction
s3-benchmark is a performance testing tool provided by Wasabi for performing S3 operations (PUT, GET, and DELETE) for objects. Besides the bucket configuration, the object size and number of threads varied be given for different tests.

The testing tool is loosely based on the Nasuni (http://www6.nasuni.com/rs/nasuni/images/Nasuni-2015-State-of-Cloud-Storage-Report.pdf) performance benchmarking methodologies used to test the performance of different cloud storage providers

# Prerequisites
To leverage this tool, the following prerequisites apply:
*	Git development environment
*	Ubuntu Linux shell programming skills
*	Access to a Go 1.7 development system (only if the OS is not Ubuntu Linux 16.04)
*	Access to the appropriate AWS EC2 (or equivalent) compute resource (optimal performance is realized using m4.10xlarge EC2 Ubuntu with 10 GB ENA)


# Building the Program
Obtain a local copy of the repository using the following git command with any directory that is convenient:

```
sudo apt install golang -y 
git clone https://github.com/uncelvel/s3-benchmark.git
rm -f s3-benchmark s3-benchmark.ubuntu 
go mod init example.com
go mod tidy 
go build s3-benchmark.go
```
 
# Command Line Arguments
Below are the command line arguments to the program (which can be displayed using -help):

```
  -a string
        Access key
  -b string
        Bucket for testing (default "wasabi-benchmark-bucket")
  -d int
        Duration of each test in seconds (default 60)
  -l int
        Number of times to repeat test (default 1)
  -s string
        Secret key
  -t int
        Number of threads to run (default 1)
  -u string
        URL for host with method prefix (default "http://s3.wasabisys.com")
  -z string
        Size of objects in bytes with postfix K, M, and G (default "1M")
```        

# Example Benchmark
Below is an example run of the benchmark for 10 threads with the default 1MB object size.  The benchmark reports
for each operation PUT, GET and DELETE the results in terms of data speed and operations per second.  The program
writes all results to the log file benchmark.log.

```
for thead in 1 2 4 8 16
do
    for size in 4k 16k 1M 2M 4M 16M 128M 
    do 
    echo "=========================="
    date 
    ./s3-benchmark -u https://han01.vstorage.vngcloud.vn -r HAN01 \
               -b ghtk-poc \
               -a "f29064732c538e51c7e2c8a4f62e5714" \
               -s "db746d0aa31ae679878bc9b0a6233732"  \
               -t $thead -z $size
    done 
    echo "=========================="
done
```

# Note
Your performance testing benchmark results may vary most often because of limitations of your network connection to the cloud storage provider.  Wasabi performance claims are tested under conditions that remove any latency (which can be shown using the ping command) and bandwidth bottlenecks that restrict how fast data can be moved.  For more information,
contact Wasabi technical support (support@wasabi.com).
