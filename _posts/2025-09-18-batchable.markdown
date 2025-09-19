---
layout: post
title:  "Batchable"
date:   2025-09-19 11:10:00 +0200
categories: sbox
---

This is something that comes up in the Discord server from time to time.

When you have a model that has a material with render attributes for custom shaders or dynamic expressions, having multiple instances of that model in the scene and changing the attributes on one of them will cause all of them to update, because they are set to be batchable by default.

At the moment of writing this, settings things in `Attributes` on the model renderer won't disable batching, so you have to set `Batchable` to `false` on the `SceneObject` itself.

```csharp
ModelRenderer.SceneObject.Batchable = false;
```