# Android Signature Spoofing

How I enabled signature spoofing on my LineageOS 18.1 (android 11) phone without magisk/root

## Downloading required things

```bash
mkdir ~/signatureSpoof && cd ~/signatureSpoof
wget https://github.com/DexPatcher/dexpatcher-tool/releases/download/v1.8.0-beta1/dexpatcher-1.8.0-beta1.jar
wget https://gitlab.com/oF2pks/haystack/-/archive/11-attempt/haystack-11-attempt.zip && unzip haystack-11-attempt.zip
wget https://downloads.nanolx.org/NanoDroid/Stable/NanoDroid-microG-23.1.2.20210117.zip
unzip -d spoofing 'haystack-11-attempt/spoof_AVDapi30.zip.ONLY'\''MAGISK&ANDROID-STUDIO'
```

Make sure you also have ADB (Android Debug Bridge), java and unzip installed on your PC.
Make sure you have TWRP recovery on your phone (others might work too, not sure)

## How to (needs to be done after each and every system update)

First of all, make sure you have a backup (which can be created using TWRP)!

```bash
adb reboot recovery
```

#### mount system (TWRP => mount => system) and run the commands below
```bash
cd ~/signatureSpoof
rm classes*.dex
rm services.jar
adb pull /system/framework/services.jar
cp services.jar services-backup.jar
java -jar dexpatcher-1.8.0-beta1.jar -a 11 -M -v -d -o ./ services.jar haystack-11-attempt/11-hook-services.jar.dex haystack-11-attempt/11core-services.jar.dex
rm services.jar
zip -j services.jar classes*.dex
adb push services.jar /system/framework/services.jar
adb push spoofing/system/framework/org.spoofing.apk /system/framework/org.spoofing.apk
```

#### enable ADB Sideload (TWRP => Advanced => ADB Sideload => Swipe to right)
```bash
adb sideload ~/signatureSpoof/NanoDroid-microG-23.1.2.20210117.zip
```

#### If you want you can uninstall the packages if you don't need them
```bash
# Aurora
adb shell pm uninstall com.aurora.services
adb shell pm uninstall com.aurora.store
# Location providers
adb shell pm uninstall org.fitchfamily.android.dejavu
adb shell pm uninstall org.microg.nlp.backend.nominatim
# Fakestore
adb shell pm uninstall com.android.vending
# Microg
adb shell pm uninstall com.google.android.gms
adb shell pm uninstall com.google.android.gsf
```

## Sources

* [Kingslayer9988](https://forum.xda-developers.com/t/signature-spoofing-on-unsuported-android-11-r-roms.4214143/)
* [Otus9051](https://github.com/Otus9051/spoof11)
* [1Michael23](https://github.com/1Michael23/android-11-signature-spoofing)


## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
