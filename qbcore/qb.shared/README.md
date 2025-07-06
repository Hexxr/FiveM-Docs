# QB.Shared

#### QBCore Shared Folder Overview

The `shared` folder in `qb-core` stores all your server's essential data like jobs, items, vehicles, and gangs. Instead of using a database for static info, it uses Lua tables to speed things up. It's also deeply integrated with the Core Object, allowing quick access and configuration across your server.

***

#### Utilities (main.lua)

**Starter Items**

Items players receive on first login:

```lua
QBShared.StarterItems = {
    phone = 1,
    id_card = 1,
    driver_license = 1,
}

for item, amount in pairs(QBCore.Shared.StarterItems) do
    local itemInfo = QBCore.Shared.Items[item]
    print('Received '..amount..' '..itemInfo.label)
end
```

**Random Generators**

Generate randomized values:

```lua
local str = QBCore.Shared.RandomStr(8)      -- e.g. "xK93Lm2Q"
local num = QBCore.Shared.RandomInt(6)      -- e.g. "527194"
```

**String Utilities**

Useful functions for string handling:

```lua
local parts = QBCore.Shared.SplitStr("one,two,three", ",")
local clean = QBCore.Shared.Trim("   hello world   ")
```

**Math Utilities**

```lua
local rounded = QBCore.Shared.Round(3.14159, 2) -- Output: 3.14
```

**Vehicle Extras**

Control extra vehicle components:

```lua
QBCore.Shared.ChangeVehicleExtra(vehicle, 1, true)

local config = {
    [1] = true,
    [2] = false,
    [3] = true
}
QBCore.Shared.SetDefaultVehicleExtras(vehicle, config)
```

***

#### Items (items.lua)

Defines usable and non-usable items on your server. Example:

```lua
QBShared.Items = {
    id_card = {
        name = 'id_card',
        label = 'ID Card',
        weight = 0,
        type = 'item',
        image = 'id_card.png',
        unique = true,
        useable = true,
        shouldClose = false,
        description = 'A card containing your personal identification.'
    }
}
```

***

#### Jobs (jobs.lua)

Defines server jobs, including their grades and payment.

```lua
QBShared.Jobs = {
    police = {
        label = 'Law Enforcement',
        type = 'leo',
        defaultDuty = true,
        offDutyPay = false,
        grades = {
            { name = 'Recruit', payment = 50 },
            { name = 'Officer', payment = 75 },
            { name = 'Sergeant', payment = 100 },
            { name = 'Lieutenant', payment = 125 },
            { name = 'Chief', payment = 150, isboss = true },
        }
    }
}
```

Set this to control whether duty resets:

```lua
QBShared.ForceJobDefaultDutyAtLogin = true
```

***

#### Gangs (gangs.lua)

Works like jobs but geared toward gang hierarchy:

```lua
QBShared.Gangs = {
    lostmc = {
        label = 'The Lost MC',
        grades = {
            { name = 'Recruit' },
            { name = 'Enforcer' },
            { name = 'Shot Caller' },
            { name = 'Boss', isboss = true }
        }
    }
}
```

***

#### Vehicles (vehicles.lua)

Defines which vehicles can be owned and how they behave:

```lua
QBShared.Vehicles = {
    asbo = {
        model = 'asbo',
        name = 'Asbo',
        brand = 'Maxwell',
        price = 4000,
        category = 'compacts',
        type = 'automobile',
        shop = 'pdm'
    }
}
```

You can also find them by hash:

```lua
local vehicleData = QBShared.VehicleHashes[hash]
```

***

#### Weapons (weapons.lua)

Defines metadata for weapons, usually accessed by hash:

```lua
QBShared.Weapons = {
    [`weapon_pistol`] = {
        name = 'weapon_pistol',
        label = 'Walther P99',
        weapontype = 'Pistol',
        ammotype = 'AMMO_PISTOL',
        damageReason = 'Blasted'
    }
}
```

> Note: Weapons must also be added in `items.lua` if you want to use them as inventory items.

***

This shared structure is key to defining your economy, law system, job system, gangs, and vehicle usage. It's modular, fast, and easily adjustable for your server's unique needs.
