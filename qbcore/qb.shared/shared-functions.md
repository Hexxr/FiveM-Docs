# Shared Functions

#### Dynamic Imports (Jobs, Gangs, Items)

These functions let you add data dynamically at runtime—handy for modular scripts and standalone job/gang/item additions.

**Adding Single Entries:**

```
-- Job
QBCore.Functions.AddJob('trucker', { label = 'Trucker', defaultDuty = true, offDutyPay = false, grades = { ['0'] = { name = 'Driver', payment = 50 } } })

-- Gang
exports['qb-core']:AddGang('azteca', { label = 'Azteca', grades = { ['0'] = { name = 'Recruit' } } })

-- Item
QBCore.Functions.AddItem('sandwich', {
    name = 'sandwich', label = 'Sandwich', weight = 10, type = 'item',
    image = 'sandwich.png', unique = false, useable = true,
    shouldClose = true, description = 'A nice quick bite'
})
```

**Adding Multiple:**

```
QBCore.Functions.AddJobs({
    ['garbage'] = {
        label = 'Sanitation', defaultDuty = true, offDutyPay = false,
        grades = { ['0'] = { name = 'Cleaner', payment = 40 } }
    }
})

exports['qb-core']:AddItems({
    ['cola'] = {
        name = 'cola', label = 'Cola', weight = 5,
        type = 'item', image = 'cola.png', unique = false,
        useable = true, shouldClose = true,
        description = 'Ice cold and fizzy.'
    }
})
```

**Core Object Update Events:**

Add these to auto-update the core object when needed:

```
-- Client
RegisterNetEvent('QBCore:Client:UpdateObject', function()
    QBCore = exports['qb-core']:GetCoreObject()
end)

-- Server
RegisterNetEvent('QBCore:Server:UpdateObject', function()
    if source ~= '' then return false end
    QBCore = exports['qb-core']:GetCoreObject()
end)
```

***

This modular approach gives you full control and flexibility when creating content—whether it's jobs, gangs, or new items—without bloating your base core files.
