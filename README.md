# openwrt
```
yum install epel-release -y
yum -y update

yum install bash-completion bzip2 gcc gcc-c++ git \
make ncurses-devel patch perl-Data-Dumper perl-Thread-Queue python2 \
python3 rsync tar unzip wget perl-base perl-File-Compare \
perl-File-Copy perl-FindBin diffutils which -y

yum install subversion binutils bzip2 gcc gcc-c++ gawk gettext flex \
ncurses-devel zlib-devel make patch unzip perl-ExtUtils-MakeMaker glibc \
glibc-devel glibc-static quilt ncurses-lib sed sdcc intltool \
sharutils bison wget git-core openssl-devel xz -y

useradd test
su test
cd

git clone https://git.openwrt.org/openwrt/openwrt.git
cd openwrt
git pull
git branch -a
git tag
git checkout v21.02.1
git clone -b master --single-branch https://github.com/1121170088/openwrt-fullconenat.git package/fullconenat
git clone -b master --single-branch https://github.com/trojan-gfw/openwrt-trojan.git
mv openwrt-trojan/openssl1.1 openwrt-trojan/trojan package
./scripts/feeds update -a
./scripts/feeds install -a
wget https://raw.githubusercontent.com/1121170088/openwrt/main/.config
wget https://raw.githubusercontent.com/coolsnowwolf/lede/master/target/linux/generic/hack-5.4/952-net-conntrack-events-support-multiple-registrant.patch -O target/linux/generic/hack-5.4/952-net-conntrack-events-support-multiple-registrant.patch
#export FORCE_UNSAFE_CONFIGURE=1
make -j $(nproc) V=99

disable default zone_wan ip MASQUERADE

tee>/etc/rc.local<<-"EOF"
# Put your custom commands here that should be executed once
# the system init finished. By default this file does nothing.

sleep 10
iptables -t nat -A zone_wan_prerouting -j FULLCONENAT
iptables -t nat -A zone_wan_postrouting -j FULLCONENAT

exit 0
EOF

reboot
```
