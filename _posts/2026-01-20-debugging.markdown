---
layout: post
title:  "Debugging"
date:   2026-01-20 11:00:00 +0100
categories: sbox
---

Debugging is an essential part of game development, but s&box doesn't play too well with traditional debugging tools like Visual Studio's debugger.
This is mainly due to the way s&box loads and runs code at runtime, which can interfere with breakpoints and step-through debugging, since the generated code can differ from the source.

Here's what we've found so far to help with debugging s&box projects.

## Logging

The true classic way to debug code is to use logging statements, it's very crass but it just works. Unless you're trying to debug something that happens very frequently (like every frame).

```csharp
Log.Info( "This is a debug message" );
Log.Warning( "This is a warning message" );
Log.Error( "This is an error message" );
```

You can also use string interpolation to include variable values in your log messages:

```csharp
int playerHealth = 100;
Log.Info( $"Player health is {playerHealth}" );
```

You can log objects as well, which will call their `ToString()` method and make a clickable link in the output to inspect the object:

```csharp
class PlayerData
{
    public string Name { get; set; }
    public int Score { get; set; }

    public override string ToString()
    {
        return $"PlayerData(Name: {Name}, Score: {Score})";
    }
}
PlayerData player = new PlayerData { Name = "JohnDoe", Score = 42 };
Log.Info( player );
// Outputs: PlayerData(Name: JohnDoe, Score: 42)
```

## Debug Overlay

s&box provides a class of draw functions that can be used to visualize things in the game world.

```csharp
DebugOverlay.Line( startPosition, endPosition, duration: 5.0f, color: Color.Red );
DebugOverlay.Sphere( new Sphere( center, radius ), duration: 5.0f, color: Color.Green );
DebugOverlay.Text( position, "Hello World", duration: 5.0f, color: Color.White );
```

Main documentation for DebugOverlay can be found [here](https://sbox.game/api/Sandbox.DebugOverlaySystem).

## IDE Debugging

Setting breakpoints and stepping through code with an IDE debugger is not very reliable in s&box at the moment, but there are some tips that can help improve the experience:

Attach the debugger like normal, but when making breakpoints, try changing this setting in Visual Studio:
- Allow source code to be different from the original
Found by right-clicking on the breakpoint and selecting "Conditions..." then clicking on the "Location" text where it says "Must match source".

*I currently do not know of a way in Rider to do the same, but if you find out please let me know!*

At least what does work reliably is to catch exceptions and view the call stack/local variables at that point.

## Profiling

For performance profiling, s&box has a built-in profiler that can be accessed via the in-game console. You can use the `profile` command to start and stop profiling sessions, which will generate a link to the Mozilla Profiler for detailed analysis.

You can also use tools like Superluminal Profiler or Visual Studio's Performance Profiler to analyze CPU and GPU usage, memory allocation, and more.

