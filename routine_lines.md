```
GPU:
$ sudo lshw -numeric -class video
$ watch -n 10 nvidia-detector
$ nvadia-detector
$ lspci | grep -i nvidia
$ sudo dpkg --list | grep nvidia-*
$ lspci |grep VGA

cuda:
$ cat /usr/local/cuda/version.txt
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2


/etc/apt/source.list:
$ sudo add-apt-repository --remove ppa:jonathonf/texlive




//check files
df -h
//check files space
du -sh *


dpkg:
dpkg -s # check installed package info
dpkg -S <filename> # which package the file belongs to
dpkg -c <deb name> # what's inside the package
dpkg -l | grep <app name>
dpkg -r <...>

$ apt-cache show <package> # show the info of the package, no matter the package is installed or not.


$ cat /etc/os-release

/etc/default/grub
/boot/grub/grub.cfg
/etc/grub/...

# grep basic and extend regular expressions:
https://blog.csdn.net/yjk13703623757/article/details/80410351

# be aware of the difference between RE and Wildcard characters

# to show how many files
$ ll | grep '^-' | wc -l
# tar gzip rar
$ tar -xvf $file -C $dir  # .tar
$ tar -xzvf $file -C $dir # .tar.gz
$ gzip -dv *
$ unrar x $file $dir
$ tar -jxvf xx.tar.bz2 # .tar.bz2

# num of cpus
grep 'physical id' /proc/cpuinfo | sort -u
# num of cores
grep 'core id' /proc/cpuinfo | sort -u | wc -l
# num of processes
grep 'processor' /proc/cpuinfo | sort -u | wc -l

refresh nvidia-smi:
$ watch -n 1 -d nvidia-smi

# check public ip
$ curl www.icanhazip.com

# ngrok
$ ngrok tcp 22 # start on server
$ ssh -p 14080 woody@0.tcp.ngrok.io #on client

# to reinstall packages in virtualenv:
$ pip freeze > requirements.txt
$ pip install -r requirements.txt

$ docker run -it --runtime=nvidia -v /home/woody/workspace/:/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-gpu-py3-jupyter

# to save and create a new image in docker:
$ docker commit <current_container> <new_image_name>
$ <start a new container with the new_image>
$ docker container stop <current_container>
(option)$ docker image rm <old_saved_image>

# remove all containers
$ docker rm $(docker ps -aq)
$ docker commit brats19 brats19
# we'll use 8887 for docker's jupyter-notebook, 8888 would be left for the host's jupyter-notebook
$ docker run --name brats19 -it --rm --runtime=nvidia -p 8887:8888 -v /media/woody/Elements/brats2019/:/mnt brats19 

$ virtualenv -p /usr/bin/python3.5 --system-site-packages vp3.5

$ pip freeze > requirements.txt
$ pip install -r requirements.txt

# github go back:
$ git log
$ git reset --hard <>
$ git push -f -u origin master
```

