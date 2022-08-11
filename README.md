# Android Signature Spoofing

How I enabled signature spoofing on my LineageOS 18.1 (android 11) phone without magisk/root

## How to

First of all, make sure you have a backup (which can be created using TWRP)!

You need ADB, java and (un)zip installed on your pc and TWRP recovery on your phone

```bash
adb reboot recovery
```

Go to mount and enable/check system, then run the commands here below

```bash
mkdir ~/signatureSpoof && cd ~/signatureSpoof
wget https://github.com/DexPatcher/dexpatcher-tool/releases/download/v1.8.0-beta1/dexpatcher-1.8.0-beta1.jar
wget https://gitlab.com/oF2pks/haystack/-/archive/11-attempt/haystack-11-attempt.zip && unzip haystack-11-attempt.zip

adb pull /system/framework/services.jar
cp services.jar services-backup.jar
java -jar dexpatcher-1.8.0-beta1.jar -a 11 -M -v -d -o ./ services.jar haystack-11-attempt/11-hook-services.jar.dex haystack-11-attempt/11core-services.jar.dex
rm services.jar
zip -j services.jar classes*.dex
adb push services.jar /system/framework/services.jar

unzip -d spoofing 'haystack-11-attempt/spoof_AVDapi30.zip.ONLY'\''MAGISK&ANDROID-STUDIO'
adb push spoofing/system/framework/org.spoofing.apk /system/framework/org.spoofing.apk

adb reboot
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
