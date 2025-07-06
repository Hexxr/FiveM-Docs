# Network and Local IDs

## Understanding Network and Local IDs in FiveM

When working with FiveM scripts, you’ll come across multiple kinds of identifiers used to reference players, entities, and networked objects. Here's a breakdown of what they are and how to use them properly on both the client and server side.

***

### 👥 Player Identifiers

#### 🔹 Server ID (a.k.a. NetID)

* **Client-side**: Can access via `GetPlayerServerId(PlayerId())`
* **Server-side**: Known as `source` in most server events

This is the main way to refer to a player from server scripts. It’s unique per player while connected and is often used to send events or track players.

**✅ Convert Between Client & Server:**

```lua
-- Client
local serverId = GetPlayerServerId(PlayerId())
local player = GetPlayerFromServerId(serverId)
```

***

### 🧑‍💻 Player Index (Client-Only)

* **Client-side**: Used as `PlayerId()`, returns 0–128
* **Server-side**: Not available. Use server IDs instead.

The player index is useful for client natives like `GetPlayerPed(-1)` but doesn't round-trip to the server.

> ⚠️ Not all server players are visible to the client. Always check that a player index is valid.

***

### 🧍 Ped Handles

A ped is the actual game entity representing a player’s in-world character.

#### How to Work With It:

```lua
-- Get local player’s ped
local ped = GetPlayerPed(-1)

-- Get player from a ped
local player = NetworkGetPlayerIndexFromPed(ped)
```

Peds are synced using **Network IDs**, which let you access them across machines.

***

### 🚗 Entity Handles vs Network IDs

#### 🔸 Entity Handle

* A pointer to the actual game object (ped, vehicle, object, etc.)
* **Local only**: Handle is unique per client and not shared across players
* Used in most natives: `GetEntityCoords`, `SetEntityHealth`, etc.

> ⚠️ Entity handles are **not stable** across clients or the server.

***

#### 🔸 Network ID

* **Shared across all players and server**
* Lets you refer to the same object across machines
* Convert using:

```lua
-- Convert entity to network ID
local netId = NetworkGetNetworkIdFromEntity(entity)

-- Convert back to entity handle
local entity = NetworkGetEntityFromNetworkId(netId)
```

> Always verify that a network ID is valid before using:

```lua
if NetworkDoesEntityExistWithNetworkId(netId) then
    -- safe to use
end
```

***

### ✅ Summary Table

| ID Type       | Scope           | Usage Notes                                    |
| ------------- | --------------- | ---------------------------------------------- |
| Server ID     | Server ↔ Client | Use to track players in events or permissions  |
| Player Index  | Client-only     | Use for client natives like `GetPlayerPed`     |
| Ped Handle    | Both            | Represents the player's character in-game      |
| Entity Handle | Client-only     | Internal object reference (not shared)         |
| Network ID    | Shared          | Use to sync entities across clients and server |

***

### 💡 Use Case Example: Spawning a Vehicle for Another Player

```lua
-- Server
RegisterCommand("givecar", function(source, args)
    local targetId = tonumber(args[1])
    if targetId then
        TriggerClientEvent("myScript:spawnCar", targetId, "sultan")
    end
end)

-- Client
RegisterNetEvent("myScript:spawnCar")
AddEventHandler("myScript:spawnCar", function(model)
    local ped = PlayerPedId()
    local coords = GetEntityCoords(ped)
    local vehicle = CreateVehicle(model, coords.x, coords.y, coords.z, GetEntityHeading(ped), true, true)
end)
```

***

Keep this reference handy any time you're passing data between client and server. Misusing IDs is one of the most common causes of bugs in FiveM scripts.
