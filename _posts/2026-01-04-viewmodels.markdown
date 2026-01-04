---
layout: post
title:  "Viewmodels"
date:   2026-01-04 15:11:00 +0100
categories: sbox
---

Facepunch has some free to use viewmodels available in the cloud browser. They're separated into a weapon model and a hands/arms model.

Create a new prefab for your viewmodel - this is my preferred way to set up viewmodels, as you can easily tweak and test them in the editor before using them in code, adjusting fingers, positions, etc.

You can start by adding a `SkinnedModelRenderer` to your viewmodel prefab, and then assigning the weapon model to it. Make sure it's a viewmodel and not a world model, as the latter has no animations.

![SkinnedModelRenderer Weapon](/assets/viewmodel/2026-01-04%2016.06.02.png)

Next, you'll want to add another `SkinnedModelRenderer` for the hands/arms model. This one can be a separate GameObject next to the weapon GameObject. For this one, you want the `facepunch.v_first_person_arms_human` model.

![SkinnedModelRenderer Arms](/assets/viewmodel/2026-01-04%2016.06.15.png)

On the `SkinnedModelRenderer` for the arms, make sure to set the `Bone Merge Target` to the weapon's `SkinnedModelRenderer`. This will ensure that the arms animate correctly with the weapon.

![Bone Merge Target](/assets/viewmodel/2026-01-04%2016.05.44.png)

Spawning this prefab as a child of the camera will give you a basic viewmodel setup.

Accessing the `SkinnedModelRenderer` weapon component in code allows you to play animations on it. You use the `Set` method with the name of the animation you want to play. How you want to access the weapon and hands renderers is up to you - you could have a `ViewModel` component that holds references to them like I have.

```csharp
public class ViewModelComponent : Component
{
    [Property] public SkinnedModelRenderer Weapon { get; set; }
    [Property] public SkinnedModelRenderer Arms { get; set; }
}
```

```csharp
ViewModelComponent.Weapon.Set("b_attack", true);
```

The [official docs page on viewmodels](https://sbox.game/dev/doc/assets/ready-to-use-assets/first-person-weapons/) has more information from here on, like the available parameters. You can also find all the parameters if you open the `Parameters` group on the `SkinnedModelRenderer` component in the editor.

![Parameters Group](/assets/viewmodel/2026-01-04%2016.06.27.png)

