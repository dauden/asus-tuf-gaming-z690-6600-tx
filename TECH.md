# This guide step by step to install

## - pre-install
- Prepare EFI
  - Download OpenCore from [here](https://github.com/acidanthera/OpenCorePkg/releases), then create your owner EFI 
  - or Clone my EFI if you have the same [my hardware](README.md#hardware-spec)
- Create USB install
  - Get disk number in list
  ```shell
  $ diskutil list
    ```
  - format disk,
  ```shell
  $ diskutil partitionDisk /dev/disk`<DiskNumber>` GPT JHFS+ "USB" 100%
  # replace your disk number from disk lit
  # example: diskutil partitionDisk /dev/disk03 GPT JHFS+ "USB" 100%
    ```
  - Create a bootable
    - macOS Sonoma
    ```shell
      $ sudo /Applications/Install\ macOS\ Sonoma.app/Contents/Resources/createinstallmedia --volume /Volumes/USB
     ```
    
    - Ventura
    ```shell
    $ sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
    ```
    
- Config bios
  - You need config, depending on your hardware but all most you need:
    * Hyper Threading - Enabled 
    * Above 4G decoding - on 
    * Fast Boot - off 
    * XHCI Hand-off - on
  - If you are using my EFI you can refer to my [BIOS config](README.md#bios-config)
  - Or you can restore BIOS and BIOS configs from [my backup](/BIOS)
  - NOTE
    - you should not upgrade BIOS at lasted version below (I'm totally stuck in this version for one day of work):
    ```
      TUF GAMING Z690-PLUS WIFI D4 BIOS 2802
      Version 2802
      11.19 MB
      2023/10/06
    ```
     currently it work well with BIOS version: 
    ```
      TUF GAMING Z690-PLUS WIFI D4 BIOS 2403
      Version 2403
      11.14 MB
      2023/06/21
    ```

- Config/Edit or modify your EFI with OPENCONFIG
   - you can download openconfig from [here](https://mackie100projects.altervista.org/opencore-configurator/)

## - Install
- plug bootable to USB 2.0 port (not 3.0 port <Blue port>)
- boot and install Mac Os as Apple guideline

## - Pro-install
don't forget copy your EFI from Bootable to EFI Disk

- mount two EFI disks (one in USB and one in internal DISK)
```shell
$ sudo diskutil mount /dev/disk<Number>
# example: 
# $ sudo diskutil mount /dev/disk4
# $ sudo diskutil mount /dev/disk2
```
- Copy EFI
```shell
$ sudo cp -rf /Volumes/<Volume name EFI from USD disk>/EFI /Volumes/<Volume name of Internal EFI Disk>/EFI
# example sudo cp -rf /Volumes/EFI/EFI /Volumes/EFI 1/EFI
```
- Patcher if used Wifi BCM94360
After done and reboot Computer we don't see any wifi network in list. then we need Patch wifi
run [OpenCore-Patcher.app](tools%2FOpenCore-Patcher.app) 
  - choose Settings >> Security 
  - selected 
    - Disable AMFI
    - ALLOW_UNTRUSTED_KEXTS
    - ALLOW_UNRESTRICTED_FS
    - ALLOW_UNAUTHENTICATED_ROOT
    ![patcher.png](img%2Fpatcher_setting.png)
  - Back to Home (Return)
  - run Post-Install Root Patch
    ![patcher.png](img%2Fpatcher.png)
  - Wait finish the reboot 
Enjoy macOS on non-Apple hardware :)

## NOTE
- More information for Open Core install you find [here](https://dortania.github.io/OpenCore-Install-Guide/config.plist/)
- Ref [post](https://www.reddit.com/r/hackintosh/comments/sp1zgv/opencore_alder_lake_12thgen_intel_hackintosh) 