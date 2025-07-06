# Lua Basics for FiveM

Learn variables, functions, tables, and control structures specific to FiveM scripting.



This tutorial shows you how to create your own FiveM resource from scratch using Lua. We'll create a lightweight gamemode that spawns the player at a custom location and includes a basic `/car` command to spawn vehicles.

***

### 🗂 Step 1: Resource Folder Setup

Create a folder for your resource:

```
resources/[local]/mymode/
```

If you're on Windows, it might look like:

```
C:\path\to\cfx-server-data\resources\[local]\mymode
```

This folder will hold all the scripts and configuration for your custom mode.

***

### 🧾 Step 2: Create the Manifest

Inside `mymode`, create a new file called `fxmanifest.lua`. Paste in the following:

```lua
fx_version 'cerulean'
game 'gta5'

author 'Your Name Here'
description 'Simple gamemode example with spawn + car command'
version '1.0.0'

resource_type 'gametype' { name = 'Custom Mode: Stage Spawn' }

client_script 'mymode_client.lua'
```

***

### 🧠 Step 3: Write the Client Script

Create `mymode_client.lua` in the same folder. Here’s our basic logic:

```lua
local spawnPos = vector3(686.245, 577.950, 130.461)

AddEventHandler('onClientGameTypeStart', function()
    exports.spawnmanager:setAutoSpawnCallback(function()
        exports.spawnmanager:spawnPlayer({
            x = spawnPos.x,
            y = spawnPos.y,
            z = spawnPos.z,
            model = 'a_m_m_skater_01'
        }, function()
            TriggerEvent('chat:addMessage', {
                args = { 'Welcome to the server!' }
            })
        end)
    end)

    exports.spawnmanager:setAutoSpawn(true)
    exports.spawnmanager:forceRespawn()
end)
```

***

### 🚀 Step 4: Try It Out

1. Start your FXServer.
2.  In the server console, type:

    ```
    refresh; start mymode
    ```
3. Connect to your server with FiveM.
4. You’ll spawn on a stage and see a chat message.

Want a different location? Replace the `spawnPos` line with a new set of coordinates and use:

```
restart mymode
```

to reload the resource live.

***

### 🚗 Step 5: Add a Car Spawner Command

At the bottom of `mymode_client.lua`, add this command:

```lua
RegisterCommand('car', function(source, args)
    local vehicleName = args[1] or 'adder'

    if not IsModelInCdimage(vehicleName) or not IsModelAVehicle(vehicleName) then
        TriggerEvent('chat:addMessage', {
            args = { 'Invalid vehicle: ' .. vehicleName }
        })
        return
    end

    RequestModel(vehicleName)
    while not HasModelLoaded(vehicleName) do
        Wait(500)
    end

    local playerPed = PlayerPedId()
    local pos = GetEntityCoords(playerPed)

    local vehicle = CreateVehicle(vehicleName, pos.x, pos.y, pos.z, GetEntityHeading(playerPed), true, false)
    SetPedIntoVehicle(playerPed, vehicle, -1)

    SetEntityAsNoLongerNeeded(vehicle)
    SetModelAsNoLongerNeeded(vehicleName)

    TriggerEvent('chat:addMessage', {
        args = { 'Spawned a ^*' .. vehicleName .. '^r! Enjoy the ride!' }
    })
end, false)
```

***

### 🧹 Restarting the Resource

Instead of restarting your whole server, just use:

```
restart mymode
```

***
