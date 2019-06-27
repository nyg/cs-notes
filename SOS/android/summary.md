# Android

* .apk : application Android.
    * AndroidManifest.xml
    * classes.dex : Dalvik bytecode, DEX file format.
    * Resources.arsc

* DVM, machine virtuelle pour le bytecode Dalvik.

* Java/Kotlin compilé en DEX bytecode
* DEX bytecode désassemblé en Smali puis en Java

* Obfuscation : reflection, dynamic code loading, native code.

* ODEX : optimised DEX, plus rapide, plus de place, dépendent du device. Fichier supplémentaire en plus de l'APK.

* ART format : boot.art, pre-initialized memory du framework Android.
* OAT format : boot.oat, pre-compiled libraries. oatdump.

* unzip app.apk, dézip l'archive
* baksmali classes.dex -o output, désassemble les fichiers DEX en Smali.
* smali output -o patched.apk, processus inverse
* apktool d app.apk -o output
* apktool b output -o patched.apk

* Décompilation : JEB, BytecodeViewer, Jadx
    * dex2jar (Dalvik to Java bytecode)
    * Jd-Gui (Java bytecode to Java)

* aapt dump badging \*.apk
* adb devices
* adb install app.apk
* adb uninstall com.example.nsmlp
* adb push, pull, shell

* check app components, autorisations for functionnalities, network endpoints, network apis
* top-down approach
* static : inspect app without running it
* dynamic analysis : run app and check what it does, trace apis, syscalls, attach debugger, instrumentation
