# Vehicle Natives

## 🚗 Vehicle Natives in FiveM

Vehicle natives give you control over all aspects of vehicle creation, manipulation, and removal. From spawning cars to modifying performance or detecting seat occupants — this category is crucial for gameplay.

***

### 🚘 Creating a Vehicle (Recommended Pattern)

```lua
local ModelHash = `adder` -- Use Compile-time hashes to get the hash of this model

if not IsModelInCdimage(ModelHash) then return end

RequestModel(ModelHash) -- Request the model
while not HasModelLoaded(ModelHash) do -- Wait for the model to load
    Wait(0)
end

local MyPed = PlayerPedId()
local Vehicle = CreateVehicle(ModelHash, GetEntityCoords(MyPed), GetEntityHeading(MyPed), true, false) -- Spawn networked vehicle
SetModelAsNoLongerNeeded(ModelHash) -- Free memory
```

* **Natives used:**
  * `IsModelInCdimage`
  * `RequestModel`
  * `HasModelLoaded`
  * `CreateVehicle`
  * `SetModelAsNoLongerNeeded`
* **Why:** This is a safe and memory-efficient way to spawn vehicles.

```lua
local model = `adder`
RequestModel(model)
while not HasModelLoaded(model) do
    Wait(0)
end

local coords = GetEntityCoords(PlayerPedId())
local vehicle = CreateVehicle(model, coords.x, coords.y, coords.z, GetEntityHeading(PlayerPedId()), true, false)
```

* **Native:** `CreateVehicle(modelHash, x, y, z, heading, isNetworked, dynamic)`
* **Use:** Spawns a new vehicle at a specific location.

***

### 🔑 Assigning Player to a Vehicle

```lua
TaskWarpPedIntoVehicle(PlayerPedId(), vehicle, -1) -- -1 for driver seat
```

* **Native:** `TaskWarpPedIntoVehicle(ped, vehicle, seatIndex)`
* **Use:** Places a player or ped into a specific seat.

***

### 🧼 Deleting a Vehicle

\-- DELETE\_VEHICLE local vehicle --\[\[ Vehicle ]] = DeleteVehicle()

Deletes a vehicle.\
The vehicle must be a mission entity to delete, so call this before deleting: SET\_ENTITY\_AS\_MISSION\_ENTITY(vehicle, true, true);\
eg how to use:\
SET\_ENTITY\_AS\_MISSION\_ENTITY(vehicle, true, true);\
DELETE\_VEHICLE(\&vehicle);\
Deletes the specified vehicle, then sets the handle pointed to by the pointer

* **Native:** `DeleteVehicle(vehicle: Vehicle*)`
* **Use:** Removes the vehicle from the world.

***

### 🚪 Locking Doors

\`-- SET\_VEHICLE\_DOORS\_LOCKED SetVehicleDoorsLocked( vehicle --\[\[ Vehicle ]], doorLockStatus --\[\[ integer ]] )

Parameters: vehicle: The vehicle whose doors are to be locked. doorLockStatus: The lock state to apply to the vehicle's doors, see eVehicleLockState. Locks the doors of a specified vehicle to a defined lock state, affecting how players and NPCs can interact with the vehicle.

NativeDB Introduced: v323 enum eVehicleLockState { // No specific lock state, vehicle behaves according to the game's default settings. VEHICLELOCK\_NONE = 0, // Vehicle is fully unlocked, allowing free entry by players and NPCs. VEHICLELOCK\_UNLOCKED = 1, // Vehicle is locked, preventing entry by players and NPCs. VEHICLELOCK\_LOCKED = 2, // Vehicle locks out only players, allowing NPCs to enter. VEHICLELOCK\_LOCKOUT\_PLAYER\_ONLY = 3, // Vehicle is locked once a player enters, preventing others from entering. VEHICLELOCK\_LOCKED\_PLAYER\_INSIDE = 4, // Vehicle starts in a locked state, but may be unlocked through game events. VEHICLELOCK\_LOCKED\_INITIALLY = 5, // Forces the vehicle's doors to shut and lock. VEHICLELOCK\_FORCE\_SHUT\_DOORS = 6, // Vehicle is locked but can still be damaged. VEHICLELOCK\_LOCKED\_BUT\_CAN\_BE\_DAMAGED = 7, // Vehicle is locked, but its trunk/boot remains unlocked. VEHICLELOCK\_LOCKED\_BUT\_BOOT\_UNLOCKED = 8, // Vehicle is locked and does not allow passengers, except for the driver. VEHICLELOCK\_LOCKED\_NO\_PASSENGERS = 9, // Vehicle is completely locked, preventing entry entirely, even if previously inside. VEHICLELOCK\_CANNOT\_ENTER = 10 }; This is the server-side RPC native equivalent of the client native SET\_VEHICLE\_DOORS\_LOCKED.

\-- Command to lock the car of the player for everyone. RegisterCommand("lockcar", function() local playerPed = PlayerPedId() -- Get the player ped local vehicle = GetVehiclePedIsIn(playerPed, false) -- Get the vehicle the player is in if (vehicle == 0) then return end -- If the player is not in a vehicle, return SetVehicleDoorsLocked(vehicle, 2) -- Lock the doors of the vehicle end, false)

\-- Command to unlock the car of the player for everyone. RegisterCommand("unlockcar", function() local playerPed = PlayerPedId() -- Get the player ped local vehicle = GetVehiclePedIsIn(playerPed, false) -- Get the vehicle the player is in if (vehicle == 0) then return end -- If the player is not in a vehicle, return SetVehicleDoorsLocked(vehicle, 1) -- Unlock the doors of the vehicle end, false)

* **Native:** `SetVehicleDoorsLocked(vehicle, lockStatus)`
* **Use:** Prevents players from entering the vehicle.

| Value | Status                 |
| ----- | ---------------------- |
| 1     | Unlocked               |
| 2     | Locked                 |
| 4     | Locked (Player Inside) |

***

### 🔄 Setting Vehicle Mods

```lua
SetVehicleModKit(vehicle, 0)
SetVehicleMod(vehicle, 11, 3, false) -- Engine mod level 3
- SET_VEHICLE_MOD
SetVehicleMod(
	vehicle --[[ Vehicle ]], 
	modType --[[ integer ]], 
	modIndex --[[ integer ]], 
	customTires --[[ boolean ]]
)

// eVehicleModType values modified to conform to script native reorganization (see 0x140D25327 in 1604).
enum eVehicleModType
{
	VMT_SPOILER = 0,
	VMT_BUMPER_F = 1,
	VMT_BUMPER_R = 2,
	VMT_SKIRT = 3,
	VMT_EXHAUST = 4,
	VMT_CHASSIS = 5,
	VMT_GRILL = 6,
	VMT_BONNET = 7,
	VMT_WING_L = 8,
	VMT_WING_R = 9,
	VMT_ROOF = 10,
	VMT_ENGINE = 11,
	VMT_BRAKES = 12,
	VMT_GEARBOX = 13,
	VMT_HORN = 14,
	VMT_SUSPENSION = 15,
	VMT_ARMOUR = 16,
	VMT_NITROUS = 17,
	VMT_TURBO = 18,
	VMT_SUBWOOFER = 19,
	VMT_TYRE_SMOKE = 20,
	VMT_HYDRAULICS = 21,
	VMT_XENON_LIGHTS = 22,
	VMT_WHEELS = 23,
	VMT_WHEELS_REAR_OR_HYDRAULICS = 24,
	VMT_PLTHOLDER = 25,
	VMT_PLTVANITY = 26,
	VMT_INTERIOR1 = 27,
	VMT_INTERIOR2 = 28,
	VMT_INTERIOR3 = 29,
	VMT_INTERIOR4 = 30,
	VMT_INTERIOR5 = 31,
	VMT_SEATS = 32,
	VMT_STEERING = 33,
	VMT_KNOB = 34,
	VMT_PLAQUE = 35,
	VMT_ICE = 36,
	VMT_TRUNK = 37,
	VMT_HYDRO = 38,
	VMT_ENGINEBAY1 = 39,
	VMT_ENGINEBAY2 = 40,
	VMT_ENGINEBAY3 = 41,
	VMT_CHASSIS2 = 42,
	VMT_CHASSIS3 = 43,
	VMT_CHASSIS4 = 44,
	VMT_CHASSIS5 = 45,
	VMT_DOOR_L = 46,
	VMT_DOOR_R = 47,
	VMT_LIVERY_MOD = 48,
	VMT_LIGHTBAR = 49,
};
```

* **Native:** `SetVehicleMod(vehicle, modType, modIndex, customTires)`
* **Use:** Upgrades vehicle parts (engine, turbo, wheels, etc.)

***

### ⛽ Vehicle Fuel Level (if fuel system enabled)

```lua
SetVehicleFuelLevel(vehicle, 100.0)
```

\-- SET\_VEHICLE\_FUEL\_LEVEL SetVehicleFuelLevel( vehicle --\[\[ Vehicle ]], level --\[\[ number ]] )

* **Native:** `SetVehicleFuelLevel(vehicle, fuel: float)`
* **Use:** Sets how much fuel the vehicle has.

***

### 🧍 Detect Who’s in the Vehicle

\-- GET\_PED\_IN\_VEHICLE\_SEAT local retval --\[\[ Ped ]] = GetPedInVehicleSeat( vehicle --\[\[ Vehicle ]], seatIndex --\[\[ integer ]] )

Parameters: vehicle: The vehicle to get the ped for. seatIndex: See eSeatPosition declared in IS\_VEHICLE\_SEAT\_FREE. Returns: A handle to a ped in the specified vehicle seat, or 0 if no such ped existed.

Gets the ped in the specified seat of the passed vehicle.

If there is no ped in the seat, and the game considers the vehicle as ambient population, this will create a random occupant ped in the seat, which may be cleaned up by the game fairly soon if not marked as script-owned mission entity.

NativeDB Added Parameter 3: BOOL p2 (uses a different GetOccupant function)

Examples:

Lua -- Checks if the player ped is in the drivers seat of the vehicle they are in. if GetPedInVehicleSeat(GetVehiclePedIsIn(PlayerPedId()), -1) == PlayerPedId() then print("The player is the driver of this vehicle.") else print("The player is not the driver of this vehicle.") end

\-- GET\_PED\_IN\_VEHICLE\_SEAT local retval --\[\[ Entity ]] = GetPedInVehicleSeat( vehicle --\[\[ Vehicle ]], seatIndex --\[\[ integer ]] )

Parameters: vehicle: The target vehicle. seatIndex: See eSeatPosition declared in IS\_VEHICLE\_SEAT\_FREE. Returns: The ped in the specified seat of the passed vehicle. Returns 0 if the specified seat is not occupied.

```lua
local driver = GetPedInVehicleSeat(vehicle, -1)
local passenger = GetPedInVehicleSeat(vehicle, 0)
```

* **Native:** `GetPedInVehicleSeat(vehicle, seatIndex)`
* **Use:** Returns the ped in a given seat.

***

### 🎨 Changing Vehicle Colors

```lua
SetVehicleColours(vehicle, 12, 12) -- Primary, secondary color
```

* **Native:** `SetVehicleColours(vehicle, colorPrimary, colorSecondary)`
* **Use:** Changes visual appearance of the vehicle.

***

### 🚫 Disabling Vehicle Controls

```lua
SetVehicleUndriveable(vehicle, true)
```

* **Native:** `SetVehicleUndriveable(vehicle, toggle)`
* **Use:** Prevents a vehicle from being driven.

***

### 🧠 Best Practices

* Always check if a vehicle exists: `if DoesEntityExist(vehicle)`
* For persistence, use **networked vehicles**
* Clean up unused vehicles with `DeleteVehicle()` or `SetEntityAsNoLongerNeeded()`
