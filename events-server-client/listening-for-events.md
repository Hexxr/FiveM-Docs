# Listening For Events

## 🔊 Listening for Events in FiveM

In FiveM, events are the primary way to communicate between client and server — and between scripts. Knowing how to **listen for**, **respond to**, and **securely handle events** is critical to building interactive systems.

***

### 🧠 Basic Concept

You use **event listeners** to handle custom or native events. These can be local (within the same environment) or **networked** (between client and server).

***

### 🔂 Lua Example

Basic local event listener:

```lua
AddEventHandler("eventName", function(param1, param2)
    print("Event triggered with:", param1, param2)
end)
```

Networked event (client or server):

```lua
RegisterNetEvent("eventName")
AddEventHandler("eventName", function(param1, param2)
    print("Network event triggered with:", param1, param2)
end)
```

> 🧠 **Note**: In Lua, the `source` global holds the player ID that triggered the event **on the server**. If you're using it inside a `Wait()` or async scope, store it in a local variable first.

***

### 💬 JavaScript Example

Local:

```js
on("eventName", (param1, param2) => {
    console.log("Event triggered with:", param1, param2);
});
```

Networked:

```js
onNet("eventName", (param1, param2) => {
    console.log("Net event triggered:", param1, param2);
});
```

> ✅ In JS, `onNet()` allows receiving events from both local and remote sources.

***

### 🧾 C# Example

Basic handler:

```csharp
EventHandlers["eventName"] += new Action<string, bool>(TargetFunction);

private void TargetFunction(string param1, bool param2)
{
    // Event logic here
}
```

With attribute:

```csharp
[EventHandler("eventName")]
private void TargetFunction(string param1, bool param2)
{
    // Event logic here
}
```

Networked with `source`:

```csharp
EventHandlers["netEventName"] += new Action<Player, string, bool>(TargetFunction);

private void TargetFunction([FromSource] Player source, string param1, bool param2)
{
    // Source contains the player who triggered the event
}
```

Or with attributes:

```csharp
[EventHandler("netEventName")]
private void TargetFunction([FromSource] Player source, string param1, bool param2)
{
    // Access player object directly from source
}
```

***

### 🔐 Event Registration Rules

#### Lua / JS

If your event is going to be triggered across the network (e.g., from client to server), you **must register it** using:

```lua
RegisterNetEvent("eventName")
```

```js
onNet("eventName", callback)
```

> C# does **not require registration** for network events — but you should still secure them from abuse.

***

### ✅ Summary

| Language | Local Event                       | Networked Event                            | Notes                                        |
| -------- | --------------------------------- | ------------------------------------------ | -------------------------------------------- |
| Lua      | `AddEventHandler()`               | `RegisterNetEvent()` + `AddEventHandler()` | `source` must be stored early if using async |
| JS       | `on()`                            | `onNet()`                                  | Use `onNet()` for remote events              |
| C#       | `EventHandler` / `[EventHandler]` | Same                                       | No need to manually register                 |

Use these methods to handle everything from UI interaction to gameplay logic and custom frameworks.
