# STOR_CORE_015

> STOR_CORE_015 The objective of this test is to ensure that host delete and reprovision of nodes running ceph-mon works properly on all supported configs.

- Results
  - Virtual
  - Bare Metal
- Issues

## Virtual

1. Lock one of the nodes that are part of a ceph-system. e.g. controller-0 on an All-in-One Duplex system, controller-0 on a standard system, or storage-0 on a ceph storage system.
Delete the node
2. Verify the appropriate alarms and events are seen.
3. Verify the ceph status is updated as expected.
4. Re-provision the deleted node
5. Once the node is available, ensure that ceph recovers.
6. Ensure the weights look accurate in
7. Ensure there are no unexpected alarms or events
8. Perform basic actions to ensure the system is working properly, e.g. create some volumes, import some images, launch VMs from volume.
9. Repeat test for the other system configuration types

## Bare Metal

1. Lock one of the nodes that are part of a ceph-system. e.g. controller-0 on an All-in-One Duplex system, controller-0 on a standard system, or storage-0 on a ceph storage system.
Delete the node
2. Verify the appropriate alarms and events are seen.
3. Verify the ceph status is updated as expected.
4. Re-provision the deleted node
5. Once the node is available, ensure that ceph recovers.
6. Ensure the weights look accurate in
7. Ensure there are no unexpected alarms or events
8. Perform basic actions to ensure the system is working properly, e.g. create some volumes, import some images, launch VMs from volume.
9. Repeat test for the other system configuration types
