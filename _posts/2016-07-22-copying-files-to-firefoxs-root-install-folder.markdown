---
layout: post
title: Copying files to Firefox's root install folder
date: '2016-07-22 00:05:19'
tags:
- code
- mozilla
- windows10
---

###<i>Part 1 in [a series](http://blog.katiebroida.com/tag/windows10/) on improving Firefox's Windows 10 start menu tile</i>
<br>
I've been working on a pretty challenging bug, and I thought I'd share it with you all. The issue was how to improve Firefox's start menu tile for Windows 10. Currently on Windows 10, the start menu tile for Firefox uses just the default display settings. This means the tile image is small and the background color is an uninspiring grey for me. For others it is the default start menu color.

<div class="img-center">
    
![Old Firefox icon](/content/images/2016/07/old-firefox-icon.png)
<br>
<i>Not an engaging icon and looks hard to click</i>
</div>

Ideally the Firefox image should be larger and have a custom background color that matches Firefox's branding. 

<div class="img-center">
    
![Purdy](/content/images/2016/07/tile-colors-1.png)
<br>
<i>So pretty, so clickable</i>
</div>

To customize a Windows 10 start menu tile, you need to create an XML file named ExecutableName.VisualElementsManifest.xml that tells Windows what image and colors to use for your tile. With this file in the same directory as your executable file, Windows 10 will display your custom settings. It sounds straight forward enough, but there were a couple of roadblocks. 

## The challenge: 
### Editing the build file to copy my VisualElementsManifest file into the right directory

In order to get my XML file into the right directory, I needed to tell the program that builds Firefox that I wanted my VisualElementsManifest file to be copied into object directory that it generates and that I want it to be in its `dist/bin` folder. To unpack that last sentence a little bit, when I run `mach build` ([more info here](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/How_Mozilla_s_build_system_works)), Firefox's build program (mach) looks at Firefox's configuration and definition files to see what the developers wanted included in the Firefox program. Then it builds the program in the object directory according to these instructions. I needed to give instructions to mach to add my new file to the same folder as the firefox.exe file. 

If you take a look at Firefox's source code, you will find .mozbuild files scattered through out. These files are the ones where we say what files we want copied into our object directory. Since I was adding logos and my VisualElementsManifest file to `browser/branding/BRAND-NAME`, I told the .mozbuild file in `branding` that I want my new files to be copied into the object directory. You can see my changes below in the lines that begin with `+`:

```
@template
 def FirefoxBranding():
     JS_PREFERENCE_FILES += [
         'pref/firefox-branding.js',
     ]
 
     if CONFIG['MOZ_WIDGET_TOOLKIT'] == 'windows':
+        FINAL_TARGET_FILES['..'] += [
+            'firefox.VisualElementsManifest.xml',
+        ]
+        FINAL_TARGET_FILES.VisualElements += [
+            'VisualElements_150.png',
+            'VisualElements_70.png',
+        ]
         BRANDING_FILES += [
             'appname.bmp',
             'bgintro.bmp',
```

The key here is that I needed to use the `['..']` notation on `FINAL_TARGET_FILES` to specify that I wanted my files to be copied to `obj-dir/dist/bin`. Previously I used just `FINAL_TARGET_FILES`, and that copied my files to `obj-dir/dist/bin/browser`. This was because there was a variable in a [Makefile that was setting the `browser` folder's subdirectory to be `obj-dir/dist/bin/browser`.](https://dxr.mozilla.org/mozilla-central/source/browser/installer/Makefile.in#6) This variable is what was telling mach to copy all of my files into a subdirectory of `dist/bin`. By using `[..]` at the end of `FINAL_TARGET_FILES` I was able to override this local setting and just copy my files into `bin`. This was important for later because these files needed to be in the topmost directory of the installed program with firefox.exe

Once I added my files to the object directory, I needed to let Firefox's installer know that these files should be included in the final installed program folder. To do this, I made the following changes to `/browser/installer/package-manifest.in`:

```
+++ b/browser/installer/package-manifest.in
@@ -108,16 +108,20 @@
 @BINPATH@/@DLL_PREFIX@mozgtk@DLL_SUFFIX@
 @BINPATH@/gtk2/@DLL_PREFIX@mozgtk@DLL_SUFFIX@
 #endif
 
 [browser]
 ; [Base Browser Files]
 #ifndef XP_UNIX
 @BINPATH@/@MOZ_APP_NAME@.exe
+@BINPATH@/firefox.VisualElementsManifest.xml
+@BINPATH@/browser/VisualElements/VisualElements_150.png
+@BINPATH@/browser/VisualElements/VisualElements_70.png
 #else
 @BINPATH@/@MOZ_APP_NAME@-bin
 @BINPATH@/@MOZ_APP_NAME@
```

Adding my manifest xml file and images to package-manifest.in tells Firefox's installer to include these images in the installed version of Firefox with the same file hierarchy as in the object directory in relation to it's `bin` folder. This means that if my firefox.VisualElementsManifest.xml file is in `bin`, it will be copied in the root of my installed program's folder. Since my png images are in `/browser`, they will be copied into the root folder's `browser` folder. 

<i>Stay excited for part 2: what I actually put in those files</i>