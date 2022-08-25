# Fix Ubuntu Server Taking Long To Start

So everytime you reboot your server its stuck at some point during the boot. It most likely writes something like this to the output: `wait for network to be configured`.

In this quick and easy guide you'll learn how to make your ubuntu server (version does not matter) boot faster.

## Steps

1. Go to this directory by using cd: `cd /etc/netplan/`
2. `ls` to list all files. this will show you a .yml file. Mine is named `00-installer-config-wifi.yaml` but it could also be named different.
3. Open the .yml file, in my case i do it with vim: `sudo vim 00-installer-config-wifi.yaml`
4. You'll see a list of wifi adapters. add the tag `optional: true` to the wifi adapter that makes trouble and save the file.

My end result looks like this

```yml
# This is the network config written by 'subiquity'
network:
  version: 2
  wifis:
    wlo1:
      access-points:
        WIFI_NAME:
          password: WIFI_PASSWORD
      dhcp4: true
      optional: true                      
```

5. Enter `sudo netplan apply` to apply the just made changes and reboot the server.
6. Enjoy faster boot times :)
