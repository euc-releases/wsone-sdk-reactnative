
# VMware Workspace ONE Software Development Kit (SDK)

## iOS And Android - Getting Started

This document explains how to integrate the Workspace ONE SDKs into your React-Native-built apps.

 For detailed information about the Workspace ONE SDK and managing internal apps, See the **VMware Workspace ONE UEM Mobile Application Management Guide** and the **VMware Workspace ONE SDK Technical Implementation Guides** located on the Workspace ONE Resources Portal at <https://my.workspaceone.com/products/Workspace-ONE-SDK>

## iOS Overview

In order to inject Workspace ONE SDK functionality into your  React-Native AWSDK App, integrate the two systems.

### Requirements

* iOS 12.0+
* Visual Studio Code  and Xcode 12.1
* Workspace ONE-enrolled iOS test device
* The Workspace ONE React-Native SDK  package from npm.
* A React-Native iOS app to integrate with the Workspace ONE SDK
** If you do not have a suitable application, you can create a new application and integrate the SDK into that.

### Add App to the Workspace ONE UEM Console

Upload your internal app to the Workspace ONE UEM Console to register it with the system. This step enables UEM Console to identify the app and to add functionality to it. The **Workspace ONE UEM MAM Guide** details how to upload an internal app.

1. In React-Native, export the app as a signed IPA.
2. Log into the Workspace ONE UEM Console as an administrator.
3. Navigate to **Apps & Books > Applications > List View > Internal** and then choose **Add Application**.
4. Select **Upload > Local File**, add the IPA file, and select **Continue**.
5. Select **More** and choose **SDK**.
6. Select the **iOS Default Settings** profile in the *SDK Profile* field.
7. Select **Save and Assign** to continue to the **Assignment** page.
8. Assign the app to a smart group and select a **Push Mode**.
9. Select **Add**, and **Save & Publish** the app to complete the upload process.

### Enable Communication Between the Intelligent Hub(formerly AirWatch Agent) and the React-Native IPA File

Expose a custom scheme in the Info.plist file in the React-Native project to enable the app to receive a call back from the Intelligent Hub. Your app receives communications from the Workspace ONE UEM Console through the Intelligent Hub. To expose the scheme, add a callback scheme registration and add a query scheme to your React-Native project.

#### Add Callback Scheme Registration

1. In Xcode, navigate to Supporting Files.
2. Select the file <YourAppName> -Info.plist.
3. Navigate to the URL Types section.
4. If it does not exist, add it at the Information Property List root node of the PLIST.
5. In the **URL Types** section, choose the **Add URL Type** button.
6. Set the values of *Identifier* and *URL Schemes* to the desired callback scheme.
7. Set the *Role* to **Editor**.
8. Configure trust for all Workspace ONE UEM anchor application schemes under the LSApplicationQueriesSchemes entry in the Information Property List.
9. Within the *Array*,  Add following 3 values for  anchor application.
   * Type String  and Value **airwatch**.
   * Type String  and Value  **awws1enroll**.
   * Type String  and Value   **wsonesdk**.

### Add Support for QR Scan and FaceId

#### QR Scan

Include NSCameraUsageDescription in the application info.plist file to enable the SDK to scan QR codes with the device camera.
Provide a description that devices prompt users to allow the application to enable this feature.

#### FaceID

Include NSFaceIDUsageDescription in the application info.plist file to enable the SDK to use FaceID.
Provide a description that devices prompt users to allow the application to enable this feature. Consider controlling the message users read. If you do not include a description, the iOS system prompts users with native messages that might not align with the capabilities of the application.

## Android Overview

To integrate Workspace ONE Android SDK React-Native components into an existing React-Native Android app follow described steps.

### Requirements

* Visual Studio Code  and Android Studio
* The Workspace ONE React-Native SDK  package from npm
* Android test device running Lollipop and above.
* Intelligent Hub for Android from Google Playstore.
* Whitelisted Release/Debug signing key as explained below should be used for signing the React-Native android application.

### Whitelist Signing Key

Before you can begin using the **Workspace ONE SDK**, you must ensure your application signing key is whitelisted with your Workspace ONE UEM Console. When your SDK-integrated application starts up, the SDK checks the signing public key with which it is signed. It compared againt the list of whitelisted apps to determine whether your application is trusted.

Workspace ONE allows whitelisting for both apps deployed internally or deployed through a public app store.

#### Internally Deployed Applications

1. After building the application apk, sign it using your own specific app signing key.
2. Upload the signed apk file to the Workspace ONE UEM Console as described below. Workspace ONE UEM Console extracts the application's public signing key and adds it to the whitelisted apps list
    a) Log into the Workspace ONE UEM Console as an administrator.
    b) Navigate to **Apps & Books > Applications > List View > Internal** and then choose *Add Application*.
    c) Select *Upload > Local File*, add the APK file, and select Continue.
    d) Select **More** and choose *SDK*.
    e) Select the *Android Default Settings* profile in the SDK Profile field.
    f) Select *Save and Assign* to continue to the *Assignment* page.
    g) Assign the app to a smart group and select a *Push Mode*.
    h) Select *Add*, and *Save & Publish* the app to complete the upload process.

#### Publicly Deployed Applications

For applications that are deployed publicly through the Play Store, send the public signing key of the application to AirWatch for whitelisting.

**Note: Contact your professional services representative for the process of whitelisting the public signing key.**

### Push App to Dev Device using App Catalog

In order for the **Intelligent Hub** to manage an app, it needs to be sent to the device.  This can be done via an installation policy of **Automatic** or by pushing the app down once using the **Hub**'s *APP CATALOG*.  Once the app is listed in the *Managed Apps* section of the Hub, it is ready for local management.