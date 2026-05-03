---
layout: post
title:  "Scene.Trace.Ray: Direction vs. End Position"
date:   2026-05-03 12:00:00 +0100
categories: sbox
---

A common gotcha when working with `Scene.Trace.Ray` is misunderstanding what the second `Vector3` argument actually represents.

The signature looks like this:

```csharp
Scene.Trace.Ray( Vector3 from, Vector3 to )
```

Both arguments are **absolute world positions**. `from` is where the ray starts, and `to` is where the ray ends.

## The Mistake

It's easy to assume the second argument is a direction, and write something like this:

```csharp
var tr = Scene.Trace.Ray( Scene.Camera.WorldPosition, Scene.Camera.WorldRotation.Forward * 1000f )
    .Run();
```

`Scene.Camera.WorldRotation.Forward * 1000f` is not a world position - it's a direction vector scaled to a length of 1000 units relative to the world origin. This means the ray will trace from the camera's world position *to a point 1000 units in front of the world origin*, which is almost certainly not what you want.

Depending on where your camera is, this can produce results that appear to work near the origin but break entirely when the camera moves further away.

To further confuse this, a float implicitly converts to a `Vector3` with that value in all components, so if you did this:

```csharp
var tr = Scene.Trace.Ray( Scene.Camera.WorldPosition, 1000f )
    .Run();
```

You would get a ray from the camera's world position to the point (1000, 1000, 1000) in world space, which is almost certainly not what you intended.

## The Fix

Add the direction offset to the starting position so the result is an absolute world position:

```csharp
var tr = Scene.Trace.Ray( Scene.Camera.WorldPosition, Scene.Camera.WorldPosition + Scene.Camera.WorldRotation.Forward * 1000f )
    .Run();
```

Now the ray traces from the camera position to a point 1000 units directly in front of it in world space, regardless of where the camera actually is.

## Quick Reference

| Second argument | What it means |
|---|---|
| `Scene.Camera.WorldRotation.Forward * 1000f` | Point 1000 units from the **world origin** — wrong |
| `Scene.Camera.WorldPosition + Scene.Camera.WorldRotation.Forward * 1000f` | Point 1000 units in front of the **camera** — correct |

The same rule applies to any `Scene.Trace.Ray` call — always pass an end *position*, never a bare direction.

You can also use the `Ray` struct to construct rays with a direction and length, which can be more convenient:

```csharp
var ray = new Ray( Scene.Camera.WorldPosition, Scene.Camera.WorldRotation.Forward );
var tr = Scene.Trace.Ray( ray, 1000f )
    .Run();
```