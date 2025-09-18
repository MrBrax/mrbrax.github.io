---
layout: post
title:  "Static Local Player Instance"
date:   2025-09-18 12:08:04 +0200
categories: sbox
---

I think the approach of having an UI instance on each player is a messy idea. I prefer to use a root GameObject with all the UI stuff on it, and access the player via a static variable.


```cs
public class Player : Component
{

    public static Player LocalPlayer { get; private set; }

    public override void OnStart() {
        if ( !IsProxy ) {
            LocalPlayer = this;
        }
    }

    public override void OnDestroyed() {
        if ( LocalPlayer == this ) {
            LocalPlayer = null;
        }
    }

}
```

When the player is created, it checks if it's a proxy (remote player) or not (local player). If it's the local player, it assigns itself to the static variable. When the player is destroyed, it checks if it's the local player and sets the static variable to null.

This way, you can always access the local player instance from anywhere in your code using `Player.LocalPlayer`, without needing to have a separate UI instance for each player. Just make sure to handle the case where `LocalPlayer` might be null if there is no local player currently active or spawned.

You can do something like this in your UI code:

```razor
<root>
    <span class="name">@Player.LocalPlayer?.Network.Owner.DisplayName</span>
    <span class="health">Health: @Player.LocalPlayer?.Health</span>
</root>

@code {
    protected override int BuildHash() => System.HashCode.Combine(Player.LocalPlayer?.Network?.Owner?.DisplayName, Player.LocalPlayer?.Health);
}
```

This way, the UI will always display the correct information for the local player, and you don't have to worry about managing multiple UI instances or `[Property]` values.

Ideally, you would also want to add some error handling or fallback UI in case `LocalPlayer` is null, to avoid any potential null reference exceptions in your UI rendering.

```razor
<root>
    @if ( Player.LocalPlayer.IsValid() ) {
        <span class="name">@Player.LocalPlayer.Network.Owner.DisplayName</span>
        <span class="health">Health: @Player.LocalPlayer.Health</span>
    } else {
        <span class="name">No Player</span>
        <span class="health">Health: N/A</span>
    }
</root>