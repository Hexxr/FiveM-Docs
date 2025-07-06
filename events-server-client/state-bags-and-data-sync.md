# State Bags & Data Sync

## 🔄 State Bags in FiveM

State bags are a powerful system in FiveM used to attach custom key-value data to entities, players, and the global environment. These values automatically sync between the client and server, depending on ownership and visibility.

***

### 🧠 What Are State Bags?

In **state awareness mode**, every entity (like players, vehicles, objects) can store and share small chunks of information using a system called a _state bag_. You can think of this like a data container tied to that specific entity.

***

### 🧰 General Behavior & Guidelines

* Every access to a state value **deserializes the entire state bag**.
* You should **avoid nested access** like `Entity(x).state.x.y` and instead use flat keys like `Entity(x).state["x:y"]`.

#### ❌ Avoid:

```lua
-- These are problematic
Entity(x).state.x.y = "b"       -- Won't replicate
local y = Entity(x).state.x.y   -- Slow
```

#### ✅ Prefer:

```lua
Entity(x).state["x:y"] = "b"    -- Efficient and replicates
```

***

### 🧪 Lua Usage Examples

#### 🔹 Getting State

```lua
local vehicle = GetVehiclePedIsIn(PlayerPedId(), false)
local state = Entity(vehicle).state
print(state.mollis)
```

#### 🔹 Setting State

> You can only set state if you're the **entity owner** or the **server**.

```lua
Entity(vehicle).state.mollis = "vesuvius citrate"
```

***

### 👤 Player State Example

Players also have their own `state` bag:

```lua
-- Set on local player
LocalPlayer.state.soviet = "connection"

-- Access another player's state (server-side)
local testosterone = Player(source).state.testosterone
```

***

### 🌐 Global State

`GlobalState` lets you define global values accessible across the server.

```lua
-- Server only
GlobalState.moneyEnabled = true
```

> Clients can **read** GlobalState, but only the **server** can write to it.

***

### 🔧 Fine-Tuned Control with `set`

By default:

* Server state **replicates**
* Client state **does not replicate**

You can override this with `set()`:

```lua
-- Server: non-replicated
Entity(vehicle).state:set("clone", 600, false)

-- Client: replicate this to the server
Entity(enemy).state:set("taskAck", "guard", true)
```

***

### 📡 Listening for State Changes

You can use **state change handlers** to react when a value updates, like when a server tells a client something changed.

Example (see `AddStateBagChangeHandler` for full native):

```lua
AddStateBagChangeHandler('myKey', nil, function(bagName, key, value, reserved, replicated)
    print(("State for %s changed: %s = %s"):format(bagName, key, value))
end)
```

***

### 🔐 Policy Limitations

| Type         | Who Can Write          |
| ------------ | ---------------------- |
| Player State | That player + Server   |
| Entity State | Owning player + Server |
| Global State | Server only            |

***

### ✅ Summary

* State bags are great for syncing small amounts of custom data.
* Use `Entity().state["flat:key"]` for efficiency.
* Only owners (or the server) can write to a given state bag.
* Listen to changes using `AddStateBagChangeHandler`.
* Combine this with `GlobalState` and `LocalPlayer.state` for flexible architecture.

Use state bags wisely, and you'll unlock powerful synchronization patterns in your scripts.
