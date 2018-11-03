# This is repo to track my progress of testing QAT with Intel® QuickAssist Adapter 8950

Product brief at the moment of writing this README is hosted there https://www.intel.com/content/www/us/en/ethernet-products/gigabit-server-adapters/quickassist-adapter-8950-brief.html

## My current setup 

```
lspci
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Host Bridge/DRAM Registers (rev 07)
00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor PCIe Controller (x16) (rev 07)
00:02.0 VGA compatible controller: Intel Corporation HD Graphics 530 (rev 06)
00:14.0 USB controller: Intel Corporation Sunrise Point-H USB 3.0 xHCI Controller (rev 31)
00:16.0 Communication controller: Intel Corporation Sunrise Point-H CSME HECI #1 (rev 31)
00:1b.0 PCI bridge: Intel Corporation Sunrise Point-H PCI Root Port #17 (rev f1)
00:1c.0 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #1 (rev f1)
00:1c.2 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #3 (rev f1)
00:1c.3 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #4 (rev f1)
00:1c.4 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #5 (rev f1)
00:1d.0 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #9 (rev f1)
00:1f.0 ISA bridge: Intel Corporation Sunrise Point-H LPC Controller (rev 31)
00:1f.2 Memory controller: Intel Corporation Sunrise Point-H PMC (rev 31)
00:1f.3 Audio device: Intel Corporation Sunrise Point-H HD Audio (rev 31)
00:1f.4 SMBus: Intel Corporation Sunrise Point-H SMBus (rev 31)
01:00.0 Ethernet controller: Intel Corporation I350 Gigabit Network Connection (rev 01)
01:00.1 Ethernet controller: Intel Corporation I350 Gigabit Network Connection (rev 01)
01:00.2 Ethernet controller: Intel Corporation I350 Gigabit Network Connection (rev 01)
01:00.3 Ethernet controller: Intel Corporation I350 Gigabit Network Connection (rev 01)
03:00.0 USB controller: ASMedia Technology Inc. ASM1142 USB 3.1 Host Controller
04:00.0 PCI bridge: ASMedia Technology Inc. ASM1083/1085 PCIe to PCI Bridge (rev 04)
06:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 15)
07:00.0 PCI bridge: PLX Technology, Inc. PEX 8724 24-Lane, 6-Port PCI Express Gen 3 (8 GT/s) Switch, 19 x 19mm FCBGA (rev ca)
08:00.0 PCI bridge: PLX Technology, Inc. PEX 8724 24-Lane, 6-Port PCI Express Gen 3 (8 GT/s) Switch, 19 x 19mm FCBGA (rev ca)
08:08.0 PCI bridge: PLX Technology, Inc. PEX 8724 24-Lane, 6-Port PCI Express Gen 3 (8 GT/s) Switch, 19 x 19mm FCBGA (rev ca)
09:00.0 Co-processor: Intel Corporation DH895XCC Series QAT
0b:00.0 Non-Volatile memory controller: Intel Corporation PCIe Data Center SSD (rev 01)
```

```# cat /proc/cpuinfo 
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 94
model name	: Intel(R) Core(TM) i7-6700K CPU @ 4.00GHz
stepping	: 3
microcode	: 0xc6
cpu MHz		: 799.804
cache size	: 8192 KB
physical id	: 0
siblings	: 8
core id		: 0
cpu cores	: 4
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 22
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch intel_pt ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm mpx rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 dtherm arat pln pts hwp hwp_notify hwp_act_window hwp_epp spec_ctrl intel_stibp flush_l1d
bogomips	: 8016.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 39 bits physical, 48 bits virtual
power management:

```

```
# cat /etc/dh895xcc_dev0.conf 
#########################################################################
#
# @par
# This file is provided under a dual BSD/GPLv2 license.  When using or
#   redistributing this file, you may do so under either license.
#
#   GPL LICENSE SUMMARY
#
#   Copyright(c) 2007-2018 Intel Corporation. All rights reserved.
#
#   This program is free software; you can redistribute it and/or modify
#   it under the terms of version 2 of the GNU General Public License as
#   published by the Free Software Foundation.
#
#   This program is distributed in the hope that it will be useful, but
#   WITHOUT ANY WARRANTY; without even the implied warranty of
#   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
#   General Public License for more details.
#
#   You should have received a copy of the GNU General Public License
#   along with this program; if not, write to the Free Software
#   Foundation, Inc., 51 Franklin St - Fifth Floor, Boston, MA 02110-1301 USA.
#   The full GNU General Public License is included in this distribution
#   in the file called LICENSE.GPL.
#
#   Contact Information:
#   Intel Corporation
#
#   BSD LICENSE
#
#   Copyright(c) 2007-2018 Intel Corporation. All rights reserved.
#
#   Redistribution and use in source and binary forms, with or without
#   modification, are permitted provided that the following conditions
#   are met:
#
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in
#       the documentation and/or other materials provided with the
#       distribution.
#     * Neither the name of Intel Corporation nor the names of its
#       contributors may be used to endorse or promote products derived
#       from this software without specific prior written permission.
#
#   THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
#   "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
#   LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
#   A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
#   OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
#   SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
#   LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
#   DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
#   THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
#   (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
#   OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#
#
#########################################################################
########################################################
#
# This file is the configuration for a single dh895xcc_qa
# device.
#
# Each device has 32 independent banks.
#
# - Each bank can contain up to 2 crypto and/or up to 2 data
#   compression services.
#
# - The interrupt for each can be directed to a
#   specific core.
#
########################################################

##############################################
# General Section
##############################################

[GENERAL]
ServicesEnabled = cy

# Use version 2 of the config file
ConfigVersion = 2

# Look Aside Cryptographic Configuration
cyHmacAuthMode = 1

# Wireless Enable/Disable, valid values: 1,0
WirelessEnabled = 0

# Firmware Location Configuration
Firmware_MofPath = dh895xcc/mof_firmware.bin
Firmware_MmpPath = dh895xcc/mmp_firmware.bin

# Default selection of CY concurrent requests.
CyNumConcurrentSymRequests = 512
CyNumConcurrentAsymRequests = 64

# Default number of DC concurrent requests.
DcNumConcurrentRequests = 512

#Statistics, valid values: 1,0
statsGeneral = 1
statsDc = 1
statsDh = 1
statsDrbg = 1
statsDsa = 1
statsEcc = 1
statsKeyGen = 1
statsLn = 1
statsPrime = 1
statsRsa = 1
statsSym = 1

# Debug feature, if set to 1 it enables additional entries in /proc filesystem
ProcDebug = 1

# Enables or disables Single Root Complex IO Virtualization.
# If this is enabled (1) then SRIOV and VT-d need to be enabled in
# BIOS and there can be no Cy or Dc instances created in PF (Dom0).
# If this is disabled (0) then SRIOV and VT-d needs to be disabled
# in the BIOS and Cy and/or Dc instances can be used in PF (Dom0)
SRIOV_Enabled = 0

#######################################################
#
# Logical Instances Section
# A logical instance allows each address domain
# (kernel space and individual user space processes)
# to configure rings (i.e. hardware assisted queues)
# to be used by that address domain and to define the
# behavior of that ring.
#
# The address domains are in the following format
# - For kernel address domains
#       [KERNEL]
# - For user process address domains
#   [xxxxx]
#   Where xxxxx may be any ascii value which uniquely identifies
#   the user mode process.
#   To allow the driver correctly configure the
#   logical instances associated with this user process,
#   the process must call the icp_sal_userStartMultiProcess(...)
#   passing the xxxxx string during process initialisation.
#   When the user space process is finished it must call
#   icp_sal_userStop(...) to free resources.
#   NumProcesses will indicate the maximum number of processes
#   that can call icp_sal_userStartMultiProcess on this instance.
#   Warning: the resources are preallocated: if NumProcesses
#   is too high, the driver will fail to load
#
# Items configurable by a logical instance are:
# - Name of the logical instance
# - The response mode associated wth this logical instance (0
#   for IRQ or 1 for polled).
# - The core the instance is affinitized to (optional)
#
# The format of the logical instances are:
# - For crypto:
#               Cy<n>Name = "xxxx"
#               Cy<n>IsPolled = 0|1
#               Cy<n>CoreAffinity = 0-7
#
# - For Data Compression
#               Dc<n>Name = "xxxx"
#               Dc<n>IsPolled = 0|1
#               Dc<n>CoreAffinity = 0-7
#
# Where:
#       - n is the number of this logical instance starting at 0.
#       - xxxx may be any ascii value which identifies the logical instance.
#
# Note: for user space processes, a list of values can be specified for
# the core affinity: for example
#              Cy0CoreAffinity = 0,2,4
# These comma-separated lists will allow multiple processes to use
# different accelerators and cores, and will wrap around the numbers
# in the list. In the above example, process 0 will have affinity 0,
# and process 1 will have affinity 2 etc.
#
########################################################

# This flag is to enable SSF features (CNV and BnP)
StorageEnabled = 0

##############################################
# Kernel Instances Section
##############################################
[KERNEL]
NumberCyInstances = 0
NumberDcInstances = 0

##############################################
# User Process Instance Section
##############################################
[SHIM]
NumberCyInstances = 2
NumberDcInstances = 0
NumProcesses = 4
LimitDevAccess = 0

# Crypto - User space
Cy0Name = "UserCY0"
Cy0IsPolled = 1
Cy0CoreAffinity = 0

# Crypto - User space
Cy1Name = "UserCY1"
Cy1IsPolled = 1
Cy1CoreAffinity = 0
```

```
# /etc/init.d/qat_service status
Checking status of all devices.
There is 1 QAT acceleration device(s) in the system:
 qat_dev0 - type: dh895xcc,  inst_id: 0,  node_id: 0,  bsf: 09:00.0,  #accel: 6 #engines: 12 state: up
 ```  
 
 ```
 # cat /sys/kernel/debug/qat_dh895xcc_09\:00.0/fw_counters 
+------------------------------------------------+
| FW Statistics for Qat Device                   |
+------------------------------------------------+
| Firmware Requests [AE  0]:               83909 |
| Firmware Responses[AE  0]:               83909 |
+------------------------------------------------+
| Firmware Requests [AE  1]:               24315 |
| Firmware Responses[AE  1]:               24315 |
+------------------------------------------------+
| Firmware Requests [AE  2]:               32609 |
| Firmware Responses[AE  2]:               32609 |
+------------------------------------------------+
| Firmware Requests [AE  3]:               29851 |
| Firmware Responses[AE  3]:               29851 |
+------------------------------------------------+
| Firmware Requests [AE  4]:               24333 |
| Firmware Responses[AE  4]:               24333 |
+------------------------------------------------+
| Firmware Requests [AE  5]:               71307 |
| Firmware Responses[AE  5]:               71307 |
+------------------------------------------------+
| Firmware Requests [AE  6]:               42264 |
| Firmware Responses[AE  6]:               42264 |
+------------------------------------------------+
| Firmware Requests [AE  7]:               31724 |
| Firmware Responses[AE  7]:               31724 |
+------------------------------------------------+
| Firmware Requests [AE  8]:               72607 |
| Firmware Responses[AE  8]:               72607 |
+------------------------------------------------+
| Firmware Requests [AE  9]:               38105 |
| Firmware Responses[AE  9]:               38105 |
+------------------------------------------------+
| Firmware Requests [AE 10]:               72260 |
| Firmware Responses[AE 10]:               72260 |
+------------------------------------------------+
| Firmware Requests [AE 11]:               23258 |
| Firmware Responses[AE 11]:               23258 |
+------------------------------------------------+
```

## My Goals (I'm planning to make all testing on CentOS 7, because Intel guys don't have experience with Debian/Ubuntu)

[ ] openssl QAT engine testing (using openssl benchmarks);

[x] haproxy testing. I'm doing this tests with simplest setup and nginx webserver as backend server.

#### haproxy config file
```
# cat /etc/haproxy/haproxy.cfg 
global
	tune.ssl.default-dh-param 2048
	maxconn 100000
  # option is used to enable QAT
	ssl-engine qat algo RSA
  # option is used to enable QAT async
  ssl-mode-async

defaults
        mode http
        timeout connect 5s
        timeout client 5s
        timeout server 5s

frontend myfrontend
         bind *:80
         bind *:443 ssl crt /etc/haproxy/server.pem
         default_backend mybackend

 backend mybackend
         balance roundrobin
         mode http
         server myserver1 127.0.0.1:8080 check

```

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
