```sh
daniel@daniel-HP-ENVY-4-Notebook-PC:/media/daniel/f2100b2f-ed84-4647-b5ae-089280112716/var/log$ tail kern.log
Jul 29 06:17:52 raspberrypi kernel: [   71.167734] cfg80211:  DFS Master region: FCC
Jul 29 06:17:52 raspberrypi kernel: [   71.167747] cfg80211:   (start_freq - end_freq @ bandwidth), (max_antenna_gain, max_eirp), (dfs_cac_time)
Jul 29 06:17:52 raspberrypi kernel: [   71.167762] cfg80211:   (2402000 KHz - 2482000 KHz @ 40000 KHz), (N/A, 2000 mBm), (N/A)
Jul 29 06:17:52 raspberrypi kernel: [   71.167777] cfg80211:   (5170000 KHz - 5250000 KHz @ 80000 KHz, 160000 KHz AUTO), (N/A, 2300 mBm), (N/A)
Jul 29 06:17:52 raspberrypi kernel: [   71.167789] cfg80211:   (5250000 KHz - 5330000 KHz @ 80000 KHz, 160000 KHz AUTO), (N/A, 2300 mBm), (0 s)
Jul 29 06:17:52 raspberrypi kernel: [   71.167800] cfg80211:   (5735000 KHz - 5835000 KHz @ 80000 KHz), (N/A, 3000 mBm), (N/A)
Jul 29 06:17:52 raspberrypi kernel: [   71.167812] cfg80211:   (57240000 KHz - 59400000 KHz @ 2160000 KHz), (N/A, 2800 mBm), (N/A)
Jul 29 06:17:52 raspberrypi kernel: [   71.167822] cfg80211:   (59400000 KHz - 63720000 KHz @ 2160000 KHz), (N/A, 4400 mBm), (N/A)
Jul 29 06:17:52 raspberrypi kernel: [   71.167833] cfg80211:   (63720000 KHz - 65880000 KHz @ 2160000 KHz), (N/A, 2800 mBm), (N/A)
Aug  7 16:59:10 raspberrypi kernel: [ 3084.461800] EXT4-fs error (device mmcblk0p2): ext4_find_dest_de:1808: inode #122732: block 533853: comm motion: bad entry in directory: inode out of bounds - offset=136(136), inode=268607985, rec_len=32, name_len=24
```

```sh
daniel@daniel-HP-ENVY-4-Notebook-PC:/media/daniel/f2100b2f-ed84-4647-b5ae-089280112716/var/log$ sudo lsblk -l
[sudo] password for daniel: 
NAME              MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sdb                 8:16   0  29,8G  0 disk 
sdb1                8:17   0     6G  0 part 
sda                 8:0    0 596,2G  0 disk 
sda2                8:2    0   488M  0 part /boot
sda3                8:3    0 595,2G  0 part 
ubuntu--vg-swap_1 253:1    0   5,9G  0 lvm  [SWAP]
ubuntu--vg-root   253:0    0 589,3G  0 lvm  /
sda1                8:1    0   512M  0 part /boot/efi
mmcblk0           179:0    0  29,3G  0 disk 
mmcblk0p2         179:2    0  29,3G  0 part /media/daniel/f2100b2f-ed84-4647-b5ae-089280112716
mmcblk0p1         179:1    0    41M  0 part /media/daniel/boot
```


```sh
sudo fsck -fy /dev/mmcblk0p2
```

