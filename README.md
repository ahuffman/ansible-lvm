# ahuffman.lvm
Configures Logical Volume Groups, Logical Volumes, Filesystems, mount points, and fstab.

## Variables
|Variable Name|Description|Required|Default Value|Type|
|---|---|:---:|:---:|:---:|
|lvm_vgs|Defines logical volume groups|yes|[{}]|list of dictionaries.|
|lvm_lvs|Defines all aspects of logical volumes, including the volume's filesystem, ownership/permissions, and mountpoint in fstab.| yes|[{}]|list of dictionaries|

### lvm_vgs Parameters and Usage
`lvm_vgs`: Hash to define several Logical Volume Groups  
&nbsp;&nbsp;&nbsp;&nbsp;- `name`: volume group1: Arbitrary name of a Logical Volume Group  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`vg`: Logical Volume Group name to create  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`pvs`: List of physical volumes to construct the Logical Volume Group with  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- /dev/sdb  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- /dev/sdc  

### lvm_lvs Parameters and Usage
`lvm_lvs`: Hash to define several Logical Volumes   
&nbsp;&nbsp;&nbsp;&nbsp;- `name`: volume1: Arbitrary name of a Logical Volume   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`vg`: Volume Group to create the Logical Volume in   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`lv`: Name of the Logical Volume to create   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`size`: Size of the Logical Volume to create   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`mount`: Where you would like the Logical Volume mounted   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`mount_owner`: Owner of the mount point  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`mount_group`: Group ownership of the mount point  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`mount_mode`: Permissions of the mount point  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`mount_dump`: Whether or not the filesystem should be dumped (5th column of /etc/fstab) `man fstab`  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`mount_passno`: Filesystem check pass number (6th column of /etc/fstab) `man fstab`  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`mount_opts`: Comma separated list of mount options for the Logical Volume, such as defaults   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`fstype`: Type of filesystem to create on the Logical Volume   


## Example Playbook
```yaml
- name: "Configure standard disk layout"
  hosts: "servers"
  roles:
    - role: "ahuffman.lvm"
      lvm_vgs:
        - name: "vg1"
          vg: "vg_myvg1"
          pvs:
            - "/dev/sdb"
            - "/dev/sdc"
        - name: "vg2"
          vg: "vg_myvg2"
          pvs:
            - "/dev/sdd"
      lvm_lvs:
        - name: "Data Volume"
          vg: "vg_myvg1"
          lv: "lv_data"
          size: "25g"
          mount: "/data/mydata"
          mount_owner: "root"
          mount_group: "root"
          mount_mode: "0755"
          mount_dump: "1"
          mount_passno: "2"
          mount_opts: "defaults"
          fstype: "xfs"
        - name: "Web Content"
          vg: "vg_myvg2"
          lv: "lv_www"
          size: "20g"
          mount: "/data/www"
          mount_owner: "root"
          mount_group: "root"
          mount_mode: "0755"
          mount_dump: "1"
          mount_passno: "2"
          mount_opts: "defaults"
          fstype: "xfs"
        - name: "Temp space"
          vg: "vg_myvg1"
          lv: "lv_temp"
          size: "35g"
          mount: "/temp"
          mount_owner: "root"
          mount_group: "root"
          mount_mode: "0755"
          mount_dump: "1"
          mount_passno: "2"
          mount_opts: "defaults"
          fstype: "xfs"
```

## License
[MIT](LICENSE)

## Author Information
[Andrew Huffman](https://github.com/ahuffman)
