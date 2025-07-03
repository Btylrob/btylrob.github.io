
# Linux KVM and Ubuntu

<time id="post-date">2025-07-01</time>

<p id="post-excerpt">
In this post, I explain Linux KVM in simple terms and show how to download and use ISO of your choice in this case Ubuntu Satanic Edition.
</p>

---

# What Is a Linux KVM?

A **KVM**, or **Kernel-based Virtual Machine**, is a Linux kernel module that turns your operating system into a **hypervisor** — a platform that can run multiple **virtual machines (VMs)** on the same hardware.

KVM supports both Intel and AMD processors with hardware virtualization extensions (Intel VT-x or AMD-V). With KVM, your Linux system can manage and run isolated operating systems, sharing physical hardware resources.

If you're unfamiliar, a **hypervisor** is software that allows multiple operating systems to run on a single machine while sharing the CPU, RAM, disk, and other resources.




            +----------------+     +----------------+     +----------------+
            |   App A1       |     |   App B1       |     |   App C1       |
            |   App A2       |     |   App B2       |     |   App C2       |
            +----------------+     +----------------+     +----------------+
            |    OS A        |     |    OS B        |     |    OS C        |
            +----------------+     +----------------+     +----------------+
                      \               |               /
                       \              |              /
                        \             |             /
                         \            |            /
                        +----------------------------------+
                        |           Hypervisor             |
                        |  (Virtual Machine Monitor, VMM)  |
                        +----------------------------------+
                        |      Shared Physical Hardware     |
                        |  - CPU                            |
                        |  - RAM                            |
                        |  - Disk / Storage                 |
                        |  - Network Interfaces             |
                        |  - GPU / I/O Devices              |
                        +----------------------------------+


The hypervisor is essential, without it, you wouldn't be able to run VMs on your machine. The amazing thing is that with KVM, your Linux kernel becomes the hypervisor itself, no need for external software like VMware or VirtualBox (although those are fine options too).

As long as you're using a Linux distro and have a compatible Intel or AMD CPU, you're good to go!

---

## 🔧 Key KVM Components

* `qemu-system-x86`  The QEMU emulator used to run x86_64 VMs with KVM acceleration. |
* `libvirt-daemon-system` The background service that manages VMs. |
* `virtinst`  CLI tool to create and define new VMs. |
* `virt-manager`  A graphical application to manage VMs easily. |

To install them:

```bash
sudo apt install qemu-system-x86 libvirt-daemon-system virtinst \
virt-manager virt-viewer ovmf swtpm qemu-utils guestfs-tools \
libosinfo-bin tuned
```
## Instructions

* First, we are going to check if whether you have compatible virtualization hardware. If compatible, output should be VT-x for Intel and AMD.

```lscpu | grep Virtualization```


* Next Step we are going to check if our settings are compatible for virtualizations.

```zgrep CONFIG_KVM /boot/config-$(uname -r)```


Output should be this.
```
CONFIG_KVM_GUEST=y
CONFIG_KVM_MMIO=y
CONFIG_KVM_ASYNC_PF=y
CONFIG_KVM_VFIO=y
CONFIG_KVM_GENERIC_DIRTYLOG_READ_PROTECT=y
CONFIG_KVM_COMPAT=y
CONFIG_KVM_XFER_TO_GUEST_WORK=y
CONFIG_KVM=m
CONFIG_KVM_WERROR=y
CONFIG_KVM_INTEL=m
CONFIG_KVM_AMD=m
CONFIG_KVM_AMD_SEV=y
```

* Next command we will run will give our used permission to run accelerated VMs.

```sudo usermod -aG kvm YOURUSER```


* Now we will start and enable libvirtd which will help mange VMs.

```sudo systemctl start libvirtd && sudo systemctl enable libvirtd```



* After this Virtual Machine Manager should show in your apps, open that up and get an ISO image you would like to use. In this case I'm using Ubuntu Satanic edition profound yes but if you are curious like me, you can download it [here](https://archiveos.org/ubuntu-satanic/)!

* Once the download is done, open VM manager and click create new virtual machine whcih is the computer icon with the play button.

![Step1](https://github.com/Btylrob/btylrob.github.io/blob/main/site/instruction3.png?raw=true)

* Then you will want to choose the first option *Local install media (ISO Image or CDROM)* because in this ecample we will be using a download ISO.

![Step3](https://github.com/Btylrob/btylrob.github.io/blob/main/site/instruction4.png?raw=true)

* After that choose the ISO from your downloads folder. If you click your whole downloads folder it will default to that everytime you make a VM, making it easier for less file transfering. If you are using a lesser know OS un-click the*Automatically detect from installation media / source* and type in *Generic or Unknown OS*.

![Step2](https://github.com/Btylrob/btylrob.github.io/blob/main/site/instruction5.png?raw=true)

* Now depending on which CPU you have and ram you can adjust the setting for your VM. I typically do not like to put anything higher than 2 CPUs and 1024 MiB on the memory, as my computer does not have strong specs. If you have an average computer, just stick to the specs provided when you get to the create disk image part.

![Step3](https://github.com/Btylrob/btylrob.github.io/blob/main/site/instruction2.png?raw=true)

* Once the CPU and Memory settings are complete click forward and check your settings to make sure your preferences are correct. Make sure to rename your VM to something you will remember, then click finish.

![Step4](https://github.com/Btylrob/btylrob.github.io/blob/main/site/instruction1.png?raw=true)

* Congrats your VM should have started running and put you into the boot menu. Once ran you can access everything through the gui just like if you where using VMWare or Virtual Box.

![Ubuntu](https://github.com/Btylrob/btylrob.github.io/blob/main/site/ubuntu.png?raw=true)
