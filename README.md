# Parallels Tools fix for Kali linux 2019.3 and previous version
# [한국어 버전](https://github.com/scadasystems/Parralles-tools-fix-kali-linux/blob/master/README-kor.md)

* It can be applied to almost all operating systems as well as Kali linux.
* Try this if you get an error from your Parallels Tools installation.
* Apply OS - BackBox, Kali linux, AnonymousOS, Ubuntu, Debian etc
* If you get an error, please write issues.

## Parallels tools Setup
First update Kali Linux environment to the latest version. Copy and execute the following commands one by one:
``` bash
apt-get clean
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
```
Restart Kali Linux and install Parallel Tools. 
## Parallels tools Patch
1. mkdir parallels_fixed in `/root/Desktop/`
2. If you have a tool installed, there are files. Copy these files into `parallels_fixed` directory.
3. Go to the kmods directory (`cd ~/parallels_fixed/kmods`)
4. extract the files (`tar -xzf prl_mod.tar.gz`), and remove prl_mod.tar.gz file from that directory (`rm prl_mod.tar.gz`)
5. Go to the prl_fs directory : `cd /root/Desktop/parallels_fixed/kmods/prl_fs/SharedFolders/Guest/Linux/prl_fs`
6. Open `prlfs.h` and modify the file by going to line 16 and inserting a new line. Add this text : `#include <uapi/linux/mount.h>`
    The file should now look like this. Save and exit.

    ``` c
    ...
    #include <linux/fs.h>
    #include <uapi/linux/mount.h>   
    #include <linux/types.h>
    ...
    ``` 

7. Go to the *kmods* directory (`cd ~/parallels_fixed/kmods`) and re-zip the files : `tar -zcvf prl_mod.tar.gz . dkms.conf Makefile.kmods`
8. Go to the installer directory : `cd ~/parallels_fixed/installer`
9. Sudo chmod the script files: **install-cli.sh** (and others) to be executable eg. `sudo chmod 777 *.sh`
10. Then run that file with : `sudo ./install-cli.sh -i --verbose`
11. Reboot when it's finished and check working well.
# End