# vgm8825
My work on the Wind Home&amp;Life Hub (Zyxel VGM8825-B50B) to debrand it and make it run OpenWrt.


## Debranding
This modem/router is shipped branded by Wind with a custom version of Zyxel Firmware, as of August 2023, the last version of the software I had installed was v5.13(ABLZ.1)b10_20200422, but it wasn't connected to the internet since mid 2021 so new custom software versions may be available.

After some research on google, I finally found these two posts (they are in italian since this modem is sold by an italian ISP):
 - [List of available pre-compiled firmwares](https://www.hwupgrade.it/forum/showthread.php?t=2871045)
 - [This one](https://www.hwupgrade.it/forum/showpost.php?p=47154099&postcount=1469) helped me to find a temporary firmware to bypass ModelID/FirmwareID check
 - [Latest unlocked/debranded firmware](https://www.hwupgrade.it/forum/showpost.php?p=47907802&postcount=2146)
The firmwares that worked for me are:
1) [Firmware 5.13(ABLZ4.C0) mod-11](https://drive.google.com/file/d/1MgLDOBA06xi9fmdz4ntkgupKU2a3HKSy/view?usp=sharing) by desigabri
2) [Firmware V5.17(ABNY.7)C0](https://mega.nz/file/Rb0RiD7D#nR2bxL_DK5zjBb204mgWSZor3Ss93tywTrYEhoDimTI) by Takuya

Since download links may become unavailable in the future, I'll upload the firmwares in the releases page.
IMPORTANT!! Always remember to check the box to restore default settings after firmware upgrade before uploading each firmware.

## OpenWrt
This is a work in progress until I manage to create and build a working image.
I'll add here any resource that I find and consider helpful for building an image.

This is a documented device with similar SoC: https://openwrt.org/toh/inteno/eg400?s[]=bcm963138

```
# uname -a
Linux VMG8825-B50B 4.1.52 #1 SMP PREEMPT Mon Aug 1 18:15:47 CEST 2022 armv7l GNU/Linux



# lsmod
Module                  Size  Used by    Tainted: P  
isofs                  21556  0 
cdrom                  24902  0 
mii                     3515  0 
uas                    11234  0 
usb_storage            39509  1 uas
bcm_usb                 1326  0 
ohci_platform           3749  0 
ohci_hcd               23781  1 ohci_platform
ehci_platform           4273  0 
ehci_hcd               33428  1 ehci_platform
xhci_plat_hcd           3588  0 
xhci_hcd               83451  1 xhci_plat_hcd
exfat                  92387  0 
nf_nat_ipsec             831  0 
nf_conntrack_ipsec      2307  1 nf_nat_ipsec
nf_conntrack_proto_esp     2920  0 
nf_nat_proto_esp         626  0 
nf_nat_pptp             1944  0 
nf_conntrack_pptp       3492  1 nf_nat_pptp
nf_conntrack_proto_gre     3058  1 nf_conntrack_pptp
nf_nat_proto_gre         913  1 nf_nat_pptp
nf_nat_rtsp             3243  0 
nf_conntrack_rtsp       7091  1 nf_nat_rtsp
nf_conntrack_netlink    18429  0 
nf_nat_sip              7203  0 
nf_conntrack_sip       18975  1 nf_nat_sip
nf_nat_tftp              655  0 
nf_conntrack_tftp       2855  1 nf_nat_tftp
nf_nat_ftp              1344  0 
nf_conntrack_ftp        5820  1 nf_nat_ftp
zyinetled               9863  0 
xt_conntrack            2546 12 
ip6t_ipv6header         1252  0 
ip6t_ah                  922  0 
ip6t_hbh                1420  0 
ip6t_REJECT             1224  0 
nf_reject_ipv6          2139  1 ip6t_REJECT
ip6t_rt                 1740  3 
ip6t_frag               1018  0 
ip6t_mh                  814  0 
ip6t_eui64               812  0 
ip6table_raw             836  0 
ip6table_mangle         1213  1 
nf_conntrack_ipv6       6523  5 
nf_defrag_ipv6         13908  1 nf_conntrack_ipv6
ip6table_filter          920  1 
ip6_tables              9712  3 ip6table_raw,ip6table_mangle,ip6table_filter
xt_SKIPLOG               577  0 
xt_helper                938  0 
xt_iprange              1206  0 
xt_TCPMSS               2738  2 
xt_mac                   689  0 
xt_limit                1383 22 
xt_state                 847  0 
xt_mark                  876  2 
xt_NFQUEUE              2111  0 
nfnetlink_queue         8369  0 
nfnetlink               4551  2 nf_conntrack_netlink,nfnetlink_queue
xt_recent               6919  4 
xt_length                831  2 
xt_pkttype               713  2 
xt_multiport            1382  4 
xt_time                 1767  0 
xt_connlimit            4357  0 
ipt_REJECT              1038  0 
nf_reject_ipv4          1858  1 ipt_REJECT
ipt_TRIGGER             1906  0 
ipt_MASQUERADE           740  0 
iptable_mangle          1094  1 
iptable_filter           977  1 
iptable_nat             1172  1 
ip_tables               9517  3 iptable_mangle,iptable_filter,iptable_nat
nf_nat_masquerade_ipv4     2979  1 ipt_MASQUERADE
nf_nat_ipv4             4071  1 iptable_nat
nf_nat                 10524 10 nf_nat_proto_esp,nf_nat_pptp,nf_nat_proto_gre,nf_nat_rtsp,nf_nat_sip,nf_nat_tftp,nf_nat_ftp,ipt_TRIGGER,nf_nat_masquerade_ipv4,nf_nat_ipv4
nf_conntrack_ipv4      11165  8 
nf_defrag_ipv4           984  1 nf_conntrack_ipv4
nf_conntrack           67282 23 nf_conntrack_ipsec,nf_conntrack_proto_esp,nf_nat_pptp,nf_conntrack_pptp,nf_conntrack_proto_gre,nf_nat_rtsp,nf_conntrack_rtsp,nf_conntrack_netlink,nf_nat_sip,nf_conntrack_sip,nf_nat_tftp,nf_conntrack_tftp,nf_nat_ftp,nf_conntrack_ftp,xt_conntrack,nf_conntrack_ipv6,xt_helper,xt_state,xt_connlimit,nf_nat_masquerade_ipv4,nf_nat_ipv4,nf_nat,nf_conntrack_ipv4
ip_gre                  8693  0 
gre                     3241  1 ip_gre
wl                   4245108  0 
dhd                   483526  0 
wlemf                  63316  2 wl,dhd
otp                     2418  0 
pwrmngtd                4259  0 
slicslac              667776  1 
dsphal                 53782  3 
wfd                    34965  2 wl,dhd
bcm_pcie_hcd           24431  0 
ahci_platform           2484  0 
libahci_platform        3944  1 ahci_platform
bcm_sata                 823  0 
ahci                   13483  0 
libahci                18688  3 ahci_platform,libahci_platform,ahci
libata                111497  4 ahci_platform,libahci_platform,ahci,libahci
bcmmcast               53314  4 dhd,wlemf,wfd
nciTMSkmod            362184  0 
exLinuxETH             13943  1 nciTMSkmod
Lservices               5564  1 nciTMSkmod
pktrunner              43378  0 
bcm_enet              171224  1 wl
bcm_tcpspdtest         70306  0 
br2684                  6970  0 
adsldd                513206  0 
bcmxtmcfg              91627  1 adsldd
bcmxtmrtdrv            40698  1 bcmxtmcfg
cmdlist                67486  1 pktrunner
pktflow               232050  1 pktrunner
rdpa_cmd               90943  0 
bcm_ingqos            214706  1 rdpa_cmd
chipinfo                1221  0 
bcm_spdsvc             19688  0 
bcm_license            36442  2 bcm_tcpspdtest,pktflow
rdpa_mw                24740  1 rdpa_cmd
rdpa_usr               34703  0 
rdpa                 1331144  3 wfd,bcm_enet,bcmxtmrtdrv
rdpa_gpl_ext             855  1 rdpa_cmd
rdpa_gpl               17614 11 dhd,wfd,pktrunner,bcm_enet,bcm_tcpspdtest,bcmxtmrtdrv,rdpa_cmd,bcm_spdsvc,rdpa_mw,rdpa_usr,rdpa
bcmvlan                97104  0 
bdmf                 1234463 12 dhd,wfd,pktrunner,bcm_enet,bcm_tcpspdtest,bcmxtmrtdrv,rdpa_cmd,bcm_spdsvc,rdpa_mw,rdpa_usr,rdpa,rdpa_gpl
bcmlibs                16276  5 wfd,pktrunner,bcm_enet,pktflow,rdpa
wlcsm                   5731 18 wl,dhd,wlemf,bcm_pcie_hcd



# df -h
Filesystem                Size      Used Available Use% Mounted on
mtd:rootfs              187.9M     57.2M    130.7M  30% /
mtd:data                  4.0M    744.0K      3.3M  18% /data
/dev/mtdblock11         128.0M      6.4M    121.6M   5% /misc
mtd:data                  4.0M    744.0K      3.3M  18% /var/home/root/data
mtd:data                  4.0M    744.0K      3.3M  18% /var/home/supervisor/data
mtd:data                  4.0M    744.0K      3.3M  18% /var/home/admin/data



# cat /proc/mtd 
dev:    size   erasesize  name
mtd0: 0bbe0000 00020000 "rootfs"
mtd1: 0bbe0000 00020000 "rootfs_update"
mtd2: 00400000 00020000 "data"
mtd3: 00100000 00020000 "romfile"
mtd4: 00100000 00020000 "rom-d"
mtd5: 00100000 00020000 "wwan"
mtd6: 00020000 00020000 "nvram"
mtd7: 0bbe0000 00020000 "image_update"
mtd8: 0bbe0000 00020000 "image"
mtd9: 20000000 00020000 "dummy1"
mtd10: 20000000 00020000 "dummy2"
mtd11: 08000000 00020000 "misc1"



# cat /proc/partitions 
major minor  #blocks  name

  31        0     192384 mtdblock0
  31        1     192384 mtdblock1
  31        2       4096 mtdblock2
  31        3       1024 mtdblock3
  31        4       1024 mtdblock4
  31        5       1024 mtdblock5
  31        6        128 mtdblock6
  31        7     192384 mtdblock7
  31        8     192384 mtdblock8
  31        9     524288 mtdblock9
  31       10     524288 mtdblock10
  31       11     131072 mtdblock11



# cat /proc/cpuinfo 
processor       : 0
model name      : ARMv7 Processor rev 1 (v7l)
BogoMIPS        : 1980.41
Features        : half thumb fastmult edsp tls 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x4
CPU part        : 0xc09
CPU revision    : 1

processor       : 1
model name      : ARMv7 Processor rev 1 (v7l)
BogoMIPS        : 1990.65
Features        : half thumb fastmult edsp tls 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x4
CPU part        : 0xc09
CPU revision    : 1

Hardware        : BCM963138
Revision        : 0000
Serial          : 0000000000000000



# cat /proc/meminfo 
MemTotal:         444372 kB
MemFree:          225984 kB
MemAvailable:     252112 kB
Buffers:               0 kB
Cached:            37744 kB
SwapCached:            0 kB
Active:            45076 kB
Inactive:          18276 kB
Active(anon):      25608 kB
Inactive(anon):        0 kB
Active(file):      19468 kB
Inactive(file):    18276 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:         25660 kB
Mapped:            13428 kB
Shmem:                 0 kB
Slab:             131368 kB
SReclaimable:       2332 kB
SUnreclaim:       129036 kB
KernelStack:        1120 kB
PageTables:         1244 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:      222184 kB
Committed_AS:     449352 kB
VmallocTotal:     499712 kB
VmallocUsed:       28328 kB
VmallocChunk:     408460 kB



# cat /proc/zoneinfo 
Node 0, zone      DMA
  pages free     402
        min      27
        low      33
        high     40
        scanned  0
        spanned  4096
        present  4096
        managed  2412
    nr_free_pages 402
    nr_alloc_batch 7
    nr_inactive_anon 0
    nr_active_anon 5
    nr_inactive_file 5
    nr_active_file 28
    nr_unevictable 0
    nr_mlock     0
    nr_anon_pages 5
    nr_mapped    5
    nr_file_pages 33
    nr_dirty     0
    nr_writeback 0
    nr_slab_reclaimable 0
    nr_slab_unreclaimable 1710
    nr_page_table_pages 0
    nr_kernel_stack 0
    nr_unstable  0
    nr_bounce    0
    nr_vmscan_write 0
    nr_vmscan_immediate_reclaim 0
    nr_writeback_temp 0
    nr_isolated_anon 0
    nr_isolated_file 0
    nr_shmem     0
    nr_dirtied   0
    nr_written   0
    nr_pages_scanned 0
    workingset_refault 0
    workingset_activate 0
    workingset_nodereclaim 0
    nr_anon_transparent_hugepages 0
    nr_free_cma  0
        protection: (0, 424, 424)
  pagesets
    cpu: 0
              count: 0
              high:  0
              batch: 1
  vm stats threshold: 4
    cpu: 1
              count: 0
              high:  0
              batch: 1
  vm stats threshold: 4
  all_unreclaimable: 0
  start_pfn:         0
  inactive_ratio:    1
Node 0, zone   Normal
  pages free     56141
        min      1252
        low      1565
        high     1878
        scanned  0
        spanned  126976
        present  109824
        managed  108681
    nr_free_pages 56141
    nr_alloc_batch 235
    nr_inactive_anon 0
    nr_active_anon 6399
    nr_inactive_file 4566
    nr_active_file 4837
    nr_unevictable 0
    nr_mlock     0
    nr_anon_pages 6412
    nr_mapped    3352
    nr_file_pages 9405
    nr_dirty     0
    nr_writeback 0
    nr_slab_reclaimable 584
    nr_slab_unreclaimable 30551
    nr_page_table_pages 311
    nr_kernel_stack 140
    nr_unstable  0
    nr_bounce    0
    nr_vmscan_write 0
    nr_vmscan_immediate_reclaim 0
    nr_writeback_temp 0
    nr_isolated_anon 0
    nr_isolated_file 0
    nr_shmem     0
    nr_dirtied   0
    nr_written   0
    nr_pages_scanned 0
    workingset_refault 0
    workingset_activate 0
    workingset_nodereclaim 0
    nr_anon_transparent_hugepages 0
    nr_free_cma  0
        protection: (0, 0, 0)
  pagesets
    cpu: 0
              count: 91
              high:  186
              batch: 31
  vm stats threshold: 12
    cpu: 1
              count: 138
              high:  186
              batch: 31
  vm stats threshold: 12
  all_unreclaimable: 0
  start_pfn:         4096
  inactive_ratio:    1



# cat /proc/cmdline 
root=mtd:rootfs rootfstype=jffs2 console=ttyS0,115200 debug irqaffinity=0 coherent_pool=1M cpuidle_sysfs_switch pci=pcie_bus_safe

```
