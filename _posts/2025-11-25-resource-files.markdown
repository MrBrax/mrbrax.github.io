---
layout: post
title:  "Resource Files"
date:   2025-11-25 00:17:55 +0200
categories: sbox
---

When you publish a game in s&box, compiled resources will automatically be included in the build. However, things like image files (PNG, JPG, etc.), sound files (WAV, MP3, etc.), and other non-compiled assets need to be explicitly added to "Resource Files" in your project settings to be included in the final build.

Go to **Project > Settings... > Other** and you'll find a section called **Resource Files**. Here, you can add any files or folders that you want to be packaged with your game.

![Resource Files](/assets/resource-files.png)

By "wildcards", it means you can use patterns like `*.png` to include all PNG files, or `images/*` to include all files in the "images" folder. This is useful for including multiple assets without having to add each one individually.

`images/*.png` will include all PNG files in the "images" folder.

After saving your settings, publish your game again, and the specified resource files should now be included in the build.