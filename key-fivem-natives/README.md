# Key FiveM Natives

## 🧍 Entity Natives in FiveM

Entity natives allow you to interact with any in-game object such as players, peds (NPCs), vehicles, and props. These functions are foundational for scripting behavior, detection, manipulation, and cleanup in the world.

***

### 📍 What Is an Entity?

An **entity** is any physical object in the game world: a ped, vehicle, or object. Each entity has a handle (ID), coordinates, heading, and network presence.

***

### 🔧 Common Entity Natives

#### 🗺️ Get Entity Coordinates

```lua
local ped = PlayerPedId()
local pos = GetEntityCoords(ped)
print(("x: %s, y: %s, z: %s"):format(pos.x, pos.y, pos.z))
```

* **Native:** `GetEntityCoords(entity: Entity, aliveOnly: boolean): vector3`
* **Use:** Retrieve the 3D position of any entity.

***

#### 🔄 Set Entity Heading

```lua
local heading = 90.0
SetEntityHeading(PlayerPedId(), heading)
```

* **Native:** `SetEntityHeading(entity: Entity, heading: float)`
* **Use:** Rotates an entity to face a specific direction.

***

#### 💀 Check if Entity Is Dead

```lua
if IsEntityDead(PlayerPedId()) then
    print("You're dead.")
end
```

* **Native:** `IsEntityDead(entity: Entity): boolean`
* **Use:** Determine if an entity is currently dead.

***

#### 🧊 Freeze Entity Position

```lua
FreezeEntityPosition(PlayerPedId(), true)
```

* **Native:** `FreezeEntityPosition(entity: Entity, freeze: boolean)`
* **Use:** Stops an entity from moving.

***

#### 👻 Make Entity Invisible

```lua
SetEntityVisible(PlayerPedId(), false, false)
```

* **Native:** `SetEntityVisible(entity: Entity, visible: boolean, unk: boolean)`
* **Use:** Hides an entity from view.

***

#### 🧼 Delete Entity

```lua
local veh = GetVehiclePedIsIn(PlayerPedId(), false)
DeleteEntity(veh)
```

* **Native:** `DeleteEntity(entity: Entity*)`
* **Use:** Permanently removes an entity from the world.

***

### 🔄 Converting Between Entity and Network IDs

#### Get Network ID

```lua
local veh = GetVehiclePedIsIn(PlayerPedId(), false)
local netId = NetworkGetNetworkIdFromEntity(veh)
```

#### Get Entity from Network ID

```lua
local veh = NetworkGetEntityFromNetworkId(netId)
```

***

### 🧠 Best Practices

* Always check `DoesEntityExist(entity)` before working with an entity.
* For multiplayer-safe scripts, use **Network IDs** to refer to entities across clients.
* Use `Entity(entity).state` if you're working with state bags.

***

### 📎 More Resources

* [FiveM Native Reference (Entities)](https://docs.fivem.net/natives/?_0x3C08A7C1)
* [State Bags & Ownership](https://docs.fivem.net/docs/scripting-reference/state-bags/)
* [Networked Entity Handling](https://docs.fivem.net/docs/scripting-reference/networking/)

***

Mastering entity natives is essential for controlling game logic and manipulating the world around the player.
