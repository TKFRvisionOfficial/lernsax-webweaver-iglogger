**This project is in no way associated with LernSax, WebWeaver, DigiOnline GmbH or Freistaat Sachsen**

# Description
This is a modification for the lernsax android app (and probarly other webweaver apps too) that integrates IGLogger into the app. 
This allows the user to watch the requests between the android device and the json_rpc api of the LernSax/WebWeaver servers. 
This can be helpful for the reverse engineering and documentation of the json_rpc lernsax api. 
If you want to look at the already existing documentation, want to share dumps or want to share you knowledge you can take look at my own **[research repository](https://github.com/TKFRvisionOfficial/lernsax-webweaver-api-research)**.

# Usage
Using [logcat](https://developer.android.com/studio/command-line/logcat) you will see every request that the lernsax app makes marked as LernLog.REQ and every response it gets as LernLog.RESP.

# Installation

## Requirements
**[lernsax](https://play.google.com/store/apps/details?id=de.digionline.webweaverlernsax)**
This probarly works with other WebWeaver based apps too but I couldn't try it out yet.<br>
**[apkextractor](https://play.google.com/store/apps/details?id=com.ext.ui)**<br>
**[apktool](https://ibotpeaches.github.io/Apktool/)**<br>
**[iglogger.smali](https://raw.githubusercontent.com/b3nn/IGLogger/master/iglogger_for_APKTOOLv2.smali)**<br>
**[JsonApiTask.diff](https://raw.githubusercontent.com/TKFRvisionOfficial/lernsax-webweaver-iglogger/main/JsonApiTask.diff)**<br>
**java jdk**
### only required for windows
**[patch](http://gnuwin32.sourceforge.net/downlinks/patch-bin-zip.php)**


## Steps
1. Install LernSax on your phone.
2. Use APKExtractor to create the lernsax.apk
3. Transfer the apk to your PC.
4. Decode the apk using apktool<br>
``apktool d -o "lernsaxdecoded" lernsax.apk``
5. Rename the iglogger file to iglogger.smali if you haven't already
6. Copy the iglogger.smali to ``lernsaxdecoded/smali/de/digionline/webweaver/api/tasks``
7. Patch the JsonApiTask.smali<br>
``patch lernsaxdecoded/smali/de/digionline/webweaver/api/tasks/JsonApiTask.smali JsonApiTask.diff``
8. Build the patched apk<br>
``apktool b -o lernsaxpatched.apk lernsaxdecoded``
9. Create a key for signing<br>
``keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000``
10. Sign the apk with the key<br>
``jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore lernsaxpatched.apk alias_name``
11. Transfer the apk to your device.
12. Uninstall the lernsax app.
13. Install the patched apk.
14. Activate USB Debugging
15. Use [logcat](https://developer.android.com/studio/command-line/logcat) to watch the log
16. profit!
