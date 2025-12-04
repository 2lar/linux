### Adding support ( or really read / write ) for iOS

Have to add this configuation into `home/{user}/etc/samba/smb.conf`
at the bottom of `[global]`

```
vfs objects = fruit streams_xattr  
fruit:metadata = stream
fruit:model = MacSamba
fruit:veto_appledouble = no
fruit:nfs_aces = no
fruit:wipe_intentionally_left_blank_rfork = yes 
fruit:delete_empty_adfiles = yes 
fruit:posix_rename = yes 
```
[reference](https://wiki.samba.org/index.php/Configure_Samba_to_Work_Better_with_Mac_OS_X)

