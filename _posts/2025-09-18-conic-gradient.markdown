---
layout: post
title:  "Conic Gradient"
date:   2025-09-18 15:56:04 +0200
categories: sbox
---

Conic gradients are a type of gradient that radiates from a central point, creating a circular pattern. I use them in my game to visualize the cooldown of items in the inventory.

![Inventory slot conic gradient](/assets/conic-gradient.png)


```html
<div class="cooldown-bar">
    <div class="cooldown-bar-fill" style="background-image: conic-gradient(
        #0095ff 0%,
        #0095ff @(Slot.GetItem().GetCooldown() * 100f)%,
    transparent @(Slot.GetItem().GetCooldown() * 100f)%,
    transparent 100%
    )"></div>
</div>
````

GetCooldown()` returns a value between 0 and 1, which represents the percentage of the cooldown that has elapsed. I multiply it by 100 to convert it to a percentage for the CSS.


```scss
.cooldown-bar {
    position: absolute;
    top: 5px;
    left: 5px;

    width: 25px;
    height: 25px;
    background-color: rgba(0, 0, 0, 0.8);
    border-radius: 100%;

    .cooldown-bar-fill {
        flex-shrink: 0;
        width: 25px;
        height: 25px;
        border-radius: 100%;
    }

}
```

Just some simple CSS to position the bar in the top-left corner of the item slot, which has `position: relative;`.

Lots of things to tweak with this, especially the colors inside since it's just a gradient.