# Player Natives

## 🧑 Player Natives in FiveM

Player natives allow you to interact with local and remote players in a FiveM server. They are used for identifying players, working with their Peds, handling server/client IDs, and controlling various states such as health or wanted levels.

***

### 👤 Getting the Local Player

```lua
local playerId = PlayerId()
local ped = PlayerPedId()
```

* **Native:** `PlayerId()`, `PlayerPedId()`
* **Use:** `PlayerId()` gives the player index (client-side only), and `PlayerPedId()` returns the ped of the current player.

***

### 🎯 Server ID vs Player Index

* `PlayerId()` is local (client-only index)
* `source` is the server ID
* Convert between them using:

```lua
-- From Server ID to Player Index
local player = GetPlayerFromServerId(serverId)

-- From Player Index to Server ID
local serverId = GetPlayerServerId(PlayerId())
```

***

### 🧍 Get Player Ped

```lua
local ped = GetPlayerPed(-1) -- or use PlayerPedId()
```

* **Native:** `GetPlayerPed(playerIndex)`
* **Use:** Returns the ped for the given player index.

***

### 🩸 Set Player Health & Armor

```lua
SetEntityHealth(PlayerPedId(), 200) -- Max is 200
SetPedArmour(PlayerPedId(), 100)
```

* **Natives:** `SetEntityHealth`, `SetPedArmour`
* **Use:** Useful for healing or applying armor during gameplay.

\-- SET\_ENTITY\_HEALTH SetEntityHealth( entity --\[\[ Entity ]], health --\[\[ integer ]] )

Parameters: entity: The entity handle. health: The health we should set this entity to. When setting health for a player ped, the game will clamp the health value to ensure it does not exceed the maximum health. This maximum health can be retrieved by calling GET\_PED\_MAX\_HEALTH. It can also be modified by calling SET\_PED\_MAX\_HEALTH.

When setting the health for non-player peds or entities, the maximum health will be increased if the new health value exceeds the current maximum.

Default health for male peds is 200, for female peds it is 175.

Added parameters inflictor: The handle for the entity that caused the damage.

***

### 🔫 Give a Player a Weapon

```lua
GiveWeaponToPed(PlayerPedId(), `weapon_carbinerifle`, 250, false, true)
```

* **Native:** `GiveWeaponToPed(ped, weaponHash, ammo, equipNow, isHidden)`
* **Use:** Adds a weapon to a player’s loadout.

***

### 🧠 Check Player's Aiming State

```lua
if IsPlayerFreeAiming(PlayerId()) then
    print("You're aiming.")
end
```

* **Native:** \`-- IS\_PLAYER\_FREE\_AIMING local retval --\[\[ boolean ]] = IsPlayerFreeAiming( player --\[\[ Player ]] )

Gets a value indicating whether the specified player is currently aiming freely -- IS\_PLAYER\_FREE\_AIMING\_AT\_ENTITY local retval --\[\[ boolean ]] = IsPlayerFreeAimingAtEntity( player --\[\[ Player ]], entity --\[\[ Entity ]] )

Gets a value indicating whether the specified player is currently aiming freely at entity

* **Use:** Detects when a player is aiming — useful for targeting logic.

***

### 🚔 Wanted Level

```lua
ClearPlayerWantedLevel(PlayerId())
```

* **Native:** `ClearPlayerWantedLevel(player: Player)`
* **Use:** Removes wanted level for the player.

***

### 🧍 Detect Closest Player

```lua
local closestPlayer = -1
local closestDist = 9999.0
local myCoords = GetEntityCoords(PlayerPedId())

for _, id in ipairs(GetActivePlayers()) do
    local ped = GetPlayerPed(id)
    if ped ~= PlayerPedId() then
        local dist = #(GetEntityCoords(ped) - myCoords)
        if dist < closestDist then
            closestDist = dist
            closestPlayer = id
        end
    end
end

print("Closest player ID:", closestPlayer)

-- GET_ENTITY_COORDS
local retval --[[ vector3 ]] =
	GetEntityCoords(
		entity --[[ Entity ]], 
		alive --[[ boolean ]]
	)

Parameters:
entity: The entity to get the coordinates from.
alive: Unused by the game, potentially used by debug builds of GTA in order to assert whether or not an entity was alive.
Returns:
The current entity coordinates.

Gets the current coordinates (world position) for a specified entity.

Examples:

Lua
JavaScript
C#
local playerCoords = GetEntityCoords(PlayerPedId())
print(playerCoords) -- vector3(...)

server--- GET_ENTITY_COORDS
local retval --[[ vector3 ]] =
	GetEntityCoords(
		entity --[[ Entity ]]
	)

Parameters:
entity: The entity to get the coordinates from.
Returns:
The current entity coordinates.

Gets the current coordinates for a specified entity. This native is used server side when using OneSync.

See GET_ENTITY_COORDS for client side.

Examples:

Lua
JavaScript
C#
local function ShowCoordinates()
    local player = source
    local ped = GetPlayerPed(player)
    local playerCoords = GetEntityCoords(ped)

    print(playerCoords) -- vector3(...)
end

RegisterNetEvent("myCoordinates")
AddEventHandler("myCoordinates", ShowCoordinates)
```

***

### 🧾 Best Practices

* Use `PlayerPedId()` over `GetPlayerPed(-1)` for readability.
* Always validate networked players using `NetworkIsPlayerActive`.
* Convert between IDs using `GetPlayerServerId` and `GetPlayerFromServerId`.
