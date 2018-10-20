This is a repository for building LovrApp, a standalone Android app which is based on the [LÖVR](lovr.org) VR API.

# Usage

## To build (Mac instructions):

* The submodule cmakelib/lovr has submodules. If you did not initially clone this repo with --recurse-submodules, you will need to run `(cd cmakelib/lovr && git submodule init && git submodule update)` before doing anything else.

* Install Android Studio

* Open Android Studio, go into Preferences, search in the box for "SDK". use the "Android SDK" pane to download Android API level 21. Now quit Android Studio (in my testing it is broken and will break your project).

* Run:

        PATH="/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home/bin":~/Library/Android/sdk/platform-tools:$PATH adb devices

    Get your id number for the device.

* Plug the id number from adb into [https://dashboard.oculus.com/tools/osig-generator/]

* Copy the file downloaded from osig-generator into `LovrApp/assets`

* cd `LovrApp/Projects/Android`

* ```PATH="/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home/bin":~/Library/Android/sdk/platform-tools:$PATH ANDROID_HOME=~/Library/Android/sdk ../../../gradlew installDebug```

* **IF IT FAILS WITH A MESSAGE ABOUT `z_crc_t`:**

	* Edit the file `cmakelib/lovr/deps/assimp/CMakeLists.txt`. On the first line, insert:

		message(FATAL_ERROR fake_failure)

	* Rerun the `installDebug` step. It will fail again, this time with the message "fake_failure".

	* Remove the line you just added to `cmakelib/lovr/deps/assimp/CMakeLists.txt`.

	* Rerun the `installDebug` step again. This time it will succeed.

	* Yes, this is *completely ridiculous*. :(

Notes: 
* To see other things gradlew can do run it with "tasks" as the argument.
* The reason for the long PATH/ANDROID_HOME line is to get the java and android tools into scope for that line. You could also just modify the env vars in your bashrc.
* You have to have turned on developer mode on your headset before deploying.
* If it gets stuck complaining about "unauthorized", try putting on the headset and see if there's a permissions popup.
* On repeat builds, the build will sometimes fail to recognize that cmakelib is a dependency of LovrApp. When this happens, you might have to separately build cmakelib before building LovrApp. For example:

    (cd ../../../cmakelib; PATH="/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home/bin":~/Library/Android/sdk/platform-tools:$PATH ANDROID_HOME=~/Library/Android/sdk ../gradlew build) && (PATH="/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home/bin":~/Library/Android/sdk/platform-tools:$PATH ANDROID_HOME=~/Library/Android/sdk ../../../gradlew installDebug) && say "Beep"

Any help with fixing the "unusual" or inconsistent steps above, or getting the project to build in Android Studio, would be much appreciated.

## To make your own app:

Decide on a name for your app and also an identifier (this is something like "com.companyname.gametitle").

Edit `LovrApp/Projects/build.gradle`. Change "project.archivesBaseName" and "applicationId" to reflect your identifier.

Edit `LovrApp/Projects/Android/AndroidManifest.xml`. Change "package=" at the top to your identifier and change "android:label=" partway down (right after YOUR NAME HERE) to your name.

# Contributing

## Upgrading

This repository has a branch "oculus-original-sdk". As commits, this branch has versions of some versions of official Oculus Mobile SDK releases (with those samples which are not necessary, notably the ones with large embedded video samples, deleted).

If you need to upgrade the version of Oculus Mobile SDK used by this release, you should do so by checking out the "oculus-original-sdk" branch, unpacking the latest ovr_sdk_mobile package into the repository, committing, and then merging back into the master branch. This will ensure that changes to the SDK are cleanly incorporated.

# License

This repository is based on the Oculus Mobile SDK, which Oculus has made available under the terms in [LICENSE-OCULUS.txt](LICENSE-OCULUS.txt), and Lovr, which is available under the terms in "cmakelib/lovr/LICENSE".

LovrApp (the additional glue built on top) is by Andi McClure <<andi.m.mcclure@gmail.com>> and is made available under the license below.

> Copyright 2018 Andi McClure
>
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
> 
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.