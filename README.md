# signingandroidapp
Signing the Development App

Manage Jenkins->Configure System -> Global properties -> Environmental Variables (add the following key -> value pairs) added 24-08-2016
KSTOREFILEPATH=/c9/home/c9uatadm/.signing/ba-dev-key.keystore
KSTOREALIAS=ba_dev_key
KSTOREPWD=
ALIASPWD=

1) Keep the keystore somewhere in the filesystem - /c9/home/c9uatadm/ba-dev-key.keystore
2) Set Password, Alias, Alias Password set as environment variables through Jenkins UI->Manage Jenkins.
3) Edit app/build.gradle
    if (project.hasProperty("jenkins")) {
        signingConfigs {
            release {
                storeFile file(System.getenv("KSTOREFILEPATH"))
                storePassword System.getenv("KSTOREPWD")
                keyAlias System.getenv("KSTOREALIAS")
                keyPassword System.getenv("ALIASPWD")
            }
        }

        buildTypes {
            release {
                signingConfig signingConfigs.release

            }
        }
    }
4) In Jenkins Job -> Build -> Invoke Gradle script -> Tasks, pass -Pjenkins so that the above signing configuration in build.gradle gets executed
tasks             ->             -Pjenkins clean build assembleRelease
5) Build results in signed apk located here - /c9/setup/data/jenkinstemp/jobs/test_android/workspace/app/build/outputs/apk/app-release.apk
6) Verify the signature by executing
[c9uatadm@c9uatap01 apk]$ keytool -list -printcert -jarfile app-release.apk
Signer #1:
Signature:
Owner: O=
Issuer: O=
Serial number: 521dbc84
Valid from: Wed Aug 28 10:01:56 BST 2013 until: Thu Aug 04 10:01:56 BST 2112
Certificate fingerprints:
         MD5:  
         SHA1: 
         SHA256: 
         Signature algorithm name: SHA1withRSA
         Version: 3
