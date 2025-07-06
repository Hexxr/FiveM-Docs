# QBCore

The Core Object (`QBCore`) provides access to player data, shared items, configuration, commands, and utility functions.

#### Import Core Object

```
local QBCore = exports['qb-core']:GetCoreObject()
```

#### Load Only What You Need (qb-core v1.3.0+)

```
local QBCore = exports['qb-core']:GetCoreObject({'Functions'})
-- Access: QBCore.Functions

local QBCore = exports['qb-core']:GetCoreObject({'Functions', 'Shared'})
-- Access: QBCore.Functions, QBCore.Shared
```

#### Exported Functions Example

```
local function getPlayer(source)
    return exports['qb-core']:GetPlayer(source)
end
```

#### Accessing Player Data

```
local QBCore = exports['qb-core']:GetCoreObject()

local function getPlayerJob(source)
    local player = QBCore.Functions.GetPlayer(source)
    if player then
        local job = player.PlayerData.job
        print("Player's job:", job.name)
    end
end
```

#### Accessing Shared Data

```
local QBCore = exports['qb-core']:GetCoreObject()

local item = QBCore.Shared.Items['water_bottle']
print("Item name:", item.name)
print("Item label:", item.label)
```

#### Registering Commands

```
local QBCore = exports['qb-core']:GetCoreObject()

QBCore.Commands.Add('greet', 'Send a greeting', {}, false, function(source, args)
    local name = args[1] or 'player'
    TriggerClientEvent('chat:addMessage', source, {
        template = '<div class="chat-message">Hello, {0}!</div>',
        args = { name }
    })
end)
```

***

### Best Practices

* **Use `Wait(0)`** for per-frame logic.
* **Avoid heavy natives per tick**.
* **Cache frequently used natives** like `PlayerPedId()`.
* **Always include `Wait()` in infinite loops** to prevent crashes.
* **Use `RegisterNetEvent` for cross-resource/network events**.
* **Load only needed Core Object parts** to reduce memory overhead.

***
