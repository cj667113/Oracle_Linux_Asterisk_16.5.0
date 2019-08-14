# Oracle_Linux_Asterisk_16.5.0
Install and configure Asterisk 16.5.0 on Oracle Linux


Install Asterisk 16 on Oracle Linux:

sudo yum - y update

sudo yum groupinstall core base "Development Tools”

sudo nano /etc/selinux/config
	###set SELINUX=disable###

sudo reboot

getenforce

sudo yum install -y epel-release dmidecode gcc-c++ ncurses-devel libxml2-devel make wget openssl-devel newt-devel kernel-devel sqlite-devel libuuid-devel gtk2-devel jansson-devel binutils-devel

adduser asterisk -c "Asterisk User”

mkdir ~/build && cd ~/build

wget https://www.pjsip.org/release/2.8/pjproject-2.8.tar.bz2

tar xvjf pjproject-2.8.tar.bz2
cd pjproject-2.8

./configure CFLAGS="-DNDEBUG -DPJ_HAS_IPV6=1" --prefix=/usr --libdir=/usr/lib64 --enable-shared --disable-video --disable-sound --disable-opencore-amr

make dep

make && sudo make install && sudo ldconfig

ldconfig -p | grep pj

cd ~/build

wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-16-current.tar.gz

tar -zxvf asterisk-16-current.tar.gz

cd asterisk-16.5.0

sudo yum install svn

./contrib/scripts/get_mp3_source.sh

sudo contrib/scripts/install_prereq install

./configure --libdir=/usr/lib64 --with-jansson-bundled

sudo yum install libedit-devel

./configure --libdir=/usr/lib64 --with-jansson-bundled

make menuselect
###Enable format_mp3###

make && sudo make install

sudo make samples

sudo make config

sudo chown asterisk. /var/run/asterisk

sudo chown asterisk. -R /etc/asterisk

sudo chown asterisk. -R /var/{lib,log,spool}/asterisk

sudo service asterisk start

sudo cat /etc/asterisk/sip.conf
###Modify Firewalls for udp port 5060 on Local Machine and on VCNs###

sudo firewall-cmd --permanent --zone=public --add-port=5060/udp

sudo firewall-cmd —reload

sudo firewall-cmd --permanent --zone=public --list-port

sudo reboot
