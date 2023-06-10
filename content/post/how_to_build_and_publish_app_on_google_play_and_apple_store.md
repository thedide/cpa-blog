---
title: "How to build and publish apps on Google Play and Apple App Store"
date: 2023-06-07T22:08:16-07:00
draft: false
categories:
  - Software Development
  - Technology
tags:
  - App development
  - React Native
  - Google Play
  - Apple App Store
  - Build package
---

This is part of a series of posts on building mobile apps with React Native
and AWS services. You can read the parent post
[here](https://www.comparepriceacross.com/post/building_phone_apps_with_react_native_and_amazon_services/).

# How to build and publish apps on Google Play and Apple App Store

## Enable USB debugging

In order to enable usb debugging for your Android and iOS devices you can follow these steps:

- On Android
  - Go to the "Settings" app on your Android device.
  - Scroll down and tap on "About phone" or "About device."
  - Locate the "Build number" or "Build version" option and tap on it seven times quickly. This action enables Developer options on your device.
  - Go back to the main "Settings" menu and find and tap on "Developer options."
  - Look for the "USB debugging" option and toggle it on.
  - A confirmation dialog will appear; select "OK" to enable USB debugging.
- On iOS
  - USB debugging is not available on iOS devices like iPhones and iPads. Instead, iOS provides a feature called "Developer mode" or "Developer options" which allows you to enable certain debugging and development features. However, it doesn't offer the same level of direct USB debugging as Android.
  - To enable Developer mode on iOS:
    - Connect your iOS device to your computer using a USB cable.
    - Launch Xcode on your Mac.
    - In Xcode, go to "Window" > "Devices and Simulators."
    - Select your connected iOS device from the list.
    - Click on the "Use for Development" button.
    - Follow the prompts to trust your computer on your iOS device.
    - Once the device is trusted, you can use Xcode and other development tools for debugging and - - - testing your iOS applications.

## Running on Device

React Native has a very useful [article about Running on Device](https://reactnative.dev/docs/running-on-device) that covers all the
basics.

## Creating a package

Before creating a package, make sure to test/debug your app throughly and ensure it works as expected.

- For Android
  - In Android Studio, select "Build" from the top menu, then choose "Generate Signed Bundle/APK."
  - You will have two package type options: APK and Android App Bundle (AAB). Either works. AAB is the newer format and size wise should be smaller that APK.
  - Follow the prompts to create a new signing key or use an existing one. **Make sure that you memorize the password you chose for your key.** You will need to enter your password any time that you create a new version of your package.
  - Build the signed APK/AAB, which will be ready for upload to Google Play.
- For iOS
  - For iOS things are slightly different. You will need an Apple developer account before you'd be able to build and publish your package. You can create one at [https://developer.apple.com](https://developer.apple.com).
  - In Xcode, go to the "Signing & Capabilities" tab for your target.
  - Ensure you have a valid Distribution Certificate and create an App Store Distribution Provisioning Profile.
  - Set the correct Bundle Identifier for your app.


## Publishing your package

- For Android
  - Go to the Google Play Console website [https://play.google.com/apps/publish/](https://play.google.com/apps/publish/) and sign in with your Google account.
  - Follow the on-screen instructions to set up a new developer account and provide the required information, including a one-time registration fee.
  - In the Google Play Console, click on "Create Application" and enter the default language and title for your app.
  - Fill in the store listing details, including app description, screenshots, icons, categorization, and any other required information.
  - Ensure that your store listing complies with the Google Play guidelines.
  - In the Google Play Console, navigate to the "App Releases" section.
  - Select the "Production" tab and click on "Create Release."
  - Choose the option to upload your APK file and follow the prompts to upload the signed APK generated earlier.
  - Fill in release notes and provide any other necessary information.
- For iOS
  - Open App Store Connect (https://appstoreconnect.apple.com) and sign in with your Apple Developer account.
  - Click on "My Apps" and then the "+" button to create a new app.
  - Fill in the required information, such as the app name, primary language, bundle ID, version number, and app icon.
  - Provide a compelling app description, keywords, screenshots, and other metadata required for the app listing.
  - In Xcode, go to the "Organizer" window.
  - Select your app's Archive and click on "Distribute App."
  - Choose the "App Store Connect" option and follow the prompts to upload your app to App Store Connect.
  - Provide the necessary details, including version information, pricing, and availability dates.  
  - Once your app is uploaded to App Store Connect, navigate to the "App Store" tab.
  - Review all the app details and ensure everything is accurate and complete.
  - Click on the "Submit for Review" button to submit your app for review by Apple's App Review team.

  <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
  <!-- cpa -->
  <ins class="adsbygoogle"
       style="display:block"
       data-ad-client="ca-pub-2843564932689995"
       data-ad-slot="3526097725"
       data-ad-format="auto"
       data-full-width-responsive="true"></ins>
  <script>
       (adsbygoogle = window.adsbygoogle || []).push({});
  </script>

## Prepare images for your app

Both Google Play and Apple App Store have certain mandatory requirements for the images associated with your app:

- Google Play
  - App Icon:
    - Dimensions: 512x512 pixels
    - Format: 32-bit PNG with alpha
    - No rounded corners or transparency
  - Feature Graphic (for the main header image):
    - Dimensions: 1024x500 pixels (landscape orientation)
    - Format: JPEG or 24-bit PNG (no alpha)
    - No text, buttons, or device frames
    - Should represent the app's core features and visual identity
  - Screenshots:
    - Dimensions: At least 320 pixels wide
    - Format: JPEG or 24-bit PNG (no alpha)
    - No device frames, status bar, or additional graphics
    - Should accurately represent the app's user interface and features
    - A maximum of 8 screenshots per type (phone, tablet, Android TV, Wear OS)
- Apple App Store
  - App Icon:
    - Dimensions: Varies (required sizes include 1024x1024 pixels for App Store Connect, and additional sizes for different devices)
    - Format: PNG with transparency
    - No rounded corners or transparency for App Store Connect icon
    - Follow Apple's Human Interface Guidelines for icon design and sizes for different devices
  - Screenshots:
    - Dimensions: Required sizes for different devices (iPhone, iPad, Apple Watch, etc.)
    - Format: JPEG or PNG
    - No additional graphics, device frames, or status bar
    - Show the app's actual user interface, features, and functionality
    - Up to 10 screenshots per device type and localization
  - App Preview Videos:
    - Format: H.264 and QuickTime (.mov)
    - Recommended resolution: 1080p (1920x1080 pixels) or 4K (3840x2160 pixels)
    - Up to 30 seconds in length
    - Demonstrate the app's core features and user experience

Do not under-estimate the amount of effort required to create these images. Seek professional help if needed. In my case, I created all images for my apps myself. If you need a free, yet powerful tool for your graphics requirements, I would strongly suggest using [Gimp](https://www.gimp.org/).
