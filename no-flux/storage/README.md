# Openebs

scsi0   / lvm-thin  / sda   / OS disk
scsi1   / lvm-thin  / sdb   / local storage
scsi2   / lvm-thin  / sdc   / replicated storage 
scsi3   / lvm-thin  / sdd   / replicated storage 
scsi4   / ssd1      / sde   / local storage
scsi5   / ssd1      / sdf   / replicated storage 
scsi6   / ssd1      / sdg   / replicated storage 
scsi7   / ssd2      / sdh   / local storage
scsi8   / ssd2      / sdi   / replicated storage 
scsi9   / ssd2      / sdj   / replicated storage 
scsi10  / ceph      / sdk   / local storage

10.60.128.193   /dev/sdb1    10.67      0.11       10.56           1.01%          /var/storage/local-thin
10.60.128.193   /dev/sde1    10.67      0.11       10.56           1.01%          /var/storage/ssd1
10.60.128.193   /dev/sdh1    10.67      0.11       10.56           1.01%          /var/storage/ssd2
10.60.128.193   /dev/sdk1    10.67      0.11       10.56           1.01%          /var/storage/ceph

```sh
talosctl disks -n 10.60.128.193
talosctl mount -n 10.60.128.193 | grep "/dev/sd"

# available block devices
kubectl mayastor get block-devices cp-0 -n openebs

kubectl mayastor get volumes -n openebs
```

## Benchmark

```sh
kubectl exec -it fio -- fio --name=benchtest --size=2g --filename=/volume/sda/test --direct=1 --rw=randrw --ioengine=libaio --bs=4k --iodepth=16 --numjobs=1 --time_based --runtime=60
kubectl exec -it fio -- fio --name=benchtest --size=2g --filename=/volume/ssd1/test --direct=1 --rw=randrw --ioengine=libaio --bs=4k --iodepth=16 --numjobs=1 --time_based --runtime=60
kubectl exec -it fio -- fio --name=benchtest --size=2g --filename=/volume/ceph/test --direct=1 --rw=randrw --ioengine=libaio --bs=4k --iodepth=16 --numjobs=11 --time_based --runtime=60
kubectl exec -it fio -- fio --name=benchtest --size=2g --filename=/volume/rep-1/test --direct=1 --rw=randrw --ioengine=libaio --bs=4k --iodepth=16 --numjobs=11 --time_based --runtime=60
kubectl exec -it fio -- fio --name=benchtest --size=2g --filename=/volume/rep-3/test --direct=1 --rw=randrw --ioengine=libaio --bs=4k --iodepth=16 --numjobs=11 --time_based --runtime=60
```

```sh
kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/local-thin /docker-entrypoint.sh'
kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/ssd1 /docker-entrypoint.sh'
kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/ssd2 /docker-entrypoint.sh'

kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/rep-1-slow /docker-entrypoint.sh'
kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/rep-1-fast/docker-entrypoint.sh'

kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/rep-3-slow /docker-entrypoint.sh'
kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/rep-3-fast /docker-entrypoint.sh'
```
