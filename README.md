# This is repo to track my progress of testing QAT with Intel® QuickAssist Adapter 8950

Product brief at the moment of writing this README is hosted there https://www.intel.com/content/www/us/en/ethernet-products/gigabit-server-adapters/quickassist-adapter-8950-brief.html

## Goals (I'm planning to make all testing on CentOS 7):

[ ] openssl QAT engine testing (using openssl benchmarks);

[x] haproxy testing;

### no QAT no SSL 

#### 10000 requests

```
$ ab -c 20 -n 10000  http://127.0.0.1:80/
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        nginx/1.14.0
Server Hostname:        127.0.0.1
Server Port:            80

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      20
Time taken for tests:   1.472 seconds
Complete requests:      10000
Failed requests:        0
Write errors:           0
Total transferred:      8450000 bytes
HTML transferred:       6120000 bytes
Requests per second:    6791.48 [#/sec] (mean)
Time per request:       2.945 [ms] (mean)
Time per request:       0.147 [ms] (mean, across all concurrent requests)
Transfer rate:          5604.30 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       1
Processing:     0    1  10.0      1    1004
Waiting:        0    1  10.0      1    1004
Total:          1    1  10.0      1    1004

Percentage of the requests served within a certain time (ms)
  50%      1
  66%      1
  75%      1
  80%      1
  90%      1
  95%      1
  98%      2
  99%      2
 100%   1004 (longest request)
```

### no QAT with SSL

#### 10000 requests 

```
$ ab -c 20 -n 10000 -f TLS1.2 https://127.0.0.1:443/
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        nginx/1.14.0
Server Hostname:        127.0.0.1
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,4096,256

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      20
Time taken for tests:   38.070 seconds
Complete requests:      10000
Failed requests:        0
Write errors:           0
Total transferred:      8450000 bytes
HTML transferred:       6120000 bytes
Requests per second:    262.67 [#/sec] (mean)
Time per request:       76.140 [ms] (mean)
Time per request:       3.807 [ms] (mean, across all concurrent requests)
Transfer rate:          216.76 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        4   65  15.6     69      88
Processing:     1   12  15.8      7      84
Waiting:        0   11  15.9      6      84
Total:          6   76   6.2     75     149

Percentage of the requests served within a certain time (ms)
  50%     75
  66%     76
  75%     76
  80%     76
  90%     77
  95%     85
  98%     90
  99%     95
 100%    149 (longest request)
 ```

### with QAT and with SSL, sync (CPU load was 100% haproxy) 

#### 10000 requests

```$ ab -c 20 -n 10000 -f TLS1.2 https://127.0.0.1:443/
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        nginx/1.14.0
Server Hostname:        127.0.0.1
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,4096,256

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      20
Time taken for tests:   31.243 seconds
Complete requests:      10000
Failed requests:        0
Write errors:           0
Total transferred:      8450000 bytes
HTML transferred:       6120000 bytes
Requests per second:    320.08 [#/sec] (mean)
Time per request:       62.485 [ms] (mean)
Time per request:       3.124 [ms] (mean, across all concurrent requests)
Transfer rate:          264.12 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        4   53  12.1     57      61
Processing:     2    9  12.1      6      59
Waiting:        0    9  12.1      5      59
Total:          6   62   2.0     62      98

Percentage of the requests served within a certain time (ms)
  50%     62
  66%     62
  75%     62
  80%     63
  90%     63
  95%     64
  98%     64
  99%     64
 100%     98 (longest request)
 ```
 
#### 100000 requests 

```ab -c 20 -n 100000 -f TLS1.2 https://127.0.0.1:443/
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient)
Completed 10000 requests
Completed 20000 requests
Completed 30000 requests
Completed 40000 requests
Completed 50000 requests
Completed 60000 requests
Completed 70000 requests
Completed 80000 requests
Completed 90000 requests
Completed 100000 requests
Finished 100000 requests


Server Software:        nginx/1.14.0
Server Hostname:        127.0.0.1
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,4096,256

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      20
Time taken for tests:   312.196 seconds
Complete requests:      100000
Failed requests:        0
Write errors:           0
Total transferred:      84500000 bytes
HTML transferred:       61200000 bytes
Requests per second:    320.31 [#/sec] (mean)
Time per request:       62.439 [ms] (mean)
Time per request:       3.122 [ms] (mean, across all concurrent requests)
Transfer rate:          264.32 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        4   54  11.5     57      61
Processing:     1    9  11.5      6      59
Waiting:        0    8  11.5      5      59
Total:          6   62   1.1     62     106

Percentage of the requests served within a certain time (ms)
  50%     62
  66%     62
  75%     63
  80%     63
  90%     63
  95%     64
  98%     64
  99%     64
 100%    106 (longest request)
```

### QAT with SSL, async (CPU load was ~ 67% haproxy) 

#### 10000 requests

```
$ ab -c 20 -n 10000 -f TLS1.2 https://127.0.0.1:443/
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        nginx/1.14.0
Server Hostname:        127.0.0.1
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,4096,256

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      20
Time taken for tests:   7.154 seconds
Complete requests:      10000
Failed requests:        0
Write errors:           0
Total transferred:      8450000 bytes
HTML transferred:       6120000 bytes
Requests per second:    1397.78 [#/sec] (mean)
Time per request:       14.308 [ms] (mean)
Time per request:       0.715 [ms] (mean, across all concurrent requests)
Transfer rate:          1153.44 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        7   12   1.7     12      18
Processing:     0    2   1.5      2       8
Waiting:        0    2   1.0      2       7
Total:          8   14   0.6     14      20

Percentage of the requests served within a certain time (ms)
  50%     14
  66%     14
  75%     15
  80%     15
  90%     15
  95%     15
  98%     16
  99%     16
 100%     20 (longest request)
```

#### 100000 requests 

```
$ ab -c 20 -n 100000 -f TLS1.2 https://127.0.0.1:443/
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient)
Completed 10000 requests
Completed 20000 requests
Completed 30000 requests
Completed 40000 requests
Completed 50000 requests
Completed 60000 requests
Completed 70000 requests
Completed 80000 requests
Completed 90000 requests
Completed 100000 requests
Finished 100000 requests


Server Software:        
Server Hostname:        127.0.0.1
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,4096,256

Document Path:          /
Document Length:        92 bytes

Concurrency Level:      20
Time taken for tests:   76.421 seconds
Complete requests:      100000
Failed requests:        99999
   (Connect: 0, Receive: 0, Length: 99999, Exceptions: 0)
Write errors:           0
Non-2xx responses:      1
Total transferred:      84499349 bytes
HTML transferred:       61199480 bytes
Requests per second:    1308.54 [#/sec] (mean)
Time per request:       15.284 [ms] (mean)
Time per request:       0.764 [ms] (mean, across all concurrent requests)
Transfer rate:          1079.79 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        6   12   1.7     12      19
Processing:     0    3  15.9      2    5005
Waiting:        0    2  15.8      2    5001
Total:          9   14  15.8     14    5016

Percentage of the requests served within a certain time (ms)
  50%     14
  66%     14
  75%     15
  80%     15
  90%     15
  95%     15
  98%     16
  99%     16
 100%   5016 (longest request)
```


[ ] nginx testing; 

[ ] ipsec (with all available implementations);
