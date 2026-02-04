---
layout: post
title:  "Camera Networking"
date:   2026-02-04 15:21:00 +0100
categories: sbox
---

**There should only be one camera active at a time.** If you have multiple cameras active, they will render at the same time, causing double the rendering workload, and if they have the same priority, either one may be chosen at random when rendering starts.

**A player should not have a camera as a child `GameObject`**, this causes it to replicate to everyone else when they spawn, which causes the issues above. *Oh!* But what if you set the network mode to `Never`? Well, that just means it won't replicate to the owning player either, so they won't see anything. (Unless you have logic to spawn the player from the owner's connection, but that's an advanced use case.)

Instead, one of the most optimal solutions I've found is to have a single camera `GameObject` in the scene, and have its network mode set to `Snapshot`. Its transform won't be synced, so any player that joins will just take "ownership" of the camera on their side.

There is a downside to the snapshot approach: if the camera has things like effects or post-processing that have a `Property` and are set locally by the host, those changes will be replicated to all players when they join. You'll have to remove or clear those effects from the joining player when they initially spawn.

You could also have a camera spawn per player locally, but make sure its network mode is set to `Never` - you could run this in `OnStart` after an `IsProxy` check. This way, each player has their own camera that doesn't replicate to anyone else. I'm using this approach in my game Braxnet RP.

If you want to switch between multiple cameras, you can manage that locally on each client. Just ensure that only one camera is enabled at a time, and set the others to disabled.