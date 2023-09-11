# Multipass with VScode Integration
## MacOs
### Step1:
1. open your terminal on mac by pressing `Cmd+Space` and typing terminal and `Enter`.
2. type the command `brew install --cask multipass`.
3. after the install, check everything is running by running `multipass --version` which should return-  
```
multipass   1.12.2+mac
multipassd  1.12.2+mac
```
4. find a suitable instance on multipass by `multipass find`. It will return something like-
```
Image                       Aliases           Version          Description
20.04                       focal             20230811         Ubuntu 20.04 LTS
22.04                       jammy,lts         20230828         Ubuntu 22.04 LTS
23.04                       lunar             20230829         Ubuntu 23.04

Blueprint                   Aliases           Version          Description
anbox-cloud-appliance                         latest           Anbox Cloud Appliance
charm-dev                                     latest           A development and testing environment for charmers
docker                                        0.4              A Docker environment with Portainer and related tools
jellyfin                                      latest           Jellyfin is a Free Software Media System that puts you in control of managing and streaming your media.
minikube                                      latest           minikube is local Kubernetes
ros-noetic                                    0.1              A development and testing environment for ROS Noetic.
ros2-humble                                   0.1              A development and testing environment for ROS 2 Humble.
```
*(Ideally, you want to select the alias of the 22.04 LTS instance. Here, it is named '**jammy**')*  

5. Make and launch an Instance on Multipass by running `multipass launch jammy --name rayman`. (note that --name is optional if you want a custom name)
Now, after loading for a bit, you should see on your terminal-`Launched: rayman`
6. Now, before moving further, you will want to open an extra terminal. You may do this by holding on to the terminal from the dock and selecting 'new window'.(You may as well select shell from the top menu and click on Shell **->** New Window **->** New Window with Profile-Basic).
7. On your old window, enter the instance by typing `multipass shell rayman`. You should see something like this on your window-
```
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-82-generic aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Sep  3 09:31:02 PDT 2023

  System load:             0.001953125
  Usage of /:              29.8% of 4.68GB
  Memory usage:            17%
  Swap usage:              0%
  Processes:               88
  Users logged in:         0
  IPv4 address for enp0s1: 192.168.64.22
  IPv6 address for enp0s1: fdd3:1d24:7c3e:9c48:5054:ff:fe38:2fd1


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@rayman:~$ 
```
Now, you have successfully setup a virtual Linux environment on your Mac but note that there is no 'GUI'. (That should be fine as long as you know the basic terminal commands.) The next step will we to setup up SSH(secure shell login) in order to integrate this machine with our VScode editor. 

8. go to your new terminal window (the one you opened in step6) and type `ssh-keygen` which will generate for you an SSH key. There are a few prompts that will come up and lets look at what to enter in them sequentially-  
    * `Generating public/private rsa key pair.
Enter file in which to save the key (/Users/aryaman_bahuguna/.ssh/id_rsa):$input$ `, just enter any name if you have done this kind of step before or click enter if this is the first time you're doing this.
    * `Enter passphrase (empty for no passphrase): `, enter a password you'd like to enter when logging into your linux VM through SSH. After confirming, the output should look something like-
    ```
    The key fingerprint is:
    SHA256:gsqyJRwqgzYy201MzY4IvJEChjZ24LF/PFIXz/Z65p4 aryaman_bahuguna@Aryamans-Mac.local
    The key's randomart image is:
    +---[RSA 3072]----+
    | o     .         |
    |o +     +        |
    |oB . . . +       |
    |* = o+. . .      |
    |o= oo++ S  .     |
    |=o+=oo..  .      |
    |X== + .  . o     |
    |+X.o      + .    |
    |o . .     .E     |
    +----[SHA256]-----+
    ```
9. Now, we want to copy the public key we just generated. The process is pretty straightforward.From Finder,first unhide the hidden files by using the combination Cmd+Shift+P. Then, Navigate to 'Users/Name/.ssh/' where you should find your stored keys under 'id_rsa.pub'. To open this file, hold 'Option' on your keyboard and right-click on the .ssh folder that appears at the bottom and select- 'Open in Terminal'.
10. You should have now opened a new terminal and now, just type- `cat id_rsa.pub` to view the file. The output should look something like this-
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCqfS4FxheVYRkkMJUwfAhawN9pa3TQcJLK/VCRKIpwhwDuDvnkEJNWhv1siFPTBzfUyU3EBp692pQpyQ9mfq3g7LYsExv4qxlm4zEwwxRGpdEXKWDrTI8n0drKzSQRMG9P2QsCYdUYLNLQNJOXI5kU+R7pFTHgGV349sU0bcenXX5IpRnvLctWTJh+YZCIaOMiKV8T+bvCqMs8jzWezJKeOdeeuzPPpP4fo5F/lXwTcQ4mcNnK9IJStSlvStkAyQe2rl4BDI+ab3r6Ho8uSQB8beIbhW4Ec0aD8TzuRyy5LLWKPQLXmv4+xx/bkO2uq43FaGKioANa8xe09fOtJB65X6A6QLQT6cn3B9swJY/mbINZ9qebivOQFxmgfeR7BaioY6rvKTXyQVxunZcIV7IjLdQv9vSqf6QeM7TOxFUSxjYj9w/E15F//ZqFV3EdzpIhPcInIbeCb6cdHcdntiWe8JlAyUipQB3QTWippzx/ZwFm2h541uteL0ZaSESntpM= aryaman_bahuguna@Aryamans-Mac.local
```
(copy the contents of this file by selecting all the output and pressing Cmd+C.)
 
11. Now, head back to your VM shell and navigate to thee ssh folder by typing `cd .ssh` and then type `vim autorized_keys`.
12. Once the file is opened, head to the end of the file by typing `$` in the vim command which will move your cursor to the end character of the file. Type `i` to get into the insert move and click Enter two times and finally, paste in your copied SSH key from Step10.
13. click ESC key on your keyboard and enter `:w` and enter and finally, ESC again and `:q` enter. This will save the file.
14. restart your instance by first exiting from it (just type exit). Then, type- `multipass restart rayman`.
15. Note that you can now login into your VM (after first starting it using `multipass start rayman`) using `ssh ubuntu@IPv4_address` which can be found using `multipass info rayman`.
16. Now, ensure that vscode is installed on your mac. go to the extensions on your vscode editor and install **Remote SSH**. Now, a symbol like such- '><' will be displayed on the bottom left corner of your editor screen. Click on it and select Connect to Host and enter `ubuntu@IPv4_address` and hit Enter. (enter password as required)
17. If you followed everything correctly, your connection to your linux machine should be successfull. Now, you may create folders on your machine(search that on google) and you're good to go! 

Cheers!! You're Set!
