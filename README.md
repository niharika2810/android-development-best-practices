## Title: Android Development | Best Practices

![Best Practices](/images/best_practice.jpeg)

## Introduction

<b>Android development</b> continues to dominate the world of mobile development.
 Fun projects, great pay, and tons of job prospects are just some of the reasons developers are starting their journeys into the exciting world of the Android operating system.
 Some [experts](https://proandroiddev.com/theres-never-been-a-better-time-to-learn-android-development-d1724409bdfc) say that there has never been a better time to learn Android skills, especially since the recent updates, like the addition of Kotlin and improvements to Google’s policies.

It’s been five years now that I’ve been into Android development and there has been no single day I haven't learned something new. But with these passing years, what I have realized is:
<br/><b>Just writing the code is not enough, Writing in an efficient way is the real challenge.</b>

### Tips and not Tricks

1) <b>Choose your App Architecture wisely based on your need, not just what the trend is.</b><br/><br/>
The architecture defines where the application performs its core functionality and how that functionality interacts with things like the database and the user interface.
<br/>We have many architectures like MVC, MVP, MVVM, MVI, Clean Architecture.
<br/>If any of these architectures is fulfilling your project requirements and you are following the standard coding guidelines and keeping your code clean, no architecture is bad.

2) Consider using <b>SVGs</b> or <b>WebPs</b> for your image drawables.<br/><br/>
Supporting multiple resolutions are a sometimes nightmare to developers. Including multiple images for different resolutions also increases the project size.
<br/>The solution is to use Vector Graphics such as SVG images or use WebP which can make a big difference in solving the image size problem by compressing lossless images.

3)  Choose your layout wisely and separate out the reusable XML and add it using [include](https://developer.android.com/training/improving-layouts/reusing-layouts) tag.<br/><br/>
  We have different layouts like [ConstraintLayout](https://developer.android.com/training/constraint-layout), [LinearLayout](https://developer.android.com/reference/android/widget/LinearLayout),
   [RelativeLayout](https://developer.android.com/guide/topics/ui/layout/relative), [FrameLayout](https://developer.android.com/reference/android/widget/FrameLayout),
    [CoordinatorLayout](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout). I did [performance](https://medium.com/1mgofficial/constraintlayout-vs-other-layouts-a-battle-towards-performance-part-1-14d8116e876e) analysis for some of them and found out that one should use layout based on their scenario/requirement only.
  <br/>Also, If you have some part of your XML getting reused in different layouts, extract it in a separate layout and use <include/> tag to avoid replication of code in different layouts.

4) Learn how to use [Build types](https://www.journaldev.com/21533/android-build-types-product-flavors), [Product Flavors](https://levelup.gitconnected.com/simple-guide-to-android-product-flavors-674106455038#:~:text=Simply%20put%2C%20a%20product%20flavor,app%20using%20a%20single%20codebase.) and [Build Variants](https://developer.android.com/studio/build/build-variants) and make most out of it for faster and easier development.<br/><br/>
####  Build Type
   Decides how our code will be compiled. For instance, If we want to sign our .apk with debug key, we put our debug configuration into debug build type. If we want to have obfuscated code when it is compiled and ready to release, we put that configuration on our release build type. If we want to log our HTTP request in debug mode but we want to disable it on release mode, we put that configuration on build types or call build types in library dependencies.

```
buildTypes
{
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        debug {
            applicationIdSuffix ".debug"
        }
}
```

Here is the simplest example:<br/>
When you run your app in debug mode, your application package name will be packagename.debug and if you run your app in release mode, your application package name will be packagename.

### Product Flavor<br/>
Let’s say we have are developing your app for your customer users. Everything is going fine for customer app. Then your product owner said that you need to develop that app for admin users. Admin user app should have all functionalities that customer app has. But also admin user can have access to statistics page and admin user should see the app in different colours and resources. And also your admin app’s analytics should not be mixed with customer app.What will you do? The answer is Product Flavor. Same app, different behaviour.

Edit your Gradle file:

```
android
 {
    ...
    defaultConfig {...}
    buildTypes {...}
    productFlavors {
        admin {
            ..
        }
        customer {
            ..
        }
    }
}
```

### Build Variants
Combines your build types and product flavors. Sync your project after you update your build.gradle. Then you will see all your build variants.

![Build Variants](/images/build_variants.png)

5) Learn & Use [Android Debug Bridge (ADB)](https://developer.android.com/studio/command-line/adb#:~:text=Android%20Debug%20Bridge%20(adb)%20is,of%20commands%20on%20a%20device.) to debug your application.<br/><br/>
Android Debug Bridge (ADB) is a versatile command-line tool that lets you communicate with a device.
It allows you to do things on an Android device that may not be suitable for everyday use, yet can greatly benefit your user or developer experience. For example, you can install apps outside of the Play Store, debug apps, access hidden features, and bring up a Unix shell so you can issue commands directly on the device.
ADB provides you with more details than your Android Studio Logcat. Just try it once and you can thank me later :-)

6) Configure your gradle.properties to increase your build speed.<br/><br/>
Long build times have always been a problem in the developer’s life.

```
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.configureondemand=true
android.enableBuildCache=true
org.gradle.jvmargs=-Xmx3072m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
org.gradle.caching= true
android.useAndroidX=true
android.enableJetifier=true
kapt.incremental.apt=true
kapt.use.worker.api=true
```

Showing you a sample of enhancements I did. You can read more about speeding up [here](https://developer.android.com/studio/build/optimize-your-build).

7) Keep a check on structural problems in your App code through [Lint](https://developer.android.com/studio/write/lint).<br/><br/>
The lint tool helps find poorly structured code that can impact the reliability and efficiency of your Android apps.
The command for MAC:
```
./gradlew lint
```
For Windows:
```
gradlew lint
```

8) Log everything in DEBUG mode only.<br/><br/>
We use logs to display useful information, errors, workflows or even to debug something.
<br/>But, Every information that we log can be a potential source of security issues! So make sure you remove before the code goes live.
<br/>And If you really want to keep these logs, you can either use [Timber](https://medium.com/mindorks/better-logging-in-android-using-timber-72e40cc2293d) library which can log your messages and gives you the control over the flow of logs or you can create your own [custom class](https://gist.github.com/niharika2810/eaf5b35b65b73883ef7714c57b3dffae) to print logs in debug mode.

9) Never add whole <b>third party library</b> if it’s possible for you to extract specific methods or small no. of classes for your functionality.<br/>
<br/>Just add those classes to your project and modify them accordingly.

10) Detect and Fix <b>memory leaks</b> in Android App time to time. <br/><br/>
“A small leak will sink a great ship.” — Benjamin Franklin
<br/>Use memory tools like [Leak canary](https://square.github.io/leakcanary/) to detect the cause for the memory leak.

Its knowledge of the internals of the Android Framework gives it a unique ability to narrow down the cause of each leak, helping developers dramatically reduce OutOfMemoryError crashes.

11) Handle [Configuration changes](https://developer.android.com/guide/topics/resources/runtime-changes) for your App.<br/><br/>
    Sometimes handling the orientation changes for your Activity, Fragment or AsyncTasks becomes most frustrating things to deal. If orientation changes are not handled properly then it results in unexpected behaviour of the application.
    When such changes occur, Android restarts the running Activity means it destroys and again created.
   <br/> There are different options to handle the orientation changes:<br/>
    1. Lock screen orientation<br/>
    2. Prevent Activity to recreated<br/>
    3. Save basic state<br/>
    4. Save complex objects<br/>

12) Perform validations in screens like input form on Client end only.<br/><br/>
Do we really need to hit backend for the <b>validations</b> like Is users email valid or Is user’s contact number is of the required length?
<br/>Consider this kind of cases and write your logic accordingly.

13) Don’t create references to activities that will prevent them from being <b>garbage collected</b> when they are done.<br/><br/>
Consider a very simple scenario — you need to register a local broadcast receiver in your activity. If you don’t unregister the broadcast receiver, then it still holds a reference to the activity, even if you close the activity.

How to <b>solve</b> this?<br/> Always remember to call unregister receiver in onStop() of the activity.

Like this, Find more scenarios and fix them.

14) Use <b>App Chooser</b> for your implicit intents and always handle NoActivityFound Exception.<br/><br/>
If multiple apps can respond to the intent and the user might want to use a different app each time, you should explicitly show a chooser dialog.
<br/>Also, if for some reason no app is available, your app shouldn’t crash. Please handle it through try /catch with showing some toast message to the user.

15) Put all your sensitive information in <b>gradle.properties</b> and never push it to your version control system.<br/><br/>
Don’t do this. This would appear in the version control system.


```
signingConfigs
 {
    release {
        // DON'T DO THIS!!
        storeFile file("myapp.keystore")
        storePassword "password123"
        keyAlias "thekey"
        keyPassword "password789"
    }
}
```

Instead, make a gradle.properties file :

```
KEYSTORE_PASSWORD=password123
KEY_PASSWORD=password789
```

That file is automatically imported by Gradle, so you can use it in build.gradle as such:

```
signingConfigs
{
    release
    {
        try
         {
            storeFile file("myapp.keystore")
            storePassword KEYSTORE_PASSWORD
            keyAlias "thekey"
            keyPassword KEY_PASSWORD
        }
        catch (ex) {
            throw new InvalidUserDataException("You should define KEYSTORE_PASSWORD and KEY_PASSWORD in gradle.properties.")
        }
    }
}
```

16) Implement SSL certificate pinning to prevent [Man-in-the-middle Attack (MITM)](https://ieeexplore.ieee.org/document/4768661/?part=1).<br/><br/>
To intercept any request, we mostly use a proxy tool. The proxy tool installs its own certificate on the device and application trust that certificate as a valid certificate and allow proxy tool to intercept application traffic. This way we can help hackers to tamper or know our data stuff.
<br/>With SSL Pinning implementation, the application does not trust custom certificates and does not allow proxy tools to intercept the traffic.
<br/>It’s a method that depends on server certificate verification on the client-side. Read more [here](https://www.nowsecure.com/blog/2017/06/15/certificate-pinning-for-android-and-ios-mobile-man-in-the-middle-attack-prevention/).

17) Use [SafetyNet Attestation API](https://developer.android.com/training/safetynet/attestation) to help determine whether your servers are interacting with your genuine app running on a genuine Android device.<br/><br/>
The API verifies the following:
1) Whether the device is rooted or not.<br/>
2) Whether the device is monitored.<br/>
3) Whether the device has recognized hardware parameters.<br/>
4) Whether the software is Android compatible.<br/>
5) Whether the device is free form malicious apps.<br/>
But before implementing it, please check the [Dos and Don’ts](https://developer.android.com/training/safetynet/attestation-checklist).

18) Use [EncryptedSharedPreferences](https://developer.android.com/reference/androidx/security/crypto/EncryptedSharedPreferences) instead of SharedPreferences for storing sensitive information like your auth tokens etc.<br/><br/>

19) Use the [Android Keystore system](https://medium.com/@josiassena/using-the-android-keystore-system-to-store-sensitive-information-3a56175a454b) to store and retrieve sensitive information from your storage like databases etc.<br/><br/>

The [Android keystore](https://developer.android.com/training/articles/keystore.html) provides a secure system level credential storage. With the keystore, an app can create a new Private/Public key pair, and use this to encrypt application secrets before saving it in the private storage folders.
While developing AarogyaSetu, I learnt about using Keystore for encrypting and decrypting highly sensitive information. You can check the implementation [here](https://github.com/nic-delhi/AarogyaSetu_Android).<br/>

>Note: We have an [Android Backup](https://developer.android.com/guide/topics/data/autobackup) mechanism which is turned on by default. Android preserves app data by uploading it to the user’s Google Drive — where it’s protected by the user’s Google Account credentials and can be easily downloaded for the same credentials on other devices.
>But, When you have encrypted something with Android Keystore, you won’t be able to restore it because the cypher key is only for that specific device. We don’t have a sync mechanism for Keystore like iOS and their keychain. A better way to backup your users’ data is to store them on your backend.

20) Call Google Play services methods to ensure that your app is running on a device that has the latest updates to protect against known vulnerabilities found in the default security provider.<br/><br/>
For example, a vulnerability was discovered in OpenSSL ([CVE-2014–0224](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-0224)) that can leave apps open to a “man-in-the-middle” attack that decrypts secure traffic without either side knowing. With Google Play services version 5.0, a fix is available, but apps must ensure that this fix is installed. By using the Google Play services methods, your app can ensure that it’s running on a device that’s secured against that attack.<br/>
To protect against these vulnerabilities, [Google Play services](https://developers.google.com/android/guides/overview) provides a way to automatically update a device’s security provider to protect against known exploits. Read more [here](https://developer.android.com/training/articles/security-gms-provider).

21) Implement [reCAPTCHA](https://developer.android.com/training/safetynet/recaptcha) to ensure your app is not automated i.e handled by a robot.<br/><br/>
reCAPTCHA is a free service that uses an advanced risk analysis engine to protect your app from spam and other abusive actions. If the service suspects that the user interacting with your app might be a bot instead of a human, it serves a CAPTCHA that a human must solve before your app can continue executing.

22) Write [Unit tests](https://developer.android.com/training/testing/unit-testing) for your feature.<br/><br/>
Listing out some benefits:<br/>
* Unit tests help to fix bugs early in the development cycle and save costs.<br/>
* It helps the developers to understand the code base and enables them to make changes quickly<br/>
* Good unit tests serve as project documentation.<br/>
* You become more confident about the stuff you are writing.<br/>
There are many more [benefits](https://www.seguetech.com/the-benefits-of-unit-testing/).:-)

23) Make <b>security decisions</b> on the server-side whenever possible.<br/><br/>
Don’t trust your application’s client-side. Hacker can easily tamper or hack your application’s codebase and can manipulate your code. So, it's better to have checks on the backend side whenever possible.

24) Learn how to use [Proguard](https://developer.android.com/studio/build/shrink-code) to the maximum for code Obfuscation and Optimization.<br/><br/>
It is quite easy to reverse engineer Android applications, so if you want to prevent this from happening, you should use ProGuard for its main function: obfuscation, a process of creating source code in a form that is hard for a human to understand(changing the name of classes and members).<br/>
ProGuard has also two other important functions: shrinking which eliminates unused code and is obviously highly useful and also optimization.
 <br/>Optimization operates with Java bytecode, though, and since Android runs on Dalvik bytecode which is converted from Java bytecode, some optimizations won’t work so well. So you should be careful there.

25) Use [Network security configuration](https://developer.android.com/training/articles/security-config) to improve your app’s network security.<br/><br/>
Security is more about layers of protection than a single iron wall. The Android Network Security Configuration feature provides a simple layer to protect apps from unintentionally transmitting sensitive data in unencrypted cleartext.<br/>
If you don’t know what “unencrypted communications” means, think of it this way — let’s say your office has the policy to send all shipments via UPS. A new intern joins the office and is tasked with shipping equipment to an office across the country.<br/> Oblivious to the policy and with all the best intentions, the intern sets up all shipments to be sent through an unknown, less expensive service.<br/> The Android Network Security Configuration feature is like the shipping/receiving manager who examines all inbound and outbound shipments and stops the shipment before the equipment gets into the hands of an unvetted delivery system. It can be used to prevent the accidental use of untrusted, unencrypted connections.
<br/>Read more [here](https://www.nowsecure.com/blog/2018/08/15/a-security-analysts-guide-to-network-security-configuration-in-android-p/).

26) Use [In-app review API](https://developer.android.com/guide/playcore/in-app-review) to give users the ability to leave a review from within the app, without heading back to the App Details page. For many developers, ratings and reviews are an important touchpoint with users. Millions of reviews are left on Google Play every day, offering developers valuable insight on what users love and what they want to be improved. Users also rely on ratings and reviews to help them decide which apps and games are right for them.

Over the past two years, Google Play has launched various features to make it easier for users to leave reviews, as well as for developers to interact and respond to them. For example, users are now able to leave reviews from the Google Play homepage. We also launched the Reviews page under My Apps & Games, which gives users a centralized place to leave and manage reviews.The API lets developers choose when to prompt users to write reviews within the app experience. We believe the best time to prompt your users is when they have used the app enough to be able to provide thorough and useful feedback. However, be sure not to interrupt them in the middle of a task or when their attention is needed, as the review flow will take over the action on the screen.

27) Introduce [Modularization](https://medium.com/google-developer-experts/modularizing-android-applications-9e2d18f244a0) in app.

Split your app module into different small modules, and give those modules as dependency to required modules like ```implementation project(":network-module")``` , you will get benefit of faster builds while developing your app and reusable code. Later you can extend this to provide dynamic delivery module.<br/><br/>
![Build Variants](/images/modularization.png)

<br/>Read more [here](https://medium.com/swlh/modularization-by-feature-and-layer-with-android-architecture-components-80bf317d737).

28) Avoid using too many base classes. Using base class everywhere creates a tight web in your code, which later makes it hard to refactor things. If still it's a necessity, create and use a standalone function (kotline file). 
Lets understand this with an example : 
You have 2 fragments namely ProfileFragment and HomeFragment, which extends from BaseFragment. BaseFragment has a function fetchPosts() in the onCreate() method, Now if in future you decide that ProfileFragment should not fetch posts when it is created, rather it should first show a dialogBox if the user is not logged-in. If the codebase is huge, you may have hardtime refactoring it. The other way around is to create a kotlin file as fun fetchPosts() and then use this function in your fragments in the onCreate() method or in a swipeRefresh() method (depending on your use case). Also one must note that a class can extend only one abstract base class.

 Read more [here](https://codeburst.io/inheritance-is-evil-stop-using-it-6c4f1caf5117), [here](https://dev.to/antonholmberg/the-baseclass-anti-pattern-16il), [here](https://proandroiddev.com/say-no-to-baseactivity-and-basefragment-83b156ed8998).

29) If you are using FirebaseFireStore in your application then don't directly write 'true' on the read and write rules to decrease security issues as it can help hackers to penetrate through your application.To clear this out lets see the default rules written in fireStore - 

<div align=center>
<h6>rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if false;
    }
  }
}</h6>
</div>
If we directly change false --> true it will no doubt work fine but its not secure because here acc to the rules everyperson has the permission to read and write the database so instead we use -->
<div align=center>
<h6>rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth!=null;
    }
  }
}</h6>
</div>
This will allow only logged users to write over db. For more about writing rules check this out : https://firebase.google.com/docs/rules 

<br>       

Read more [here](https://codeburst.io/inheritance-is-evil-stop-using-it-6c4f1caf5117) and [here](https://dev.to/antonholmberg/the-baseclass-anti-pattern-16il).
       
30) Create sourceSets for your main layout folder by adding following snippet in app level build.gradle file as follows

```
android {
    ...

    sourceSets {
        main {
           res.srcDirs = [
              'src/main/res',
              'src/main/res/layouts',
              file('src/main/res/layouts').listFiles()
           ]
        }
    }
}
```

Now you can separate out activity layouts, fragment layout and custom layouts in their respective folder as shown

![Build Variants](/images/layout_structure.png)

This will make navigating for layout files a lot easier and keep resources segregated.


#### The Critics principle<br/>
When you’re reviewing code of your teammates don’t be a friend, Be their arch enemy, don’t let them make mistakes that you might have to clean someday. Cleaning other’s shit will only make your hand dirty. Enforce good practices in code reviews.

Please feel free to contribute by raising a [PR](https://github.com/niharika2810/android-development-best-practices/compare) to add more. I will be happy to learn and share :-)

You can check the article [here](https://proandroiddev.com/android-development-best-practices-7278e9cdbbe9) which is originally published here on my [personal website](https://thedroidlady.com/2020-07-23-android-development-best-practices).

