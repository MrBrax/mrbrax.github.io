---
layout: post
title:  "Text Entry"
date:   2025-09-18 12:15:04 +0200
categories: sbox
---

If you've done HTML forms before, you might be familiar with the `<input>` element. In S&box, that's out the window since it doesn't have any logic behind it. Instead, you can use the `<TextEntry>` element, which is a S&box-specific component for text input.

```razor
<root>
    <TextEntry Value=@playerName Placeholder="Enter your name" />
    <button @onclick=@SubmitName>Submit</button>
</root>
```

But there's no binding here, so how do you get the value the user entered? You can use the `Value:bind` attribute to bind the value to a variable in your code.

```razor
<root>
    <TextEntry Value:bind=@playerName Placeholder="Enter your name" />
    <button @onclick=@SubmitName>Submit</button>
</root>

@code {
    private string playerName;

    private void SubmitName()
    {
        Log.Info($"Player name submitted: {playerName}");
    }
}

```

This way, whenever the user types something in the text entry, the `playerName` variable will be updated automatically.

TextEntry also has `OnTextEdited` and `onsubmit` events that you can use to handle text changes and form submissions. From my experience, these have to be prefixed with `@` to work properly.

```razor
<root>
    <TextEntry Value:bind=@playerName Placeholder="Enter your name" 
               @OnTextEdited=@HandleTextEdited
               @onsubmit=@HandleSubmit />
</root>

@code {
    private string playerName;

    private void HandleTextEdited(string newText)
    {
        Log.Info($"Text edited: {newText}");
    }

    private void HandleSubmit()
    {
        Log.Info($"Player name submitted: {playerName}");
    }
}
```

You have to give it your own styling, as the default style is basically non-existent.