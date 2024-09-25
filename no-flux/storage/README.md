# Openebs

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
kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/ssd1 /docker-entrypoint.sh'
kubectl exec -it dbench -- sh -c 'DBENCH_MOUNTPOINT=/volume/sda /docker-entrypoint.sh'
```
