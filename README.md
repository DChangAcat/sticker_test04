#  LoveQuest Stickers Apps for WhatsApp




## Sticker art and app requirements
We recommend you refer to the FAQ at https://faq.whatsapp.com/general/26000226 for complete details on how to properly create your sticker art. This FAQ also contains a sample PSD that demonstrates best practices for composing legible, rich sticker art.

* A sticker is an image that has a transparent background and can be sent in a
WhatsApp chat
* Stickers are organized into "packs". Your app can contain anywhere from 1 to
10 packs. Users must explicitly add each pack to WhatsApp one-by-one, so your
app should list each pack separately and each pack must have its own
affordance to add it to WhatsApp (do not try to create "add all packs"
operations).
* Each sticker pack must have a minimum of 3 stickers and a maximum of 30
stickers
* Stickers must be exactly 512 x 512 pixels
* Stickers will render on a variety of backgrounds, white, black, colored, patterned, etc. Test your sticker art on a variety of backgrounds. For this reason, we recommend you add a 8px #FFFFFF stroke to the outside of each sticker. See the sample PSD referenced at https://faq.whatsapp.com/general/26000226 for more details. 
* Stickers must be either in PNG or the [WebP format](https://developers.google.com/speed/webp). Currently, animated WebP, animated PNG, or animated stickers of any kind are not supported. See the section [Converting to WebP](#converting-to-webp) below for information on how to create WebP files.
* Each sticker must be less than 100KB. See the section [Tips for Reducing File Size](#tips-for-reducing-file-size) below. 
* Sticker Picker/Tray Icon
* Provide an image that will be used to represent your sticker pack in the WhatsApp sticker picker/tray 
* This image should be 96 x 96 pixels
* Max file size of 50KB

### Overview

* Drag your sticker images inside the `SampleStickers` folder in Xcode
* A window appears every time you drag an image into Xcode. Make sure that "Copy items if needed" and the `WAStickersThirdParty` target are checked.

* `name`: the sticker pack's name (128 characters max)
* `identifier`: The identifier should be unique and can be alphanumeric: a-z, A-Z, 0-9, and the following characters are also allowed "_", "-", "." and " ". The identifier should be less than 128 characters.
* Replace the `image_file` value with the file name of your sticker image. It must have both the file name and extension. The ordering of the files in the JSON will dictate the ordering of your stickers in your pack. 
* `ios_app_store_link` and `android_play_store_link` (optional fields): here you can put the URL to your sticker app in the App Store as well as a URL to your sticker app in the Google Play Store (if you have an Android version of your sticker app). If you provide these URLs, users who receive a sticker from your app in WhatsApp can tap on it to view your sticker app in the respective App Stores. To get your App Store link before you publish your app, refer to the instructions here: https://stackoverflow.com/questions/4137426/get-itunes-link-for-app-before-submitting.
* `emoji` (optional): add up to a maximum of three emoji for each sticker file. Select emoji that best describe or represent that sticker file. For example, if the sticker is portraying love, you may choose to add a heart emoji like 💕. If your sticker portrays pizza, you may want to add the pizza slice emoji 🍕. In the future, WhatsApp will support a search function for stickers and tagging your sticker files with emoji will enable that. The sticker picker/tray in WhatsApp today already categorizes stickers into emotion categories (love, happy, sad, and angry) and it does this based on the emoji you tag your stickers with. 

The following fields are optional: `ios_app_store_link`, `android_app_store_link`, `publisher_website`, `privacy_policy_website`, `license_agreement_website`, `emoji`

If your app has more than 1 sticker pack, you will need to reference it in `sticker_packs.wasticker`. Simply create a second array within the "sticker_packs" section of the file and include all the metadata (name, identifier, etc) along with all the references to the sticker files. 

### Required 
You must change the bundle identifier from the one provided. You cannot use anything that includes `WA.WAStickersThirdParty`. This is the only restriction on what you put as a bundle ID.

To change the bundle identifier:

* On the left sidebar in Xcode, select the very first item (unless you changed the project name, it should be `WAStickersThirdParty`)
* Change the field which says "Bundle Identifier"

To change the app name:

* On the left sidebar in Xcode, tap on the very first item (unless you changed the project name, it should be `WAStickersThirdParty`)
* Change the field which says "Display Name"

To change your app's icon:

* On the left sidebar in Xcode, tap on `Assets.xcassets`
* You'll see a lot of sections. The sections of our interest are "iPhone App" and "App Store" (required for publishing to the App Store).
* The 2x icon is 120x120 pixels and the 3x icon is 180x180. Just drag your images into the corresponding 2x or 3x bucket. If you have a 1024x1024 image for the App Store, drag it into the "App Store" bucket.

### Build the sample app
Make sure to run and test your sticker app. You can use the simulator to run the app, but you need to install your app on an actual iPhone to test and see if it works with WhatsApp. 

To prepare your app for submission to the App Store, you will to build an archive of your app. For more information, visit [https://help.apple.com/xcode/mac/current/#/devf37a1db04](https://help.apple.com/xcode/mac/current/#/devf37a1db04). 

## Submit your app
To submit your app to the App Store, you'll need to enroll as an Apple Developer at [https://developer.apple.com/programs/enroll/](https://developer.apple.com/programs/enroll/). 

Then, follow the instructions for publishing your app to the App Store: [https://help.apple.com/xcode/mac/current/#/dev442d7f2ca](https://help.apple.com/xcode/mac/current/#/dev442d7f2ca).

When preparing your app for submission in iTunes Connect, you'll have the option to add keywords associated with your app. WhatsApp can launch the App Store and perform a search for other sticker pack apps. To make sure that your app is shown in this list, add the keyword `WAStickers` when setting up your app in App Store connect. You can use additional keywords, but make sure you at least use this one.

## Advanced development
For advanced developers looking to make a more custom integration and fully control the UI of their sticker apps, follow the instructions below. We provide built-in helper classes and methods to easily create objects to represent your stickers and sticker pack, and to send them to the WhatsApp app.

### Files to use
Copy the following files from the sample app into your Xcode project:
* `Limits.swift`
* `StickerPackManager.swift`
* `StickerPack.swift`
* `Sticker.swift`
* `ImageData.swift`
* `Interoperability.swift`
* `WebPManager.swift`
* All of the files that have the "YY" prefix.

Please remember to create the Bridging Header file (if you don't have one already), and to add `#import "YYImage.h"`

You will also need to add the `WebP.framework` to your Linked Frameworks and Libraries.

### Create a sticker pack
To create a sticker pack to be sent to WhatsApp, instantiate a new `StickerPack` object:
```
let stickerPack = StickerPack(identifier: "identifier",
name: "sticker pack name", 
publisher: "sticker pack publisher", 
trayImageFileName: "tray image file name", 
publisherWebsite: "publisher website URL", 
privacyPolicyWebsite: "privacy policy website URL", 
licenseAgreementWebsite: "license agreement website URL")
```

Note: this method will throw an exception when any of the parameters don’t meet the requirements described in the sections above.

Alternatively, there is another initializer in `StickerPack` that accepts raw PNG image data instead of a file name for the sticker tray image.

### Add stickers to a sticker pack
Next, run this method for each sticker you want to add:
```
stickerPack.addSticker(contentsOfFile: "file name of sticker image", 
emojis: ["array of emojis"])
```

### Importing your sticker pack to WhatsApp
To import your sticker pack on WhatsApp, call the method below. This will open the WhatsApp app with a preview of your pack, and the user will be presented the option to save it on their sticker picker. 
```
stickerPack.sendToWhatsApp { completed in
// Called when the sticker pack has been wrapped in a form that WhatsApp 
// can read and WhatsApp is about to open.
}
```

### Structure of the JSON file that is sent to WhatsApp
If you don't want to use the API described above, you need to know how the data is sent and its structure. 

To communicate with WhatsApp, you must copy your sticker data into the pasteboard first. See [UIPasteboard](https://developer.apple.com/documentation/uikit/uipasteboard) for more. Then you need to open WhatsApp through the URL scheme `whatsapp://stickerPack`. WhatsApp will then grab your stickers from the pasteboard. 

In order for your app to to be able to check if it can open a URL using the `whatsapp://` scheme, you'll need to add the URL scheme in your `Info.plist` like this:

```
<key>LSApplicationQueriesSchemes</key>
	<array>
		<string>whatsapp</string>
	</array>
```

Format your sticker data into a JSON object with the structure described below. Then convert it into a Data object before putting it in the pasteboard.

```
{
  "ios_app_store_link" : "String",
  "android_play_store_link" : "String",
  "identifier" : "String",
  "name" : "String",
  "publisher" : "String",
  "tray_image" : "String", (Base64 representation of the PNG, not WebP, data of the tray image)
  "stickers" : [
    {
      "image_data" : "String", (Base64 representation of the WebP, not PNG, data of the sticker image)
      "emojis" : ["String", "String"] (Array of emoji strings. Maximum of 3 emoji)
    }
  ]
}
```

Important notes:
* `tray_image` uses PNG data whereas `image_data` uses WebP data.
* You can only send one sticker pack at a time
