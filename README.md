# Занятие 1. 
# Vagrant-стенд для обновления ядра в ОС Linux

## Цель домашнего задания
Научиться обновлять ядро в ОС Linux. Получение навыков работы с Vagrant. 

## Описание домашнего задания
1) Запустить ВМ с помощью Vagrant.
2) Обновить ядро ОС из репозитория ELRepo.
3) Оформить отчет в README-файле в GitHub-репозитории.

 
## Выполнение задания

#### Создание директорий для выполнения ДЗ: 

```
PS C:\Users\user> mkdir Otus && cd Otus
PS C:\Users\user\Otus> mkdir task-1 && cd task-1
```

### Создание и запуск виртуальной машины:
#### Создание Vagrantfile:

```
PS C:\Users\user\Otus\task-1> vagrant init generic/centos8s

A `Vagrantfile` has been placed in this directory. You are now

ready to `vagrant up` your first virtual environment! Please read

the comments in the Vagrantfile as well as documentation on

`vagrantup.com` for more information on using Vagrant.
```

#### Редактирование Vagrantfile:
>(задал параметры, указанные в мет. пособии, кроме :box_version - изменил на 4.3.2, т.к. с 4.3.4 не запускалась):

```
MACHINES = {
  :"kernel-update" => {
              :box_name => "generic/centos8s",
              :box_version => "4.3.2"
              :cpus => 2,
              :memory => 1024,
            }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s
      box.vm.provider "virtualbox" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
      end
    end
  end
end
```

#### Запуск ВМ с помощью Vagrant:

```
PS C:\Users\user\Otus\task-1> vagrant up
Bringing machine 'kernel-update' up with 'virtualbox' provider...
==> kernel-update: Importing base box 'generic/centos8s'...
==> kernel-update: Matching MAC address for NAT networking...
==> kernel-update: Checking if box 'generic/centos8s' version '4.3.2' is up to date...
==> kernel-update: Setting the name of the VM: task-1_kernel-update_1706001575484_47278
==> kernel-update: Clearing any previously set network interfaces...
==> kernel-update: Preparing network interfaces based on configuration...
    kernel-update: Adapter 1: nat
==> kernel-update: Forwarding ports...
    kernel-update: 22 (guest) => 2222 (host) (adapter 1)
==> kernel-update: Running 'pre-boot' VM customizations...
==> kernel-update: Booting VM...
==> kernel-update: Waiting for machine to boot. This may take a few minutes...
    kernel-update: SSH address: 127.0.0.1:2222
    kernel-update: SSH username: vagrant
    kernel-update: SSH auth method: private key
    kernel-update:
    kernel-update: Vagrant insecure key detected. Vagrant will automatically replace
    kernel-update: this with a newly generated keypair for better security.
    kernel-update:
    kernel-update: Inserting generated public key within guest...
    kernel-update: Removing insecure key from the guest if it's present...
    kernel-update: Key inserted! Disconnecting and reconnecting using new SSH key...
==> kernel-update: Machine booted and ready!
[kernel-update] GuestAdditions versions on your host (7.0.4) and guest (6.1.30) do not match.
Last metadata expiration check: 0:01:37 ago on Tue 23 Jan 2024 09:22:29 AM UTC.
Package kernel-devel-4.18.0-513.el8.x86_64 is already installed.
Package kernel-devel-4.18.0-513.el8.x86_64 is already installed.
Package gcc-8.5.0-20.el8.x86_64 is already installed.
Package binutils-2.30-123.el8.x86_64 is already installed.
Package make-1:4.2.1-11.el8.x86_64 is already installed.
Package perl-interpreter-4:5.26.3-422.el8.x86_64 is already installed.
Package bzip2-1.0.6-26.el8.x86_64 is already installed.
Package elfutils-libelf-devel-0.189-3.el8.x86_64 is already installed.
Dependencies resolved.
================================================================================
 Package                       Arch      Version             Repository    Size
================================================================================
Installing:
 kernel-devel                  x86_64    4.18.0-536.el8      baseos        28 M
Upgrading:
 cpp                           x86_64    8.5.0-21.el8        baseos        10 M
 elfutils-debuginfod-client    x86_64    0.190-2.el8         baseos        77 k
 elfutils-libelf               x86_64    0.190-2.el8         baseos       233 k
 elfutils-libelf-devel         x86_64    0.190-2.el8         baseos        62 k
 elfutils-libs                 x86_64    0.190-2.el8         baseos       305 k
 gcc                           x86_64    8.5.0-21.el8        baseos        23 M
 gcc-c++                       x86_64    8.5.0-21.el8        appstream     12 M
 libgcc                        x86_64    8.5.0-21.el8        baseos        82 k
 libgomp                       x86_64    8.5.0-21.el8        baseos       208 k
 libstdc++                     x86_64    8.5.0-21.el8        baseos       458 k
 libstdc++-devel               x86_64    8.5.0-21.el8        appstream    2.2 M

Transaction Summary
================================================================================
Install   1 Package
Upgrade  11 Packages

Total download size: 78 M
Downloading Packages:
(1/12): libstdc++-devel-8.5.0-21.el8.x86_64.rpm 2.1 MB/s | 2.2 MB     00:01    
(2/12): gcc-c++-8.5.0-21.el8.x86_64.rpm         3.6 MB/s |  12 MB     00:03    
(3/12): elfutils-debuginfod-client-0.190-2.el8. 184 kB/s |  77 kB     00:00    
(4/12): cpp-8.5.0-21.el8.x86_64.rpm             3.2 MB/s |  10 MB     00:03    
(5/12): elfutils-libelf-0.190-2.el8.x86_64.rpm  528 kB/s | 233 kB     00:00    
(6/12): elfutils-libelf-devel-0.190-2.el8.x86_6 352 kB/s |  62 kB     00:00    
(7/12): elfutils-libs-0.190-2.el8.x86_64.rpm    481 kB/s | 305 kB     00:00    
(8/12): libgcc-8.5.0-21.el8.x86_64.rpm          307 kB/s |  82 kB     00:00    
(9/12): libgomp-8.5.0-21.el8.x86_64.rpm         543 kB/s | 208 kB     00:00    
(10/12): libstdc++-8.5.0-21.el8.x86_64.rpm      1.0 MB/s | 458 kB     00:00    
(11/12): kernel-devel-4.18.0-536.el8.x86_64.rpm 4.0 MB/s |  28 MB     00:07    
(12/12): gcc-8.5.0-21.el8.x86_64.rpm            4.4 MB/s |  23 MB     00:05    
--------------------------------------------------------------------------------
Total                                           7.3 MB/s |  78 MB     00:10     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Upgrading        : elfutils-libelf-0.190-2.el8.x86_64                    1/23
  Upgrading        : libgcc-8.5.0-21.el8.x86_64                            2/23
  Running scriptlet: libgcc-8.5.0-21.el8.x86_64                            2/23
  Upgrading        : libstdc++-8.5.0-21.el8.x86_64                         3/23
  Running scriptlet: libstdc++-8.5.0-21.el8.x86_64                         3/23
  Upgrading        : libstdc++-devel-8.5.0-21.el8.x86_64                   4/23
  Upgrading        : elfutils-libs-0.190-2.el8.x86_64                      5/23
  Upgrading        : elfutils-debuginfod-client-0.190-2.el8.x86_64         6/23
  Upgrading        : elfutils-libelf-devel-0.190-2.el8.x86_64              7/23
  Upgrading        : libgomp-8.5.0-21.el8.x86_64                           8/23
  Running scriptlet: libgomp-8.5.0-21.el8.x86_64                           8/23
  Upgrading        : cpp-8.5.0-21.el8.x86_64                               9/23
  Running scriptlet: cpp-8.5.0-21.el8.x86_64                               9/23
  Upgrading        : gcc-8.5.0-21.el8.x86_64                              10/23
  Running scriptlet: gcc-8.5.0-21.el8.x86_64                              10/23
  Installing       : kernel-devel-4.18.0-536.el8.x86_64                   11/23
  Running scriptlet: kernel-devel-4.18.0-536.el8.x86_64                   11/23 
  Upgrading        : gcc-c++-8.5.0-21.el8.x86_64                          12/23 
  Cleanup          : elfutils-debuginfod-client-0.189-3.el8.x86_64        13/23 
  Cleanup          : elfutils-libs-0.189-3.el8.x86_64                     14/23 
  Cleanup          : gcc-c++-8.5.0-20.el8.x86_64                          15/23 
  Cleanup          : elfutils-libelf-devel-0.189-3.el8.x86_64             16/23 
  Cleanup          : libstdc++-devel-8.5.0-20.el8.x86_64                  17/23 
  Cleanup          : libstdc++-8.5.0-20.el8.x86_64                        18/23 
  Running scriptlet: libstdc++-8.5.0-20.el8.x86_64                        18/23 
  Running scriptlet: gcc-8.5.0-20.el8.x86_64                              19/23 
  Cleanup          : gcc-8.5.0-20.el8.x86_64                              19/23 
  Running scriptlet: cpp-8.5.0-20.el8.x86_64                              20/23 
  Cleanup          : cpp-8.5.0-20.el8.x86_64                              20/23 
  Cleanup          : libgcc-8.5.0-20.el8.x86_64                           21/23 
  Running scriptlet: libgcc-8.5.0-20.el8.x86_64                           21/23 
  Running scriptlet: libgomp-8.5.0-20.el8.x86_64                          22/23 
  Cleanup          : libgomp-8.5.0-20.el8.x86_64                          22/23
  Running scriptlet: libgomp-8.5.0-20.el8.x86_64                          22/23 
  Cleanup          : elfutils-libelf-0.189-3.el8.x86_64                   23/23 
  Running scriptlet: elfutils-libelf-0.189-3.el8.x86_64                   23/23 
  Verifying        : kernel-devel-4.18.0-536.el8.x86_64                    1/23
  Verifying        : gcc-c++-8.5.0-21.el8.x86_64                           2/23 
  Verifying        : gcc-c++-8.5.0-20.el8.x86_64                           3/23
  Verifying        : libstdc++-devel-8.5.0-21.el8.x86_64                   4/23 
  Verifying        : libstdc++-devel-8.5.0-20.el8.x86_64                   5/23 
  Verifying        : cpp-8.5.0-21.el8.x86_64                               6/23 
  Verifying        : cpp-8.5.0-20.el8.x86_64                               7/23 
  Verifying        : elfutils-debuginfod-client-0.190-2.el8.x86_64         8/23
  Verifying        : elfutils-debuginfod-client-0.189-3.el8.x86_64         9/23
  Verifying        : elfutils-libelf-0.190-2.el8.x86_64                   10/23 
  Verifying        : elfutils-libelf-0.189-3.el8.x86_64                   11/23
  Verifying        : elfutils-libelf-devel-0.190-2.el8.x86_64             12/23
  Verifying        : elfutils-libelf-devel-0.189-3.el8.x86_64             13/23
  Verifying        : elfutils-libs-0.190-2.el8.x86_64                     14/23 
  Verifying        : elfutils-libs-0.189-3.el8.x86_64                     15/23
  Verifying        : gcc-8.5.0-21.el8.x86_64                              16/23
  Verifying        : gcc-8.5.0-20.el8.x86_64                              17/23
  Verifying        : libgcc-8.5.0-21.el8.x86_64                           18/23
  Verifying        : libgcc-8.5.0-20.el8.x86_64                           19/23
  Verifying        : libgomp-8.5.0-21.el8.x86_64                          20/23
  Verifying        : libgomp-8.5.0-20.el8.x86_64                          21/23
  Verifying        : libstdc++-8.5.0-21.el8.x86_64                        22/23
  Verifying        : libstdc++-8.5.0-20.el8.x86_64                        23/23 

Upgraded:
  cpp-8.5.0-21.el8.x86_64
  elfutils-debuginfod-client-0.190-2.el8.x86_64
  elfutils-libelf-0.190-2.el8.x86_64
  elfutils-libelf-devel-0.190-2.el8.x86_64
  elfutils-libs-0.190-2.el8.x86_64
  gcc-8.5.0-21.el8.x86_64
  gcc-c++-8.5.0-21.el8.x86_64
  libgcc-8.5.0-21.el8.x86_64
  libgomp-8.5.0-21.el8.x86_64
  libstdc++-8.5.0-21.el8.x86_64
  libstdc++-devel-8.5.0-21.el8.x86_64
Installed:
  kernel-devel-4.18.0-536.el8.x86_64

Complete!
Copy iso file C:\Program Files\Oracle\VirtualBox\VBoxGuestAdditions.iso into the box /tmp/VBoxGuestAdditions.iso
Mounting Virtualbox Guest Additions ISO to: /mnt
mount: /mnt: WARNING: device write-protected, mounted read-only.
Installing Virtualbox Guest Additions 7.0.4 - guest version is 6.1.30
Verifying archive integrity...  100%   MD5 checksums are OK. All good.
Uncompressing VirtualBox 7.0.4 Guest Additions for Linux  100%
VirtualBox Guest Additions installer
Removing installed version 6.1.30 of VirtualBox Guest Additions...
/opt/VBoxGuestAdditions-7.0.4/bin/VBoxClient: error while loading shared libraries: libX11.so.6: cannot open shared object file: No such file or directory
/opt/VBoxGuestAdditions-7.0.4/bin/VBoxClient: error while loading shared libraries: libX11.so.6: cannot open shared object file: No such file or directory
VirtualBox Guest Additions: Starting.
VirtualBox Guest Additions: Setting up modules
VirtualBox Guest Additions: Building the VirtualBox Guest Additions kernel
modules.  This may take a while.
VirtualBox Guest Additions: To build modules for other installed kernels, run
VirtualBox Guest Additions:   /sbin/rcvboxadd quicksetup <version>
VirtualBox Guest Additions: or
VirtualBox Guest Additions:   /sbin/rcvboxadd quicksetup all
VirtualBox Guest Additions: Building the modules for kernel
4.18.0-513.el8.x86_64.

VirtualBox Guest Additions: Look at /var/log/vboxadd-setup.log to find out what 
went wrong
VirtualBox Guest Additions: Running kernel modules will not be replaced until 
the system is restarted
An error occurred during installation of VirtualBox Guest Additions 7.0.4. Some functionality may not work as intended.
In most cases it is OK that the "Window System drivers" installation failed.
Redirecting to /bin/systemctl start vboxadd.service
Redirecting to /bin/systemctl start vboxadd-service.service
Unmounting Virtualbox Guest Additions ISO from: /mnt
Got different reports about installed GuestAdditions version:
Virtualbox on your host claims:   6.1.30
VBoxService inside the vm claims: 7.0.4
Going on, assuming VBoxService is correct...
Got different reports about installed GuestAdditions version:
Virtualbox on your host claims:   6.1.30
VBoxService inside the vm claims: 7.0.4
Going on, assuming VBoxService is correct...
Got different reports about installed GuestAdditions version:
Virtualbox on your host claims:   6.1.30
VBoxService inside the vm claims: 7.0.4
Going on, assuming VBoxService is correct...
Restarting VM to apply changes...
==> kernel-update: Attempting graceful shutdown of VM...
==> kernel-update: Booting VM...
==> kernel-update: Waiting for machine to boot. This may take a few minutes...
==> kernel-update: Machine booted and ready!
==> kernel-update: Checking for guest additions in VM...
==> kernel-update: Setting hostname...
PS C:\Users\user\Otus\task-1>
```
### Обновление ядра
#### Подключение к виртуальной машине:
```
PS C:\Users\user\Otus\task-1> vagrant ssh
[vagrant@kernel-update ~]$ 
```
#### Проверка версии ядра до обновления:
```
[vagrant@kernel-update ~]$ uname -r
4.18.0-513.el8.x86_64
```
#### Подключение репозитория:
```
[vagrant@kernel-update ~]$ sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
CentOS Stream 8 - AppStream                                                                                                               2.2 kB/s | 4.4 kB     00:01    
CentOS Stream 8 - BaseOS                                                                                                                  5.3 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - Extras                                                                                                                  3.9 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                                                  4.3 kB/s | 3.0 kB     00:00    
Extra Packages for Enterprise Linux 8 - x86_64                                                                                             43 kB/s |  31 kB     00:00    
Extra Packages for Enterprise Linux 8 - Next - x86_64                                                                                      69 kB/s |  36 kB     00:00    
elrepo-release-8.el8.elrepo.noarch.rpm                                                                                                     10 kB/s |  13 kB     00:01    
Dependencies resolved.
========================================================================================================================================================================== Package                                   Architecture                      Version                                        Repository                               Size 
==========================================================================================================================================================================Installing:
 elrepo-release                            noarch                            8.3-1.el8.elrepo                               @commandline                             13 k 

Transaction Summary
==========================================================================================================================================================================Install  1 Package

Total size: 13 k
Installed size: 5.0 k
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                  1/1 
  Installing       : elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                           1/1 
  Verifying        : elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                           1/1 

Installed:
  elrepo-release-8.3-1.el8.elrepo.noarch

Complete!
```
#### Установка последнего ядра из репозитория:
```
[vagrant@kernel-update ~]$ sudo yum --enablerepo elrepo-kernel install kernel-ml -y                                                                                   1/1 
CentOS Stream 8 - AppStream                                                                                                               3.3 kB/s | 4.4 
kB     00:01
CentOS Stream 8 - BaseOS                                                                                                                  7.5 kB/s | 3.9 
kB     00:00
CentOS Stream 8 - Extras                                                                                                                  5.7 kB/s | 2.9 
kB     00:00
CentOS Stream 8 - Extras common packages                                                                                                  5.3 kB/s | 3.0 kB     00:01    
kB     00:00                                                                                                                                             kB     00:00    
ELRepo.org Community Enterprise Linux Repository - el8                                                                                    1.1 kB/s | 214 kB     00:00    
kB     03:09                                                                                                                                             kB     00:00    
ELRepo.org Community Enterprise Linux Kernel Repository - el8         [                                          ===                    ] ---  B/s |   0 kB     03:09     
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                             11 kB/s | 2.2 MB     03:16      B     --:-- ETA
Extra Packages for Enterprise Linux 8 - x86_64                                                                            28 kB/s |  31 kB     00:01    
Extra Packages for Enterprise Linux 8 - Next - x86_64                                                                     46 kB/s |  36 kB     00:00    
Dependencies resolved.
========================================================================================================================================================= Package                                 Architecture                 Version                                  Repository                           Size 
=========================================================================================================================================================Installing:
 kernel-ml                               x86_64                       6.7.1-1.el8.elrepo                       elrepo-kernel                       119 k 
Installing dependencies:
 kernel-ml-core                          x86_64                       6.7.1-1.el8.elrepo                       elrepo-kernel                        39 M
 kernel-ml-modules                       x86_64                       6.7.1-1.el8.elrepo                       elrepo-kernel                        34 M 

Transaction Summary
=========================================================================================================================================================Install  3 Packages

Total download size: 73 M
Installed size: 115 M
Downloading Packages:
(1/3): kernel-ml-6.7.1-1.el8.elrepo.x86_64.rpm                                                                            45 kB/s | 119 kB     00:02    
(2/3): kernel-ml-modules-6.7.1-1.el8.elrepo.x86_64.rpm                                                                   115 kB/s |  34 MB     05:03     
(3/3): kernel-ml-core-6.7.1-1.el8.elrepo.x86_64.rpm                                                                      117 kB/s |  39 MB     05:41     
---------------------------------------------------------------------------------------------------------------------------------------------------------Total                                                                                                                    143 kB/s |  73 MB     08:46     
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                             72 kB/s | 1.7 kB     00:00    
Importing GPG key 0xBAADAE52:
 Userid     : "elrepo.org (RPM Signing Key for elrepo.org) <secure@elrepo.org>"
 Fingerprint: 96C0 104F 6315 4731 1E0B B1AE 309B C305 BAAD AE52
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                 1/1 
  Installing       : kernel-ml-core-6.7.1-1.el8.elrepo.x86_64                                                                                        1/3 
  Running scriptlet: kernel-ml-core-6.7.1-1.el8.elrepo.x86_64                                                                                        1/3 
  Installing       : kernel-ml-modules-6.7.1-1.el8.elrepo.x86_64                                                                                     2/3 
  Running scriptlet: kernel-ml-modules-6.7.1-1.el8.elrepo.x86_64                                                                                     2/3 
  Installing       : kernel-ml-6.7.1-1.el8.elrepo.x86_64                                                                                             3/3 
  Running scriptlet: kernel-ml-core-6.7.1-1.el8.elrepo.x86_64                                                                                        3/3 
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y

  Running scriptlet: kernel-ml-6.7.1-1.el8.elrepo.x86_64                                                                                             3/3 
  Verifying        : kernel-ml-6.7.1-1.el8.elrepo.x86_64                                                                                             1/3 
  Verifying        : kernel-ml-core-6.7.1-1.el8.elrepo.x86_64                                                                                        2/3 
  Verifying        : kernel-ml-modules-6.7.1-1.el8.elrepo.x86_64                                                                                     3/3 

Installed:
  kernel-ml-6.7.1-1.el8.elrepo.x86_64           kernel-ml-core-6.7.1-1.el8.elrepo.x86_64           kernel-ml-modules-6.7.1-1.el8.elrepo.x86_64

Complete!
[vagrant@kernel-update ~]$ 
```
#### Обновление конфигурации загрузчика:
```
[vagrant@kernel-update ~]$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
done
```
#### Выбор загрузки нового ядра по-умолчанию:
```
[vagrant@kernel-update ~]$ sudo grub2-set-default 0
[vagrant@kernel-update ~]$ 
``` 
#### Перезагрузка виртуальной машины:
```
[vagrant@kernel-update ~]$ sudo reboot
Connection to 127.0.0.1 closed by remote host.
Connection to 127.0.0.1 closed.
PS C:\Users\user\Otus\task-1> 
```
#### Проверка версии ядра после обновления:
```
[vagrant@kernel-update ~]$ uname -r
6.7.1-1.el8.elrepo.x86_64
```
## Загрузка домашнего задания в GitHub
### На GitHub создал аккунт и cгенерировал токен доступа.
### Создал публичный репозиторй "task-1".
### В каталоге C:\Users\user\Otus\task-1 создал файл README.md
```
C:\Users\user\Otus\task-1>echo "# Занятие 1. Vagrant-стенд для обновления ядра в ОС Linux" >> README.md
```
### Отредактировал README.md - добавил этот текст.
### Добавил в папку систему контроля версий git:
```
C:\Users\user\Otus\task-1>git init
```
### Добавил файлы README.md и Vagrantfile в систему контроля версий:
```
C:\Users\user\Otus\task-1>git add README.md Vagrantfile
```
### Зафиксирова изменения:
```
C:\Users\user\Otus\task-1>git commit -m «task-1»    
[master 524519a] «task-1»
 1 file changed, 2 insertions(+), 2 deletions(-)

C:\Users\user\Otus\task-1>git push -u origin master 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 324 bytes | 324.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/b1932177/task-1.git
   64d2dd5..524519a  master -> master
branch 'master' set up to track 'origin/master'.
```
## Особенносит реализации решения
- Хостовая ОС: Windows 10
- Vagnant 2.4.0
- Git 2.39.1.windows.1
- Visual Studio Code 1.85.2
- Proton VPN 3.2.9
- Oracle Virtual Box 7.0
- Extention Pack 7.0

