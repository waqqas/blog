---
published: false
title: "React-Native: Patching third-party code"
cover_image: "https://raw.githubusercontent.com/waqqas/blog/master/blog-posts/react-native-applying-patches/assets/patches.png"
description: ""
tags: react-native, react, react.js, ci
series: react-native
canonical_url:
---

I was working on a react-native project and wanted to detect changes when the silent switch is flipped on. As usual, I searched and found a nifty package [`(react-native-silent-switch)`](https://github.com/gnestor/react-native-silent-switch) that did the job. I added that in my project as mentioned in its documentation and hit a snag. It gave an error during compilation due to missing react-native include directory. I could add that in the xcode project settings for the package and get my project to compile, but, my project was setup with continuous integration. It would fail there, as the xcode project settings file (`RCTSilentSwitch.xcodeproj`) was in `node_modules` directory, which is populated from `package.json`

I needed a way to modify `RCTSilentSwitch.xcodeproj` AFTER it has been pulled by npm. I searched again and found another cool package that did just that, that was aptly named [`patch-package`](https://github.com/ds300/patch-package).

`patch-package` allows you to modify the code in `node_modules` directory, in the form of patches. The usage was quite simple too.

I followed the following steps to get my build working again:

1. Installed `patch-package` as dev dependencies by following the [setup instructions](https://github.com/ds300/patch-package#set-up)
2. I added the required include path in `RCTSilentSwitch.xcodeproj` and generated a patch file using the following command:

`yarn patch-package react-native-silent-switch`

A patch file was generated in ``. It's content is as follows

```
diff --git a/node_modules/react-native-silent-switch/RCTSilentSwitch.h b/node_modules/react-native-silent-switch/RCTSilentSwitch.h
index 61c955f..0adef09 100644
--- a/node_modules/react-native-silent-switch/RCTSilentSwitch.h
+++ b/node_modules/react-native-silent-switch/RCTSilentSwitch.h
@@ -1,4 +1,4 @@
-#import "RCTBridgeModule.h"
+#import "React/RCTBridgeModule.h"
 #import "SharkfoodMuteSwitchDetector.h"

 @interface RCTSilentSwitch : NSObject <RCTBridgeModule>
```

3. I removed the `node_modules` directory and ran `yarn install` and confirmed that the patch was applied. It printed the following messages at the end.

```
$ patch-package
patch-package 6.1.2
Applying patches...
react-native-silent-switch@0.1.0 ✔
```

## Conclusion

Using `patch-package`, I was able to make small changes in the `node_modules` directory in a way that is CI friendly.
