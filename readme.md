# Dracrius' fork of ZSA's fork of QMK Firmware With OpenRGB Support 

![OpenRGB Devices](https://i.imgur.com/WELbAyR.png)

## ZSA's Current

[![Current Version](https://img.shields.io/github/tag/ErgoDox-EZ/qmk_firmware.svg)](https://github.com/ErgoDox-EZ/qmk_firmware/tags)
[![GitHub contributors](https://img.shields.io/github/contributors/ErgoDox-EZ/qmk_firmware.svg)](https://github.com/ErgoDox-EZ/qmk_firmware/pulse/monthly)
[![GitHub forks](https://img.shields.io/github/forks/ErgoDox-EZ/qmk_firmware.svg?style=social&label=Fork)](https://github.com/ErgoDox-EZ/qmk_firmware/)

## QMK-OpenRGB - All Credit to @Kasper24 for the orignal Protocol

[Current Version on Gitlab in the OpenRGB Developers Community](https://gitlab.com/OpenRGBDevelopers/QMK-OpenRGB/)

[Original \ Current Branch on Github](https://github.com/Kasper24/QMK-OpenRGB)

## OpenRGB QMK Protocol Settings
    Name:ErgoDox EZ
    USB VID: 3297
    USB PID: 4976

![Protocol Settings](https://imgur.com/00CzmyJ.png)

## List of OpenRGB Modified Files

Here is a list of the files modified or added by Kasper24 that are required to add OpenRGB QMK support to any QMK Branch

    Added:
    quantum/openrgb.c
    quantum/openrgb.h
    quantum/rgb_matrix_animations/openrgb_direct_anim.h

    Modified for OpenRGB Support:
    common_features.mk 
    show_options.mk

    quantum/quantum.h
    quantum/rgb_matrix.c
    quantum/rgb_matrix_animations/rgb_matrix_effects.inc

    tmk_core/protocol/usb_descriptor.h
    tmk_core/protocol/arm_atsam/usb/udi_device_epsize.h

    keyboards/ergodox_ez/glow/keymaps/glow_openrgb/rules.mk

Rules.mk Note: only `OPENRGB_ENABLE = yes` is required to add support but you may have to disable other settings to be able to enable OpenRGB without getting a 
"not enough available endpoints" error.

For instance for the EZ Glow to maintian the most features I enabled `MOUSE_SHARED_EP = yes` which is enabled by default in most QMK firmwares but disabled by ZSA's rule's for the ErgoDox EZ, mostlikely just to maintain Boot Mouse compatibility as it breaks with `MOUSE_SHARED_EP = yes`. As you never need a mouse on boot anyways I felt this was a worth sacrafice to maintain WebUSB an ORYX support.

From my testing this is enough to add OpenRGB support to other QMK Branches / Keyboards. There apears to be no need to edit the keymap.c atleast for ErgoDox EZ Glow keyboards I'm sure your milage may very. This does me that you can still use ORYX to make keyboard layouts, but instead of clicking "Download this Layout" choose the "Download Source (Glow)" option and extract the keymap.c into the `keyboards\ergodox_ez\glow\keymaps\glow_openrgb` folder overwiting my personal keymap.

## How to Find your keyboard's VID & PID on Windows

1. From the Classic Control Panel Open the Keyboard Settings `Control Panel\All Control Panel Items\Keyboard`

2. Switch to the 'Hardware' Tab

![Hardware Tab](https://imgur.com/AdaLENA.png)

3. Scroll through the list and make sure all show "Device Status: This device is working properly."
4. Then Unplug your keyboard and find the that now say "Device Status: Unknown"
5. You can skip these two steps if your keyboard is more obviously named or identifyable.
6. Once you have found the keyboard in question click 'Properties'
7. Then Switch to the 'Details' tab and change the 'Property' dropdown to 'Hardware Ids'

![Hardware Ids](https://imgur.com/jygCN8y.png)

# ZSA's Readme

This purpose of this fork is maintain a clean repo that only contains the keyboard code that we need, and as little else as possible.  This is to keep it lightweight, since we only need a couple of keyboards. This is the repo that the EZ Configurator will pull from. 

## Supported Keyboards

* [ErgoDox EZ](/keyboards/ergodox_ez/)
* [Planck EZ](/keyboards/planck/ez)
* [Moonlander Mark I](/keyboards/moonlander)

## Building

To set up the local build enviroment to create the firmware image manually, head to the [Newbs guide from QMK](https://docs.qmk.fm/#/newbs).
And instead of using just `qmk setup`, you will want to run this instead: 

```sh
qmk setup zsa/qmk_firmware -b firmware20
```

## Maintainers

QMK is developed and maintained by Jack Humbert of OLKB with contributions from the community, and of course, [Hasu](https://github.com/tmk). The ZSA branch is maintained by Drashna, ZSA's official QMK Liaison, and by Florian Didron, ZSA's lead developer, with input from Erez Zukerman (ZSA CEO).


# Update Process

1. Check out branch from ZSA's master branch:
    1. `git remote add zsa https://github.com/zsa/qmk_firmware.git`
    2. `git fetch --all`
    3. `git checkout -B branchname zsa/master`
    4. `git push -u zsa branchname`
2. Check for core changes:
    - [https://github.com/qmk/qmk_firmware/commits/master/quantum](https://github.com/qmk/qmk_firmware/commits/master/quantum)
    - [https://github.com/qmk/qmk_firmware/commits/master/tmk_core](https://github.com/qmk/qmk_firmware/commits/master/tmk_core)
    - [https://github.com/qmk/qmk_firmware/commits/master/util](https://github.com/qmk/qmk_firmware/commits/master/util)
    - [https://github.com/qmk/qmk_firmware/commits/master/drivers](https://github.com/qmk/qmk_firmware/commits/master/drivers)
    - [https://github.com/qmk/qmk_firmware/commits/master/lib](https://github.com/qmk/qmk_firmware/commits/master/lib)
    - These folders are the important ones for maintaining the repo and keeping it properly up to date. Most, but not all, changes on this list should be pulled into our repo.
3. `git cherry-pick` the commits we want
    - `git rm docs/* -r` to remove the document updates when cherry picking. Repeat for any keyboard/keymap/etc that have changes that we aren't interested in
4. Commit update
   * Include commit info in `[changelog.md](http://changelog.md)` 
5. Open Pull request, and include information about the commit

## Strategy

To keep PRs small and easier to test, they should ideally be 1:1 with commits from QMK Firmware master. They should only group commits if/when it makes sense. Such as multiple commits for a specific feature (split RGB support, for instance)

## Merging

Pull Requests should be merged/rebased, not squashed, so we can maintain a commit history that is close to QMK Firmware's, for ease of reference.
