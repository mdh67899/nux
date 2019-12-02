nux
===

*nux metric collector

## metrci信息获取规则

### cpu信息获取规则
1. cpu数量: golang自带的 ```runtime.NumCPU()```， shell取值可以使用```cat /proc/cpuinfo  | grep "processor" | wc -l```的方式
1. cpu频率: 从机器的```/proc/cpuinfo```文件中获取
<pre>
$ cat /proc/cpuinfo | grep "cpu MHz"
cpu MHz		: 500.018
cpu MHz		: 500.013
cpu MHz		: 500.009
cpu MHz		: 500.022
$
</pre>

### cpu使用情况获取规则
通过从```/proc/stat```文件获取
<pre>
$ cat /proc/stat
cpu  200452 1024 55433 1126902 23300 0 4980 0 0 0
cpu0 49831 218 13838 283362 5441 0 956 0 0 0
cpu1 49978 196 13709 283762 4551 0 1211 0 0 0
cpu2 51543 379 13867 277668 7834 0 1152 0 0 0
cpu3 49098 230 14019 282108 5472 0 1660 0 0 0
intr 4012342 9 15 0 0 0 0 0 0 1 9169 0 0 0 0 0 0 0 758 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 2 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 78054 183528 105867 561129 37 34452 921 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
ctxt 13741632
btime 1575250974
processes 14589
procs_running 2
procs_blocked 0
softirq 4102168 686630 1130994 620 120792 171014 0 20016 1075016 0 897086
$
</pre>

cpu序号或者整体|User(普通进程执行的时间)|Nice(某些特殊优先级进程执行的时间)|System(进程在内核模式执行时间)|Idle(cpu空闲时间)|Iowait(等待io执行时间)|Irq(服务中断)|SoftIrq（软中断）|Steal|Guest|guest_nice|
:------------ | ------------- | ------------ | ------------| :------------|------------|------------|------------|
cpu|200452| 1024| 55433| 1126902| 23300| 0| 4980| 0| 0| 0
cpu0|49831| 218| 13838| 283362| 5441| 0| 956| 0| 0| 0

类型|意义|数值
:------------ | ------------- |
ctxt|所有cpu执行的上下文切换(context switches)| 13741632
btime|系统启动时间，unix时间戳类型，单位是秒| 1575250974
processes|进程和线程创建数| 14589
procs_running|有多少线程正在运行或者准备去运行| 2
procs_blocked|有多少个进程被阻塞住了，或者等待i/o完成| 0

### 磁盘信息获取方式
从```/proc/mounts```文件中获取目录挂载列表
<pre>
$ cat /proc/mounts
sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0
proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
udev /dev devtmpfs rw,nosuid,relatime,size=1944884k,nr_inodes=486221,mode=755 0 0
devpts /dev/pts devpts rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
tmpfs /run tmpfs rw,nosuid,noexec,relatime,size=395116k,mode=755 0 0
/dev/sda1 / ext4 rw,relatime,errors=remount-ro,data=ordered 0 0
securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime 0 0
tmpfs /dev/shm tmpfs rw,nosuid,nodev 0 0
tmpfs /run/lock tmpfs rw,nosuid,nodev,noexec,relatime,size=5120k 0 0
tmpfs /sys/fs/cgroup tmpfs ro,nosuid,nodev,noexec,mode=755 0 0
cgroup /sys/fs/cgroup/systemd cgroup rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/lib/systemd/systemd-cgroups-agent,name=systemd 0 0
pstore /sys/fs/pstore pstore rw,nosuid,nodev,noexec,relatime 0 0
cgroup /sys/fs/cgroup/rdma cgroup rw,nosuid,nodev,noexec,relatime,rdma 0 0
cgroup /sys/fs/cgroup/memory cgroup rw,nosuid,nodev,noexec,relatime,memory 0 0
cgroup /sys/fs/cgroup/hugetlb cgroup rw,nosuid,nodev,noexec,relatime,hugetlb 0 0
cgroup /sys/fs/cgroup/cpu,cpuacct cgroup rw,nosuid,nodev,noexec,relatime,cpu,cpuacct 0 0
cgroup /sys/fs/cgroup/net_cls,net_prio cgroup rw,nosuid,nodev,noexec,relatime,net_cls,net_prio 0 0
cgroup /sys/fs/cgroup/devices cgroup rw,nosuid,nodev,noexec,relatime,devices 0 0
cgroup /sys/fs/cgroup/cpuset cgroup rw,nosuid,nodev,noexec,relatime,cpuset 0 0
cgroup /sys/fs/cgroup/perf_event cgroup rw,nosuid,nodev,noexec,relatime,perf_event 0 0
cgroup /sys/fs/cgroup/freezer cgroup rw,nosuid,nodev,noexec,relatime,freezer 0 0
cgroup /sys/fs/cgroup/pids cgroup rw,nosuid,nodev,noexec,relatime,pids 0 0
cgroup /sys/fs/cgroup/blkio cgroup rw,nosuid,nodev,noexec,relatime,blkio 0 0
systemd-1 /proc/sys/fs/binfmt_misc autofs rw,relatime,fd=29,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=1569 0 0
debugfs /sys/kernel/debug debugfs rw,relatime 0 0
mqueue /dev/mqueue mqueue rw,relatime 0 0
hugetlbfs /dev/hugepages hugetlbfs rw,relatime,pagesize=2M 0 0
sunrpc /run/rpc_pipefs rpc_pipefs rw,relatime 0 0
configfs /sys/kernel/config configfs rw,relatime 0 0
fusectl /sys/fs/fuse/connections fusectl rw,relatime 0 0
tmpfs /run/user/1000 tmpfs rw,nosuid,nodev,relatime,size=395116k,mode=700,uid=1000,gid=1000 0 0
gvfsd-fuse /run/user/1000/gvfs fuse.gvfsd-fuse rw,nosuid,nodev,relatime,user_id=1000,group_id=1000 0 0
$
</pre>
内容格式是设备路径，挂载点，文件类型，挂载参数，然后排除那些不需要收集的目录和文件类型，然后再根据挂载点，使用syscall来获取挂载点的信息


### 获取网卡信息
通过读取```/proc/net/dev```文件来获取的
<pre>
$ cat /proc/net/dev
Inter-|   Receive                                                |  Transmit
 face |bytes    packets errs drop fifo frame compressed multicast|bytes    packets errs drop fifo colls carrier compressed
wlp3s0:    3661      21    0    0    0     0          0         0    15826      91    0    0    0     0       0          0
    lo: 14410802   31020    0    0    0     0          0         0 14410802   31020    0    0    0     0       0          0
enp2s0: 47586104   81766    0    0    0     0          0      1648 20602434   73839    0    0    0     0       0          0
$
</pre>

格式说明:

网卡接口名称|收到的数据字节数|收到的数据包数|收到的错误数据包数|drop掉的数据包数量|fifo错误|frame错误|压缩数据包个数|多播个数|发送的数据字节数|发送的数据包数|发送数据包错误数|发送过程drop掉的数据包数量|发送过程fifo错误|colls|carrier|发送压缩数据包个数|
:------------ | ------------- | ------------ | ------------| :------------|------------|------------|------------|
face |bytes|    packets| errs| drop| fifo| frame| compressed| multicast|bytes|    packets| errs| drop| fifo| colls| carrier| compressed
wlp3s0:|    3661|      21|    0|    0|    0|     0|          0|         0|    15826|      91|    0|    0|    0|     0|       0|          0
    lo:| 14410802|   31020|    0|    0|    0|     0|          0|         0| 14410802|   31020|    0|    0|    0|     0|       0|          0
enp2s0:| 47586104|   81766|    0|    0|    0|     0|          0|      1648| 20602434|   73839|    0|    0|    0|     0|       0|          0

再根据```/sys/class/net/%s/speed```目录，找到网卡接口的速率，如果内核版太低，就使用```ethtool iface_name```的方式来获取网卡速率


### 磁盘统计信息获取
根据```/proc/diskstats```文件统计信息来获取
<pre>
$ cat /proc/diskstats
   7       0 loop0 10 0 32 0 0 0 0 0 0 0 0
   7       1 loop1 0 0 0 0 0 0 0 0 0 0 0
   7       2 loop2 0 0 0 0 0 0 0 0 0 0 0
   7       3 loop3 0 0 0 0 0 0 0 0 0 0 0
   7       4 loop4 0 0 0 0 0 0 0 0 0 0 0
   7       5 loop5 0 0 0 0 0 0 0 0 0 0 0
   7       6 loop6 0 0 0 0 0 0 0 0 0 0 0
   7       7 loop7 0 0 0 0 0 0 0 0 0 0 0
   8       0 sda 216132 156672 7966738 1009864 89073 629622 6906168 4537136 0 270064 5546752
   8       1 sda1 216080 156672 7963570 1009852 87722 629622 6906168 4527888 0 263640 5537472
$
</pre>

格式说明:
+ https://www.kernel.org/doc/Documentation/ABI/testing/procfs-diskstats
+ https://www.kernel.org/doc/Documentation/admin-guide/iostats.rst
<pre>
		 1 - major number
		 2 - minor mumber
		 3 - device name
		 4 - reads completed successfully
		 5 - reads merged
		 6 - sectors read
		 7 - time spent reading (ms)
		 8 - writes completed
		 9 - writes merged
		10 - sectors written
		11 - time spent writing (ms)
		12 - I/Os currently in progress
		13 - time spent doing I/Os (ms)
		14 - weighted time spent doing I/Os (ms)

		Kernel 4.18+ appends four more fields for discard
		tracking putting the total at 18:

		15 - discards completed successfully
		16 - discards merged
		17 - sectors discarded
		18 - time spent discarding
</pre>

### 内核信息状态获取
+ ```/proc/sys/fs/file-max```: 表示系统级别的能够打开的文件句柄的数量。是对整个系统的限制，并不是针对用户的
+ ```/proc/sys/fs/file-nr```: 当前使用的文件句柄数

### 机器负载值获取
主要是从```/proc/loadavg```文件获取
<pre>
$ cat /proc/loadavg
2.57 2.85 2.07 2/748 25035
$
</pre>

文件格式说明:

机器1分钟的负载|机器5分钟的负载|机器10分钟的负载|当前处于running状态的进程数/机器上所有的进程数|最被使用的进程id，有可能是进程id分配
:------------ | ------------- | ------------ |
2.57| 2.85| 2.07| 2/748| 25035


### 机器内存使用状态获取
主要是从```/proc/meminfo```文件获取的, 内核文档出处: https://www.kernel.org/doc/Documentation/filesystems/proc.txt
<pre>
$ cat /proc/meminfo
MemTotal:       32727660 kB
MemFree:         1122416 kB
MemAvailable:   18518380 kB
Buffers:            2572 kB
Cached:         17846512 kB
SwapCached:            0 kB
Active:         16617864 kB
Inactive:       12931248 kB
Active(anon):    7635868 kB
Inactive(anon):  5047476 kB
Active(file):    8981996 kB
Inactive(file):  7883772 kB
Unevictable:        9096 kB
Mlocked:            9096 kB
SwapTotal:       4194300 kB
SwapFree:        4194300 kB
Dirty:              1144 kB
Writeback:             0 kB
AnonPages:      11709364 kB
Mapped:           437320 kB
Shmem:            979680 kB
Slab:            1253156 kB
SReclaimable:     957612 kB
SUnreclaim:       295544 kB
KernelStack:       70864 kB
PageTables:        71176 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:    20558128 kB
Committed_AS:   34884860 kB
VmallocTotal:   34359738367 kB
VmallocUsed:      478164 kB
VmallocChunk:   34342143996 kB
HardwareCorrupted:     0 kB
AnonHugePages:   2928640 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      363456 kB
DirectMap2M:     7979008 kB
DirectMap1G:    25165824 kB
$
</pre>

### Tcp连接信息获取
主要是通过从```/proc/net/netstat```文件来获取的
<pre>
$ cat /proc/net/netstat
TcpExt: SyncookiesSent SyncookiesRecv SyncookiesFailed EmbryonicRsts PruneCalled RcvPruned OfoPruned OutOfWindowIcmps LockDroppedIcmps ArpFilter TW TWRecycled TWKilled PAWSPassive PAWSActive PAWSEstab DelayedACKs DelayedACKLocked DelayedACKLost ListenOverflows ListenDrops TCPPrequeued TCPDirectCopyFromBacklog TCPDirectCopyFromPrequeue TCPPrequeueDropped TCPHPHits TCPHPHitsToUser TCPPureAcks TCPHPAcks TCPRenoRecovery TCPSackRecovery TCPSACKReneging TCPFACKReorder TCPSACKReorder TCPRenoReorder TCPTSReorder TCPFullUndo TCPPartialUndo TCPDSACKUndo TCPLossUndo TCPLostRetransmit TCPRenoFailures TCPSackFailures TCPLossFailures TCPFastRetrans TCPForwardRetrans TCPSlowStartRetrans TCPTimeouts TCPLossProbes TCPLossProbeRecovery TCPRenoRecoveryFail TCPSackRecoveryFail TCPSchedulerFailed TCPRcvCollapsed TCPDSACKOldSent TCPDSACKOfoSent TCPDSACKRecv TCPDSACKOfoRecv TCPAbortOnData TCPAbortOnClose TCPAbortOnMemory TCPAbortOnTimeout TCPAbortOnLinger TCPAbortFailed TCPMemoryPressures TCPSACKDiscard TCPDSACKIgnoredOld TCPDSACKIgnoredNoUndo TCPSpuriousRTOs TCPMD5NotFound TCPMD5Unexpected TCPSackShifted TCPSackMerged TCPSackShiftFallback TCPBacklogDrop PFMemallocDrop TCPMinTTLDrop TCPDeferAcceptDrop IPReversePathFilter TCPTimeWaitOverflow TCPReqQFullDoCookies TCPReqQFullDrop TCPRetransFail TCPRcvCoalesce TCPOFOQueue TCPOFODrop TCPOFOMerge TCPChallengeACK TCPSYNChallenge TCPFastOpenActive TCPFastOpenActiveFail TCPFastOpenPassive TCPFastOpenPassiveFail TCPFastOpenListenOverflow TCPFastOpenCookieReqd TCPSpuriousRtxHostQueues BusyPollRxPackets TCPAutoCorking TCPFromZeroWindowAdv TCPToZeroWindowAdv TCPWantZeroWindowAdv TCPSynRetrans TCPOrigDataSent TCPHystartTrainDetect TCPHystartTrainCwnd TCPHystartDelayDetect TCPHystartDelayCwnd TCPACKSkippedSynRecv TCPACKSkippedPAWS TCPACKSkippedSeq TCPACKSkippedFinWait2 TCPACKSkippedTimeWait TCPACKSkippedChallenge TCPWqueueTooBig
TcpExt: 0 0 17 6 0 0 0 35 0 0 3501675 0 11192 0 0 6 1360945 7137 5563 0 0 1 23 66 0 71054242 2 116433578 43813599 0 652 0 0 0 0 0 0 0 21 1181 122 0 13 1 1745 347 250 1372 7850 4685 0 25 0 0 5563 0 1708 9 29842 1560 0 97 0 0 0 0 0 1567 4 0 0 3100 1437 3504 0 0 0 0 2 0 0 0 0 36094133 7578 0 0 35 26 0 0 0 0 0 0 106 0 357528 2772 2772 3860 1274 186866816 7927 171949 1 34 0 0 688 0 0 1 0
IpExt: InNoRoutes InTruncatedPkts InMcastPkts OutMcastPkts InBcastPkts OutBcastPkts InOctets OutOctets InMcastOctets OutMcastOctets InBcastOctets OutBcastOctets InCsumErrors InNoECTPkts InECT1Pkts InECT0Pkts InCEPkts ReasmOverlaps
IpExt: 0 0 0 0 53 0 1004165080598 1573518103948 0 0 4842 0 0 2759009114 5 1876 28 0
</pre>

格式说明:

TcpExt:|SyncookiesSent|SyncookiesRecv|SyncookiesFailed|EmbryonicRsts|PruneCalled|RcvPruned|OfoPruned|OutOfWindowIcmps|LockDroppedIcmps|ArpFilter|TW|TWRecycled|TWKilled|PAWSPassive|PAWSActive|PAWSEstab|DelayedACKs|DelayedACKLocked|DelayedACKLost|ListenOverflows|ListenDrops|TCPPrequeued|TCPDirectCopyFromBacklog|TCPDirectCopyFromPrequeue|TCPPrequeueDropped|TCPHPHits|TCPHPHitsToUser|TCPPureAcks|TCPHPAcks|TCPRenoRecovery|TCPSackRecovery|TCPSACKReneging|TCPFACKReorder|TCPSACKReorder|TCPRenoReorder|TCPTSReorder|TCPFullUndo|TCPPartialUndo|TCPDSACKUndo|TCPLossUndo|TCPLostRetransmit|TCPRenoFailures|TCPSackFailures|TCPLossFailures|TCPFastRetrans|TCPForwardRetrans|TCPSlowStartRetrans|TCPTimeouts|TCPLossProbes|TCPLossProbeRecovery|TCPRenoRecoveryFail|TCPSackRecoveryFail|TCPSchedulerFailed|TCPRcvCollapsed|TCPDSACKOldSent|TCPDSACKOfoSent|TCPDSACKRecv|TCPDSACKOfoRecv|TCPAbortOnData|TCPAbortOnClose|TCPAbortOnMemory|TCPAbortOnTimeout|TCPAbortOnLinger|TCPAbortFailed|TCPMemoryPressures|TCPSACKDiscard|TCPDSACKIgnoredOld|TCPDSACKIgnoredNoUndo|TCPSpuriousRTOs|TCPMD5NotFound|TCPMD5Unexpected|TCPSackShifted|TCPSackMerged|TCPSackShiftFallback|TCPBacklogDrop|PFMemallocDrop|TCPMinTTLDrop|TCPDeferAcceptDrop|IPReversePathFilter|TCPTimeWaitOverflow|TCPReqQFullDoCookies|TCPReqQFullDrop|TCPRetransFail|TCPRcvCoalesce|TCPOFOQueue|TCPOFODrop|TCPOFOMerge|TCPChallengeACK|TCPSYNChallenge|TCPFastOpenActive|TCPFastOpenActiveFail|TCPFastOpenPassive|TCPFastOpenPassiveFail|TCPFastOpenListenOverflow|TCPFastOpenCookieReqd|TCPSpuriousRtxHostQueues|BusyPollRxPackets|TCPAutoCorking|TCPFromZeroWindowAdv|TCPToZeroWindowAdv|TCPWantZeroWindowAdv|TCPSynRetrans|TCPOrigDataSent|TCPHystartTrainDetect|TCPHystartTrainCwnd|TCPHystartDelayDetect|TCPHystartDelayCwnd|TCPACKSkippedSynRecv|TCPACKSkippedPAWS|TCPACKSkippedSeq|TCPACKSkippedFinWait2|TCPACKSkippedTimeWait|TCPACKSkippedChallenge|TCPWqueueTooBig
:------------ | ------------- | ------------ |
TcpExt:|0|0|17|6|0|0|0|35|0|0|3501675|0|11192|0|0|6|1360945|7137|5563|0|0|1|23|66|0|71054242|2|116433578|43813599|0|652|0|0|0|0|0|0|0|21|1181|122|0|13|1|1745|347|250|1372|7850|4685|0|25|0|0|5563|0|1708|9|29842|1560|0|97|0|0|0|0|0|1567|4|0|0|3100|1437|3504|0|0|0|0|2|0|0|0|0|36094133|7578|0|0|35|26|0|0|0|0|0|0|106|0|357528|2772|2772|3860|1274|186866816|7927|171949|1|34|0|0|688|0|0|1|0


IpExt:|InNoRoutes|InTruncatedPkts|InMcastPkts|OutMcastPkts|InBcastPkts|OutBcastPkts|InOctets|OutOctets|InMcastOctets|OutMcastOctets|InBcastOctets|OutBcastOctets|InCsumErrors|InNoECTPkts|InECT1Pkts|InECT0Pkts|InCEPkts|ReasmOverlaps
:------------ | ------------- | ------------ |
IpExt:|0|0|0|0|53|0|1004165080598|1573518103948|0|0|4842|0|0|2759009114|5|1876|28|0

### 端口监听获取
+ 获取tcp端口监听主要是通过```ss -t -l -n```命令获取的
+ 获取udp端口监听主要是通过```ss -u -a -n```命令获取的

### 进程状态获取
主要是通过从/proc/$porcess_id/目录里获取的
<pre>
$ cat /proc/2514/status
Name:	falcon-agent
Umask:	0022
State:	S (sleeping)
Tgid:	2514
Ngid:	2514
Pid:	2514
PPid:	1
TracerPid:	0
Uid:	0	0	0	0
Gid:	0	0	0	0
FDSize:	256
Groups:
VmPeak:	 2931016 kB
VmSize:	 2931016 kB
VmLck:	       0 kB
VmPin:	       0 kB
VmHWM:	   36344 kB
VmRSS:	   36064 kB
RssAnon:	   31768 kB
RssFile:	    4296 kB
RssShmem:	       0 kB
VmData:	 2918180 kB
VmStk:	     132 kB
VmExe:	    3664 kB
VmLib:	    1972 kB
VmPTE:	     520 kB
VmSwap:	       0 kB
Threads:	75
SigQ:	0/127758
SigPnd:	0000000000000000
ShdPnd:	0000000000000000
SigBlk:	0000000000000000
SigIgn:	0000000000000003
SigCgt:	ffffffffffc1fefc
CapInh:	0000000000000000
CapPrm:	0000001fffffffff
CapEff:	0000001fffffffff
CapBnd:	0000001fffffffff
CapAmb:	0000000000000000
NoNewPrivs:	0
Seccomp:	0
Speculation_Store_Bypass:	vulnerable
Cpus_allowed:	ffff,ffffffff
Cpus_allowed_list:	0-47
Mems_allowed:	00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000003
Mems_allowed_list:	0-1
voluntary_ctxt_switches:	1013717
nonvoluntary_ctxt_switches:	14839
$
$ cat /proc/2514/cmdline
./falcon-agent-ccfg.json
$
</pre>

主要是通过这两个文件获取进程的pid，名字和cmdline，进程id获取是通过从/proc目录下遍历查找，能被```strconv.Atoi()```解析成int类型的就是进程id，然后去/proc/$pid下面去找

### snmp状态信息获取

主要是通过```/proc/net/snmp```文件
<pre>
$ cat /proc/net/snmp
Ip: Forwarding DefaultTTL InReceives InHdrErrors InAddrErrors ForwDatagrams InUnknownProtos InDiscards InDelivers OutRequests OutDiscards OutNoRoutes ReasmTimeout ReasmReqds ReasmOKs ReasmFails FragOKs FragFails FragCreates
Ip: 1 64 2513679049 0 0 1632484746 0 0 859215144 2411215015 0 9 53 53 0 53 0 57 0
Icmp: InMsgs InErrors InCsumErrors InDestUnreachs InTimeExcds InParmProbs InSrcQuenchs InRedirects InEchos InEchoReps InTimestamps InTimestampReps InAddrMasks InAddrMaskReps OutMsgs OutErrors OutDestUnreachs OutTimeExcds OutParmProbs OutSrcQuenchs OutRedirects OutEchos OutEchoReps OutTimestamps OutTimestampReps OutAddrMasks OutAddrMaskReps
Icmp: 7511 126 0 827 0 0 0 4 6677 0 3 0 0 0 15874 0 9194 0 0 0 0 0 6677 0 3 0 0
IcmpMsg: InType3 InType5 InType8 InType13 OutType0 OutType3 OutType14
IcmpMsg: 827 4 6677 3 6677 9194 3
Tcp: RtoAlgorithm RtoMin RtoMax MaxConn ActiveOpens PassiveOpens AttemptFails EstabResets CurrEstab InSegs OutSegs RetransSegs InErrs OutRsts InCsumErrors
Tcp: 1 200 120000 -1 45015289 2199069 185172 47465 103 397154082 361391064 11563 26 502027 0
Udp: InDatagrams NoPorts InErrors OutDatagrams RcvbufErrors SndbufErrors InCsumErrors
Udp: 462052801 749 1 26098 0 0 1
UdpLite: InDatagrams NoPorts InErrors OutDatagrams RcvbufErrors SndbufErrors InCsumErrors
UdpLite: 0 0 0 0 0 0 0
$
</pre>
格式说明:

Ip:|Forwarding|DefaultTTL|InReceives|InHdrErrors|InAddrErrors|ForwDatagrams|InUnknownProtos|InDiscards|InDelivers|OutRequests|OutDiscards|OutNoRoutes|ReasmTimeout|ReasmReqds|ReasmOKs|ReasmFails|FragOKs|FragFails|FragCreates
:------------ | ------------- | ------------ |
Ip:|1|64|2513679049|0|0|1632484746|0|0|859215144|2411215015|0|9|53|53|0|53|0|57|0

Icmp:|InMsgs|InErrors|InCsumErrors|InDestUnreachs|InTimeExcds|InParmProbs|InSrcQuenchs|InRedirects|InEchos|InEchoReps|InTimestamps|InTimestampReps|InAddrMasks|InAddrMaskReps|OutMsgs|OutErrors|OutDestUnreachs|OutTimeExcds|OutParmProbs|OutSrcQuenchs|OutRedirects|OutEchos|OutEchoReps|OutTimestamps|OutTimestampReps|OutAddrMasks|OutAddrMaskReps
:------------ | ------------- | ------------ |
Icmp:|7511|126|0|827|0|0|0|4|6677|0|3|0|0|0|15874|0|9194|0|0|0|0|0|6677|0|3|0|0

IcmpMsg:|InType3|InType5|InType8|InType13|OutType0|OutType3|OutType14
:------------ | ------------- | ------------ |
IcmpMsg:|827|4|6677|3|6677|9194|3

Tcp:|RtoAlgorithm|RtoMin|RtoMax|MaxConn|ActiveOpens|PassiveOpens|AttemptFails|EstabResets|CurrEstab|InSegs|OutSegs|RetransSegs|InErrs|OutRsts|InCsumErrors
:------------ | ------------- | ------------ |
Tcp:|1|200|120000|-1|45015289|2199069|185172|47465|103|397154082|361391064|11563|26|502027|0

Udp:|InDatagrams|NoPorts|InErrors|OutDatagrams|RcvbufErrors|SndbufErrors|InCsumErrors
:------------ | ------------- | ------------ |
Udp:|462052801|749|1|26098|0|0|1

UdpLite:|InDatagrams|NoPorts|InErrors|OutDatagrams|RcvbufErrors|SndbufErrors|InCsumErrors
:------------ | ------------- | ------------ |
UdpLite:|0|0|0|0|0|0|0


### 套接字状态信息获取

主要是通过: ```ss -s```实现的
```
$ ss -s
Total: 3870 (kernel 8681)
TCP:   1129 (estab 102, closed 988, orphaned 8, synrecv 0, timewait 171/0), ports 0

Transport Total     IP        IPv6
*	  8681      -         -        
RAW	  3         1         2        
UDP	  2         2         0        
TCP	  141       26        115      
INET	  146       29        117      
FRAG	  0         0         0        
$
```

### 系统运行时间获取
主要是通过```/proc/uptime```文件来获取的, 取值是第一列
```
$ cat /proc/uptime
1630846.39 34999530.34
$
```
The first value represents the total number of seconds the system has been up. The second value is the sum of how much time each core has spent idle, in seconds. Consequently, the second value may be greater than the overall system uptime on systems with multiple cores.
