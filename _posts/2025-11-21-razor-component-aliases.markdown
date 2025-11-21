---
layout: post
title:  "Razor Component Aliases"
date:   2025-11-21 21:35:04 +0100
categories: sbox
---

s&box has a bunch of built-in Razor components for creating user interfaces, like `TextEntry`, `Button`, `DropDown`, etc.

If you are used to working with HTML, you'll know about `input`, `button`, `select`, etc. These are the standard HTML tags for creating similar UI elements. Some of the most common of these HTML tags have aliases for their s&box Razor component counterparts, but they still work differently than using the Razor components directly.

## Image
```razor
<img src="image.png" />
```

This will work, because `src` is handled by the `Image` Razor component in `SetProperty`, but the `Image` component itself does not have a `src` property - it is only parsed for compatibility with HTML.

```razor
<Image src="image.png" />
```

This will not work, because the `Image` component does not have a `src` property. You need to use the `Texture` property instead and load the texture manually:

```razor
<Image Texture=@Texture.Load( "image.png" )" />
```

Using the alias is more convenient, but you lose some functionality by not being able to set the texture directly. I'm not sure if there's a way to make the alias support both `src` and `Texture` properties.

## Input
```razor
<input type="text" value="Hello" />
```

This will not work, because the `input` HTML tag does not map directly to the `TextEntry` Razor component, it would probably not work since input can have different types like `text`, `checkbox`, `radio`, etc. Instead, you should use the `TextEntry` component directly - read more about it in my previous post about <a href="{% post_url 2025-09-18-textentry %}">Text Entry</a>.

## Button
```razor
<button @onclick=@HandleClick>Click Me</button>
```

This will work, even though `button` does not map directly to the `Button` Razor component. The `@onclick` event works for any HTML element, and the Razor compiler doesn't care if it's a `button` or a `div` - you can put in any word you want, even gibberish, and it will still work as long as you handle the event properly.

There is a Button Razor component that you can use too, but personally I don't think I've ever used it, since the HTML `button` tag works just fine. It seems to have some extra properties.

## Select
```razor
<select>
    <option value="1">Option 1</option>
    <option value="2">Option 2</option>
</select>
```

This will kind of work, but not as expected. The `select` HTML tag *does* map to the `DropDown` Razor component, but the `option` tags inside it are not parsed as components, instead they are treated like render slots, which completely breaks the functionality of the dropdown.

To use a dropdown properly, you need to use the `DropDown` and its properties directly, which you can read more about in my previous post about <a href="{% post_url 2025-10-04-dropdown %}">DropDowns</a>.