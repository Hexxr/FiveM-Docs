# Runtime references

## Lua Scripting Essentials for FiveM

This page covers core Lua scripting tools used in FiveM development. It includes usage of threads, delays, events, HTTP requests, and player-related functions, with examples.

***

### `Citizen.Wait` / `Wait`

Pauses the current thread for a given amount of milliseconds.

```lua
CreateThread(function()
    print("Start")
    Wait(2000) -- Pauses for 2 seconds
    print("2 seconds passed")
end)
```

Use `Wait(0)` to yield until the next game tick. Always include `Wait()` in loops to avoid crashing the game.

```lua
CreateThread(function()
    while true do
        print("Looping every 5 seconds")
        Wait(5000)
    end
end)
```

***

### `Citizen.CreateThread`

Runs code asynchronously in a separate thread.

```lua
print("Hi")
Citizen.CreateThread(function()
    while true do
        print("Hello world!")
        Wait(1000)
    end
end)
print("I'm printed even with the loop running")
```

***

### `Citizen.SetTimeout`

Executes a function after a set delay.

```lua
Citizen.SetTimeout(5000, function()
  print("5 seconds passed")
end)
```

***

### `Citizen.Trace`

Logs a message to the server console.

```lua
Citizen.Trace("Hello, World!\n")
```

***

### `RegisterNetEvent` & `AddEventHandler`

Registers and listens for a custom event.

```lua
RegisterNetEvent('myCustomEvent')
AddEventHandler('myCustomEvent', function(name, score)
    print(name .. " scored " .. score)
end)
```

***

### `TriggerEvent`

Triggers a local event.

```lua
TriggerEvent('myCustomEvent', 'JohnDoe', 100)
```

Also works with predefined functions:

```lua
local function updateScore(name, score)
    print(name .. " scored " .. score)
end
AddEventHandler('myCustomEvent', updateScore)
```

***

### `PerformHttpRequest`

Sends HTTP requests asynchronously.

```lua
PerformHttpRequest("http://example.com", function(code, data)
    print("Code:", code)
    print("Data:", data)
end)
```

### `PerformHttpRequestAwait`

Synchronous version (build 9515+):

```lua
local code, data = PerformHttpRequestAwait("http://example.com")
print("Code:", code)
print("Data:", data)
```

***

### `GetPlayers`

Returns a list of all server player IDs.

```lua
for _, id in ipairs(GetPlayers()) do
  print("Player ID:", id)
end
```

### `GetPlayerIdentifiers`

Returns a table of identifiers for a player.

```lua
local idents = GetPlayerIdentifiers(source)
for i = 1, #idents do
  print(idents[i])
end
```

Example to extract types:

```lua
local identifiers = {}
for _, ident in ipairs(GetPlayerIdentifiers(source)) do
    local key = ident:match("^(.-):")
    identifiers[key] = ident
end
print(identifiers["fivem"])
```

***

### Best Practices

* **Use `Wait(0)`** for per-frame logic.
* **Avoid heavy natives per tick**.
* **Cache frequently used natives** like `PlayerPedId()`.
* **Always include `Wait()` in infinite loops** to prevent crashes.
* **Use `RegisterNetEvent` for cross-resource/network events**.

***

This page serves as a foundation for creating responsive and safe Lua scripts in FiveM.
