---
layout: post
title:  "TimeSince & TimeUntil"
date:   2026-01-10 16:35:00 +0100
categories: sbox
---

TimeSince and TimeUntil are two very useful utility structs in s&box, and are already quite well known. However, I still want to highlight some of their features that might not be immediately obvious, as well as some common use cases for people new to s&box development.

Also to pay homage to how crazy useful these structs are, considering how little code is actually needed to implement them.

I often see people re-implementing similar functionality in their own code, not realizing that TimeSince and TimeUntil already exist and are optimized for this exact purpose:

```csharp
private float lastShotTime;

public void Shoot()
{
    if ( lastShotTime > 0 )
        return;

    // Perform shooting logic here
    lastShotTime = 1.0f; // 1 second cooldown
}

public void OnUpdate()
{
    if ( lastShotTime > 0 )
    {
        lastShotTime -= Time.Delta;
    }
}
```

To me at least, that code looks horrible compared to using TimeSince or TimeUntil, and it needs extra code to update the timer manually.

## TimeSince

[TimeSince Documentation](https://sbox.game/api/Sandbox.TimeSince)

TimeSince is a struct that represents the time elapsed since a certain event. It is commonly used to track cooldowns, delays, or intervals between actions. First seen in [Garry's Blog](https://garry.net/posts/timesince/).

Of note is that TimeSince is affected by time scaling, so if you change the scene time scale, TimeSince will reflect that change.

### Basic Usage

```csharp
private TimeSince timeSinceLastShot;

public void Shoot()
{
    if ( timeSinceLastShot < 1.0f ) // 1 second cooldown
        return;

    // Perform shooting logic here

    timeSinceLastShot = 0; // Reset the timer

    // do your shooting logic
}
```

You could call `Shoot()` every frame, but it will only actually shoot once per second because of the TimeSince check.

### Implicit Conversion

TimeSince can be implicitly converted to and from float values representing seconds. This makes it easy to work with time intervals.

```csharp
TimeSince timeSinceEvent = 2.5f; // 2.5 seconds since the event
float seconds = timeSinceEvent; // Implicitly converts to float
```

`TimeSince.Absolute` gives you the in-game absolute time when the TimeSince was last reset. Not the current date/time, but the time since the game/session started, based on `Time.Now`.

`TimeSince.Relative` gives you the time elapsed since the last reset, similar to just using the TimeSince directly.

### Common Use Cases
- **Cooldowns**: As shown in the basic usage example, you can use TimeSince to implement cooldowns for abilities or actions.
- **Delays**: You can use TimeSince to create delays between events, such as spawning enemies or triggering animations, by checking if a certain amount of time has passed inside an update loop.
- **Intervals**: TimeSince can be used to track intervals between repeated actions, like firing bullets or updating UI elements.

## TimeUntil

[TimeUntil Documentation](https://sbox.game/api/Sandbox.TimeUntil)

TimeUntil is a struct that represents the time remaining until a certain event occurs. It is useful for countdowns, timers, or scheduling future actions, where you want to be able to set a time on it instead of just measuring time since an event.

Of note is that TimeUntil is affected by time scaling, so if you change the scene time scale, TimeUntil will reflect that change.

### Basic Usage

```csharp
private TimeUntil timeUntilNextPunch;

private void Punch()
{
    if ( !timeUntilNextPunch ) // Still on cooldown - notice the implicit conversion to bool
        return;

    if ( hit a wall ) // Pseudocode
    {
        timeUntilNextPunch = 3.0f; // Set a 3 second cooldown because our hands hurt now
    } 
    else 
    {
        timeUntilNextPunch = 0.5f; // Shorter cooldown if we didn't hit anything
    }
}
```

You could call `Punch()` every frame, but it will only actually punch when the cooldown has expired. Notice how the implicit conversion to bool works here - it returns true if the TimeUntil has expired, and you can set it directly to a float value representing seconds until the event.

## RealTimeSince

[RealTimeSince Documentation](https://sbox.game/api/Sandbox.RealTimeSince)

Same as TimeSince, but uses real time instead of scaled time. This means that it is not affected by time scaling, and will always measure time based on real-world seconds.

## RealTimeUntil

[RealTimeUntil Documentation](https://sbox.game/api/Sandbox.RealTimeUntil)

Same as TimeUntil, but uses real time instead of scaled time. This means that it is not affected by time scaling, and will always measure time based on real-world seconds.