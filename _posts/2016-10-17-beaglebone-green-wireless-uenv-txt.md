---
layout: post
title:  "Correct BeagleBone Green Wireless uEnv.txt"
date:   2016-10-17 20:00:00 +0200
categories: beaglebone
---

[BeagleBone] comes in different flavors: White, Black, Green, Green
Wireless. But when it comes to update your debian [image], you will
find only one: same TI specific kernel, same *armhf* packages. The
only difference comes in the choice of the `dtb` ([device tree blob]),
a binary file, compile from a `dts` file thanks to the `dtc` tool that
describe your spécific SBC hardware.

**Be carefull:** Using the wrong `dtb` file may damage your card.

All the `dtb` of the beaglebone image can be found in
``/boot/dtbs/`uname -r`/``:
{% highlight bash-session %}
jerome@beaglebone:~$ ls /boot/dtbs/4.4.9-ti-r25/
am335x-abbbi.dtb                      am335x-bonegreen-overlay.dtb   am571x-idk-lcd-osd.dtb
am335x-arduino-tre.dtb                am335x-bonegreen-wireless.dtb  am572x-idk.dtb
am335x-baltos-ir5221.dtb              am335x-chiliboard.dtb          am572x-idk-lcd-osd.dtb
am335x-base0033.dtb                   am335x-evm.dtb                 am57xx-beagle-x15.dtb
am335x-boneblack-audio.dtb            am335x-evmsk.dtb               am57xx-beagle-x15-revb1.dtb
am335x-boneblack-bbb-exp-c.dtb        am335x-icev2.dtb               am57xx-evm.dtb
am335x-boneblack-bbb-exp-r.dtb        am335x-lxm.dtb                 am57xx-evm-reva3.dtb
am335x-boneblack-bbbmini.dtb          am335x-nano.dtb                dra72-evm.dtb
am335x-boneblack-cape-bone-argus.dtb  am335x-olimex-som.dtb          dra72-evm-lcd-lg.dtb
am335x-boneblack.dtb                  am335x-pepper.dtb              dra72-evm-lcd-osd.dtb
am335x-boneblack-emmc-overlay.dtb     am335x-sancloud-bbe.dtb        dra72-evm-revc.dtb
am335x-boneblack-hdmi-overlay.dtb     am335x-sl50.dtb                dra7-evm.dtb
am335x-boneblack-nhdmi-overlay.dtb    am335x-wega-rdk.dtb            dra7-evm-lcd-lg.dtb
am335x-boneblack-overlay.dtb          am437x-gp-evm.dtb              dra7-evm-lcd-osd.dtb
am335x-boneblack-wireless.dtb         am437x-gp-evm-hdmi.dtb         omap5-cm-t54.dtb
am335x-boneblack-wl1835mod.dtb        am437x-idk-evm.dtb             omap5-igep0050.dtb
am335x-bone-cape-bone-argus.dtb       am437x-sk-evm.dtb              omap5-sbc-t54.dtb
am335x-bone.dtb                       am43x-epos-evm.dtb             omap5-uevm.dtb
am335x-bonegreen.dtb                  am571x-idk.dtb
{% endhighlight %}

For the *beaglebone green wireless*, we need to configure `uBoot` to
load `am335x-bonegreen-wireless.dtb`. It is done by editing `/boot/uEnv.txt`:
```
#Docs: http://elinux.org/Beagleboard:U-boot_partitioning_layout_2.0
uname_r=4.4.9-ti-r25
#uuid=
dtb=am335x-bonegreen-wireless.dtb
cmdline=coherent_pool=1M quiet cape_universal=enable
```

After a reboot *(or two)*, you will see that the `eth0` device
*deasappear* and is replaced by the `wlan0` device:
```
debian@beaglebone:~$ sudo iwconfig
wlan0     IEEE 802.11abgn  ESSID:off/any
          Mode:Managed  Access Point: Not-Associated   Tx-Power=0 dBm
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
```

[BeagleBone]: https://beagleboard.org/bone
[image]: https://beagleboard.org/latest-images
[device tree blob]: http://elinux.org/Device_Tree