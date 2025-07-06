# NUI Callbacks

## 🔁 NUI Callbacks in FiveM

NUI callbacks allow your **browser-based UI (NUI)** to communicate back to your **Lua/JS/C# game logic**. These are essential for building dynamic menus, stores, inventories, and more.

***

### 🎯 What Is a NUI Callback?

A NUI callback is a **POST request** made from the browser UI that gets picked up by a handler in your game script. The game responds using a **callback function** (`cb`) with a result.

***

### 🧠 Lua: Registering a Callback

Use `RegisterNUICallback()` to handle NUI events.

```lua
RegisterNUICallback('getItemInfo', function(data, cb)
    local itemId = data.itemId

    if not itemCache[itemId] then
        cb({ error = 'No such item!' })
        return
    end

    cb(itemCache[itemId])
end)
```

> ✅ `data` is your JSON payload from the NUI request. `cb()` must always be called to avoid stalls.

***

### 💻 JavaScript: Registering a Callback

```js
RegisterNuiCallbackType('getItemInfo');

on('__cfx_nui:getItemInfo', (data, cb) => {
    const itemId = data.itemId;

    if (!itemCache[itemId]) {
        cb({ error: 'No such item!' });
        return;
    }

    cb(itemCache[itemId]);
});
```

***

### ⚙️ C#: Registering a Callback

```csharp
RegisterNuiCallbackType("getItemInfo");

EventHandlers["__cfx_nui:getItemInfo"] += new Action<IDictionary<string, object>, CallbackDelegate>((data, cb) =>
{
    if (!data.TryGetValue("itemId", out var itemIdObj))
    {
        cb(new { error = "Item ID not specified!" });
        return;
    }

    var itemId = itemIdObj as string ?? "";

    if (!ItemCache.TryGetValue(itemId, out Item item))
    {
        cb(new { error = "No such item!" });
        return;
    }

    cb(item);
});
```

> ⚠️ C# requires a bit more boilerplate due to manual data marshaling.

***

### 🌐 Browser-Side (HTML/JS)

Invoke a NUI callback from your frontend like this:

```js
fetch(`https://${GetParentResourceName()}/getItemInfo`, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json; charset=UTF-8',
    },
    body: JSON.stringify({
        itemId: 'my-item'
    })
})
.then(resp => resp.json())
.then(resp => {
    console.log("Server responded:", resp);
});
```

> 🔁 Always return a response via `cb()` — even `{ ok: true }` — to prevent UI freezing.

***

### ✅ Summary

| Language | Register Callback                                   | Event Triggered           |
| -------- | --------------------------------------------------- | ------------------------- |
| Lua      | `RegisterNUICallback()`                             | `"getItemInfo"`           |
| JS       | `RegisterNuiCallbackType()` + `on('__cfx_nui:...')` | Fully supported           |
| C#       | Manual setup with event handler                     | Supported with marshaling |

Use NUI callbacks to turn your static NUI pages into rich, responsive interfaces that sync with your game data in real time.
