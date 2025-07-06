# Canceling Events

## 🚫 Canceling Events in FiveM

In FiveM, you can **cancel events** to prevent the default behavior from continuing — or to signal to other parts of your script that something shouldn't happen.

> Canceling an event **does not stop other event handlers** from running. It only marks the event as canceled.

***

### 🛑 Canceling an Event

Use the `CancelEvent()` native **inside an event handler**.

#### Lua

```lua
AddEventHandler("eventName", function(param1, param2)
    CancelEvent()
end)
```

#### JavaScript

```js
on("eventName", (param1, param2) => {
    CancelEvent();
});
```

#### C\#

```csharp
EventHandlers["eventName"] += new Action<string, bool>(TargetFunction);

private void TargetFunction(string param1, bool param2)
{
    CancelEvent();
}
```

***

### 🔍 Detecting Canceled Events

You can check if a **locally triggered event** was canceled using `WasEventCanceled()`.

***

### ✅ Example: Check Cancelation After Triggering

#### Lua

```lua
TriggerEvent("eventName", param1, param2)

if WasEventCanceled() then
    print("The event was canceled.")
end
```

Or use the return value of `TriggerEvent()` directly:

```lua
if TriggerEvent("eventName", param1, param2) then
    print("Event was canceled.")
end
```

***

#### JavaScript

```js
emit("eventName", param1, param2);

if (WasEventCanceled()) {
    console.log("Event was canceled");
}
```

***

#### C\#

```csharp
TriggerEvent("eventName", param1, param2);

if (WasEventCanceled())
{
    Console.WriteLine("Event was canceled.");
}
```

***

### ⚠️ Important Notes

* `WasEventCanceled()` only works with **local events** (triggered and handled on the same side).
* Cancelation doesn't prevent other listeners from running — it's simply a flag you can check afterward.
* For server-client communication, you'll need to build custom cancel signals in your event parameters if needed.

***

### 🧠 Use Cases

| Use Case                      | When to Cancel                                  |
| ----------------------------- | ----------------------------------------------- |
| Prevent a default UI behavior | Inside the event handler                        |
| Stop a script from continuing | Use `WasEventCanceled()` after `TriggerEvent()` |
| Override logic in a framework | First event handler cancels; others detect that |

Use event cancelation to fine-tune your event flow without rewriting core logic or event chains.
