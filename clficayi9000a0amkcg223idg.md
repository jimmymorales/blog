---
title: "How to Make Your Android App Smaller"
seoTitle: "What's the Ideal Android App Size? Tips to Reduce Your App Size"
seoDescription: "Learn how to reduce your Android app size to improve user experience and increase downloads. Follow our tips to optimize your app's size efficiency."
datePublished: Tue Mar 21 2023 14:17:18 GMT+0000 (Coordinated Universal Time)
cuid: clficayi9000a0amkcg223idg
slug: android-app-size
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/98MbUldcDJY/upload/f3467436242fd20cb09dcaa926ff4237.jpeg
tags: android-development, android

---

Have you ever gone on a diet and struggled to shed those stubborn pounds? It's a familiar challenge for many of us, and one that Android developers can relate to when it comes to reducing the size of their apps. Just like losing weight, trimming down an Android app can be a daunting task, but the benefits are worth it: faster downloads, improved user experience, and better app store optimization. In this post, we'll explore some proven techniques for shedding those extra app pounds and getting your app in shape.

---

# Why should we care?

App size is an important metric that [directly correlates with business metrics](https://medium.com/googleplaydev/shrinking-apks-growing-installs-5d3fcba23ce2). App size is one of the first metrics a user sees when they want to download the app from the store, and it can make the user decide to not install the app at all if the size is too large. On Android, it could be a bigger deal because of all the devices that are out there some of them have low disk space like for example some Fire Tablets and OTT devices.

Depending on the context of your application, you should be concerned about the size of their application for a few reasons:

1. **User experience**: Users may be deterred from downloading or using an app if it takes up too much storage space on their device. A large app can also slow down a user's device, which can result in a poor user experience.
    
2. **App Store Optimization** (ASO): The size of an app can affect its ASO, which is a critical factor for discoverability and download rates. If an app is too large, it may not rank as highly in the app store as smaller, more efficient apps.
    
3. **Bandwidth and storage costs**: Large app sizes can also result in higher bandwidth and storage costs for users, which can be especially problematic in areas with limited connectivity or data plans.
    

Therefore, reducing the size of an Android application is important for both the user experience and the success of the app.

## How to measure?

Depending on the packaging format your using for your app, the size of an Android application can be measured in different ways:

* APK: Using the [Analyze APK feature](https://developer.android.com/studio/debug/apk-analyzer#view_file_and_size_information) of Android Studio or by seeing [the statistics in the Play Store.](https://support.google.com/googleplay/android-developer/answer/9859372?hl=en)
    
* AAB: Using the [bundletool](https://developer.android.com/studio/command-line/bundletool#measure_size) or by seeing [the statistics in the Play Store.](https://support.google.com/googleplay/android-developer/answer/9859372?hl=en)
    

## APK Analyser

Here are some steps you can follow to use the Analyze APK feature of Android Studio to measure the size of an app:

1. Open Android Studio and open your project.
    
2. Build your app in release mode by selecting "Build &gt; Generate Signed Bundle / APK..." and then selecting "APK".
    
3. After your app is built, go to the "Build" menu and select "Analyze APK...".
    
4. Browse to the directory where your APK file is located and select it.
    
5. Android Studio will display a summary of the contents of the APK file, including the size of each file and the total size of the APK.
    

## Bundletool

If you want to measure the size of your app as a bundle, you can use the [bundletool](https://developer.android.com/studio/command-line/bundletool#measure_size) command-line tool.

1. First, build an app bundle by selecting "Build &gt; Generate Signed Bundle / APK..." and then selecting "Android App Bundle". This will generate a .aab file in the specified output directory.
    
2. Open a command prompt or terminal window and navigate to the directory where you installed the [bundletool](https://developer.android.com/studio/command-line/bundletool#measure_size).
    
3. Run the following command to measure the size of your app as a bundle:
    
    ```bash
    bundletool size --bundle=/path/to/your/app.aab
    ```
    
    This will display a summary of the size of your app's APKs, including the total size of the APKs and the size of each split APK.
    

Ideally, you would want to have an automated way of measuring the released application size and be able to compare it with previous versions, triggering alerts when the difference is considerable.

## Recommendations

### Android App Bundles

[Android App Bundle](https://developer.android.com/guide/app-bundle) is a publishing format introduced by Google in 2018 that allows developers to optimize and streamline their app's distribution by delivering optimized APKs to users based on their device configuration. Essentially, it allows developers to split their apps into smaller, more manageable parts, which can significantly reduce the app size.

Here's how it works:

1. When developers create an app bundle, they provide the app's compiled code, resources, and other assets to Google Play.
    
2. Google Play then uses the app bundle to generate and sign optimized APKs for each user's specific device configuration, based on factors such as the device's language, screen size, and architecture.
    
3. When a user downloads the app, Google Play delivers the optimized APK that's best suited for their device, resulting in a smaller download size and faster installation time.
    

According to Google, Android App Bundle users saw an average download size savings of 20% compared to traditional APKs in 2020. Developers who have adopted Android App Bundle have reported significant improvements in app performance and user experience. For example, dating app Hily saw a 3x increase in the number of daily registrations and a 7% decrease in uninstall rates after adopting Android App Bundle.

*Note: new applications that are submitted to the Play Store need to use the AAB format. As of June 3, 2022, developers can now submit apps using the App Bundles format to the Amazon Appstore.*

## R8

R8 is a code shrinking and obfuscation tool that is commonly used in Android development to reduce the size of an app's code and to make it more difficult to reverse engineer the app's code.

R8 was introduced by Google in Android Studio 3.4 as a replacement for Proguard. It is designed to optimize and reduce the size of the app's code by removing unused code, shrinking code size, and optimizing the remaining code.

Enabling R8 in an Android app is quite simple, and can be done by following these steps:

1. Open the app's `build.gradle` file, which is typically located in the app module of your Android Studio project.
    
2. Locate the "buildTypes" section of the `build.gradle` file.
    
3. For each build type (e.g. debug, release), add the following lines of code:
    
    ```plaintext
    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    ```
    
    In this example, we're enabling code shrinking and obfuscation for the `release` build type by setting `minifyEnabled` to true and specifying the location of the ProGuard rules file.
    
4. Save the changes to the `build.gradle` file, and rebuild the app.
    

When enabling R8, watch out for runtime exceptions that occurred when the app's code uses reflection, this is common for example in some "networking" libraries [like Gson](https://github.com/google/gson/tree/master/examples/android-proguard-example). You will need to add proper rules to the `proguard-rules.pro` file to tell R8 to don't change the name of a class or its properties, since they will be accessed in runtime using reflection.

## Shrink resources

There are many ways to shrink resources, so let's start with the more obvious one which is removing the unused ones.

### Remove unused resources

Unused resources, such as images or strings, can be removed from the app to reduce its size. Android Studio includes a tool called "Resource Manager" that can help identify unused resources and remove them from the project.

You can also use Android Lint and look for the `UnusedResources` warning. You run `./gradlew lintDebug`. This will generate an HTML file with more information. Check for the warnings that have an id of *UnusedResources.*

*Note: When deleting a file, double-check that it is really not being used and not a false-positive from Lint. It could be the case that it is used by other variants like the release one. If that is the case move the asset to a more specific source set if needed. For example, if the file is inside the main folder, but is only used by AndroidTV and FireTV, you can move it to the tv folder instead.*

There is also the `shrinkResources` option in an Android Gradle file which is a flag that tells the build system to remove unused resources from the compiled APK file. When this option is enabled, the build system will analyze the app's resources and remove any resources that are not referenced in the code or in any other resource file.

It's worth noting that enabling the `shrinkResources` option may result in longer build times, as the build system needs to analyze the app's resources to determine which ones can be safely removed, so it's recommended to only enable it for the release builds.

### Compress images

Images can take up a lot of space in an Android app, so it's important to optimize them to reduce their size. Tools like Photoshop or online image compressors can be used to reduce the size of images without sacrificing too much quality.

Converting images to the WebP format can be an effective way to reduce the size of an Android app. WebP is a modern image format developed by Google that provides better compression than JPEG and PNG, while maintaining good image quality. WebP images can be up to 30% smaller in size than comparable JPEG or PNG images, resulting in a smaller overall app size.

Vector graphics are smaller in size than raster graphics (like WebP, JPEG or PNG), and can be easily scaled without losing quality. By using vector graphics instead of raster graphics, developers can reduce the overall size of the app as well. However, it takes a significant amount of time for the system to render each VectorDrawable object, and larger images take even longer to appear on the screen. Therefore, consider using these vector graphics only when displaying small images or icons.

Audio and video files can also take up a lot of space in an Android app. Use compressed video formats like H.264 or VP9 instead of uncompressed formats. Also, consider reducing the frame rate and resolution of videos to the appropriate size for your app. You can use tools like [FFmpeg](https://ffmpeg.org/) or [HandBrake](https://handbrake.fr/) to optimize your videos.

### Specify resource configurations

The `resConfig` option in an Android Gradle file is used to specify which resource configurations should be included in the final APK/AAB file. By default, the Android build system includes all resource configurations, which can result in larger APK files.

The `resConfig` option allows developers to specify which resource configurations should be included, and which should be excluded. This can be useful for reducing the size of the APK file, especially if the app supports many different configurations (such as different languages or screen sizes) and only a subset of those configurations are commonly used.

For example, if an app supports multiple languages, but the majority of its users only use one or two languages, the developer could use the "resConfig" option to exclude the unused language configurations from the final APK file. This can significantly reduce the size of the APK file, making it faster and easier to download and install.

Here's an example of how the "resConfig" option can be used in an Android Gradle file:

```plaintext
android {
    // Only include the English and Spanish language configurations
    resConfig "en", "es"
}
```

In this example, only the English and Spanish language configurations will be included in the final APK file, and any other language configurations will be excluded.

---

Overall, this blog post provides a useful guide for developers looking to optimize the size of their Android applications. By following the tips and techniques outlined in this post, you can improve the performance of your apps and provide a better user experience to your users.

*Thanks for reading! Please, leave a comment with any feedback or question.*