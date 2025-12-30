# Storage(NFS and SAN)

## NFS
1.	On NFS Server(source), run below:
a.	`yum install nfs-utils rpcbind`
b.	`vim /etc/exports` then enter `/local/path DESTINATION_IP(rw,sync,no_root_squash)`
c.	Run `exportfs -a` which will add entry in file `/var/lib/nfs/etab`
d.	`systemctl start nfs-server`
e.	`systemctl enable nfs-server`
f.	`systemctl status nfs-server`
g. You can run `exportfs -r` to reload in case some changes have been done in `/etc/exports`
3.	On NFS Client(destination), run below:
a. `yum install nfs-utils`
b. `mount -t nfs SOURCE_IP:/source/path /destination/path`
c.	To permanently mount even after reboot, edit `/etc/fstab` file and add `SOURCE_IP:/source/path /destination/path nfs defaults 0 0`
d.	`umount /destination/path` OR `umount -l /destination/path`(in case there is issue in mount point)

## SAN
1. On SAN Storage(Source), make sure storage block is setup, storage IQN(iSCSI Qualified Name) is availible and is accessable to Client(destination)
2. On Client(destination) server, run below:
a. `yum install iscsi-initiator-utils iscsi-initiator-utils-iscsiuio`
b. To check connectivity from Client server to Storage server run `iscsiadm -m discovery -t st -p SAN_STORAGE_SERVER_IP`
c. To mount stoage run `iscsiadm -m node -T IQN(iSCSI_Qualified_Name):VOLUME_NAME -p SAN_STORAGE_SERVER_IP -l`
d. Format and mount: `lsblk` to Find the new disk (e.g., /dev/sdb), `mkfs.ext4 /dev/sdb` to format in case of new storage, then make directory using `mkdir /destination/path` and run `mount /dev/sdb /destination/path`
e. To automate iSCSI login run `iscsiadm -m node -T IQN(iSCSI_Qualified_Name):VOLUME_NAME -p SAN_STORAGE_SERVER_IP --op update -n node.startup -v automatic`
f. To permanently mount even after reboot, edit `/etc/fstab` file and add `/dev/sdb /destination/path ext4  defaults  0  0`
g. `umount /destination/path` OR `umount -l /destination/path`(in case there is issue in mount point)
SAN Configuration on Dell EqualLogic PS6000
<img width="1920" height="1019" alt="1" src="https://github.com/user-attachments/assets/f5f7aa9e-d6c4-4aff-b947-c5f02e829273" />
<img width="1920" height="1013" alt="2" src="https://github.com/user-attachments/assets/b8a2c984-a69d-4883-9bc5-ddb338c3974c" />

SAN Testing on Localy created CentOS9 VM
![3](https://github.com/user-attachments/assets/316a2fce-adc7-44e5-9ca5-315905624a37)
![4](https://github.com/user-attachments/assets/070d94e6-a1c1-49bf-955d-9c0726ec55bd)
