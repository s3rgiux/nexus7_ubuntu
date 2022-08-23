# nexus7_ubuntu
How installed ubuntu on Nexus 7 and achieved to used to my robot on mercari


## Installation

Basically I sarted with my nexus 7 already Rooted and insatalled teh recovery

Used the UBPorts installer to install Ubuntu wich requieres developer mode and debugging mode activated

> https://devices.ubuntu-touch.io/installer/


## Enlarged the Filesystem

First times i was running out of space so i tried the next commands to enlarge the Nexus 7 Root filesystem space

>  https://askubuntu.com/questions/514913/how-to-get-a-larger-root-partition-on-touch

```
sudo -s
dd if=/dev/null of=/userdata/ubuntu.img bs=1M seek=6000 count=0
resize2fs -f /userdata/ubuntu.img
reboot
```


## Config SSH

We need to enable ssh if we want to use it, but  first we need to add the autorized keys 
 
> https://docs.ubports.com/en/latest/userguide/advanceduse/ssh.html

Generate the ssh key on the PC

```
ssh-keygen
```

we can pass the key ot the Nexus using adb 

```
adb push ~/.ssh/id_rsa.pub /home/phablet/
```

then modify the permissions

```
mkdir /home/phablet/.ssh
chmod 700 /home/phablet/.ssh
cat /home/phablet/id_rsa.pub >> /home/phablet/.ssh/authorized_keys
chmod 600 /home/phablet/.ssh/authorized_keys
chown -R phablet:phablet /home/phablet/.ssh
```
At the end is enabled by 

```
sudo android-gadget-service enable ssh
```

then we can connect as normal ssh

```
ssh phablet@<ip-address>
```



## Use APT natively

At the begiinig can`t use Apt, since Touch is a OTA distribution the rootfilesystem is read only, we can made ir readable by using the next instruction 

> https://forums.ubports.com/topic/1055/mounting-the-root-fs-writable/3


```
sudo mount -o rw,remount /
```

## Python3.9

Tried to install a newwer verison of Python, the only one that i found working was 3.9, its needed since mercari instalations was causing some syntax errors, and also is nice to have pip


> https://linuxize.com/post/how-to-install-python-3-9-on-ubuntu-20-04/

Since now we can use apt we can do

```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.9
```

Tried to install lower versions but dindn`t work for me

I think after getting python 3.9 you should have pip, if not, go for pip. Should be something like that

```
sudo apt install python3-pip
```

## Install mercari

This was tricky because normally just requieres to use 'pip install mercari' but i had troubles with dependecies wich easily changed trying to install form source

Installed cryptography and setuptools, updated pip

```
python3.9 -m pip install setuptools
python3.9 -m pip install cryptography==3.1.1
python3.9 -m pip install mercari
```

Used nano to modify the cryptography requirements and then install

```
nano requirements.txt
python3.9 setup.py 
```

Based on the repo wich already forked

>  https://github.com/marvinody/mercari/

cloned the repo and change the requirements 



## History

Added the historyjust in case

1  ifconfig
    2  sudo android-gadget-service enable ssh
    3  mkdir .ssh
    4  chmod 700 .ssh
    5  cat id_rsa.pub >> .ssh/authorized_keys
    6  chmod 600 .ssh/authorized_keys
    7  chown -R phablet:phablet .ssh
    8  sudo android-gadget-service enable ssh
    9  ls .ssh
   10  ls .ssh/authorized_keys 
   11  cat .ssh/authorized_keys 
   12  reboot
   13  sudo reboot
   14  dd if=/dev/null of=/userdata/ubuntu.img bs=1M seek=6000 count=0
   15  resize2fs -f /userdata/ubuntu.img
   16  reboot
   17  ssh-keygen
   18  sudo android-gadget-service enable ssh
   19  sudo -s
   20  sudo android-gadget-service enable ssh
   21  ifconfog
   22  ifconfig
   23  ifconfog
   24  ifconfig
   25  python3.9 pip install pyserial
   26  python3.9 -m pip install pyserial
   27  python3.9 -m pip install pymavlink
   28  python3.9 -m pip install setuptools
   29  pip install cryptography==3.1.1
   30  python3.9 -m pip install cryptography==3.1.1
   31  python3.9 -m pip install mercari
   32  git clone https://github.com/marvinody/mercari.git
   33  cd mercari/
   34  cp requirements.txt requirements.txt.bkup
   35  nano requirements.txt
   36  ls
   37  python3.9 setup.py 
   38  python3.9 setup.py --help
   39  python3.9 setup.py build
   40  python3.9 setup.py install
   41  sudo python3.9 setup.py install
   42  python3.9
   43  python3.9 -m pip install pymavlink
   44  sudo apt search pip



















