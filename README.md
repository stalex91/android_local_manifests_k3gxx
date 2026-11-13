# LineageOS 16.0 for the Galaxy S5 Exynos (SM-G900H, k3gxx)

Local manifest for building the unofficial LineageOS 16.0 (Android 9) port
for the Samsung Galaxy S5 SM-G900H (Exynos 5422).

## Sources

| Path | Repository |
|---|---|
| device/samsung/k3gxx | [stalex91/android_device_samsung_k3gxx](https://github.com/stalex91/android_device_samsung_k3gxx) |
| kernel/samsung/k3gxx | [stalex91/android_kernel_samsung_k3gxx](https://github.com/stalex91/android_kernel_samsung_k3gxx) (GPL-2.0) |
| hardware/samsung, hardware/samsung_slsi-cm/* | forks of LineageOS and exynos5420 |
| build/make, frameworks/av, frameworks/base, hardware/interfaces, hardware/broadcom/wlan, vendor/lineage, packages/apps/{DeskClock,Settings,SetupWizard} | forks of LineageOS with the k3gxx changes on `lineage-16.0` |
| external/stlport | fork of LineageOS: libstlport built from source for the Samsung RIL |

Based on the work of the [exynos5420](https://github.com/exynos5420) team
(device tree, kernel, Exynos 5422 hardware) and of LineageOS.

## Build

Use a Linux host with the usual LineageOS 16.0 build environment.

```sh
mkdir lineage && cd lineage
repo init -u https://github.com/LineageOS/android.git -b lineage-16.0
mkdir -p .repo/local_manifests
curl -o .repo/local_manifests/k3gxx.xml \
  https://raw.githubusercontent.com/stalex91/android_local_manifests_k3gxx/lineage-16.0/k3gxx.xml
repo sync -c -j8
```

### Proprietary files

They are not published. Extract them from an SM-G900H with adb (root
access in adb), running either the stock Android 6 firmware or this ROM:

```sh
cd device/samsung/k3gxx
./extract-files.sh            # or: ./extract-files.sh /path/to/extracted/system
./setup-makefiles.sh
cd ../../..
```

This creates `vendor/samsung/k3gxx`.

### Compile

```sh
source build/envsetup.sh
lunch lineage_k3gxx-userdebug
mka bacon
```

The device is not an official LineageOS target, so `breakfast` does not
work; use `lunch` as above.

## Notes

- Use a TWRP for k3g that supports Android 9 installers (e.g. 3.7.x).
- The official builds are signed with the maintainer's keys; a build of
  your own is signed with the test keys and needs a clean install when
  switching between them.
