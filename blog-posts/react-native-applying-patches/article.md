---
published: true
title: "React-Native: CI friendly way to make changes to third-party packages"
cover_image: "https://raw.githubusercontent.com/waqqas/blog/master/blog-posts/react-native-applying-patches/assets/cover.jpg"
description: "Patch packages in node_modules that works great with CI tools"
tags: react-native, react, react.js, ci, node_modules, third-party, patch
series: react-native
canonical_url:
---

Have you been in a situation where you installed a package and it doesn't work with the latest version of react-native because the path of RN header file has changed? I ran into a similar problem and here's how I fixed it.

I was working on a react-native project and wanted to detect changes when the silent switch is flipped on or off. As usual, I searched and found a nifty package [`(react-native-silent-switch)`](https://github.com/gnestor/react-native-silent-switch) that did the job. I added that in my project as mentioned in its documentation and hit a snag. It gave an error during compilation due to wrong path in header file `RCTSilentSwitch.h`. Normally I would switch to some similar package which doesn't have this problem, but in this case I couldn't find a better alternative. (Maybe I should write one)

I could have modified the file but it has two problems. One, any other developer has to manually do those changes on hiss side, and, secondly, my CI build would fail. The build would fail because the file was part of a node packages this is installed via `package.json`.

I needed a way to modify `RCTSilentSwitch.h` AFTER it has been installed by `npm` and BEFORE it start compiling. My initial reaction was to use the `post-install` stage to patch the file, but I thought, there might be something out there for it too. So, I searched again and found another cool package that did just that. It was aptly named [`patch-package`](https://github.com/ds300/patch-package).

`patch-package` allows you to modify the code in `node_modules` directory, in the form of patches. The usage was quite simple too.

I followed the following steps to get my build working again:

1. Installed `patch-package` as dev dependencies by following the [setup instructions](https://github.com/ds300/patch-package#set-up)
2. I changed the required include path in `RCTSilentSwitch.h` and generated a patch file using the following command:

`yarn patch-package react-native-silent-switch`

A patch file was generated in `patches` directory. It's content is as follows

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

Although I used this approach for a react-native package, it can be used for any third-party package that is installed in `node_modules`
