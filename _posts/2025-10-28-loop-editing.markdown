---
layout: post
title:  "Loop Editing"
date:   2025-10-28 21:35:04 +0100
categories: sbox
---

**Note: Untested code ahead, please report any issues. Fairly unfinished post.**

If you loop through a list of items in a UI component, you might want to edit the items in the loop, like a list of player attributes with editable fields.

```razor
@foreach ( var player in Players )
{
    <div class="player-item">
        <TextEntry Value:bind=@player.Name />
    </div>
}
```

This will not work as expected, because the `TextEntry` component will not be able to bind to the `player.Name` property correctly - probably because of how loop scope works.

You'd think that using a temporary variable would help, but it doesn't:

```razor
@foreach ( var player in Players )
{
    var playerName = player.Name;
    <div class="player-item">
        <TextEntry Value:bind=@playerName />
    </div>
}
```

Probably because `playerName` is reset when the UI re-renders.

The solution that I found to work is to create a dedicated component for the player item, and pass the player object as a parameter:

```razor
<PlayerItem Player=@player />
```

Then, in the `PlayerItem.razor` component, you can bind directly to the player's properties:

```razor
@using Sandbox;
@using Sandbox.UI;
@inherits Panel
@namespace Ui

<root>
    <TextEntry Value:bind=@Player.Name />
</root>

@code {
    [Parameter] public Player Player { get; set; }
}
```

This way, each `PlayerItem` component has its own scope and can bind to the player's properties correctly. It also makes it easier to manage the UI for each player item separately, like showing/hiding a pencil icon when editing, or adding validation.