---
description: More complex info
---

# Lua Scripting Overview

## Lua Setup and Core Concepts for FiveM

FiveM uses Lua as one of its core scripting languages, offering an easy, fast, and flexible way to build functionality into your server. Whether you’re creating client-side systems, gameplay logic, or event handlers, Lua is a great place to start.

***

### 🧠 What Version of Lua Does FiveM Use?

FiveM uses a modified version of Lua 5.3 called **CfxLua**. It includes enhancements from the game engine (Grit) and features designed specifically for GTA V scripting, including:

* Relative path support in file loading
* Native support for vectors (`vector3`, `vector4`, etc.)
* First-class quaternions for rotation handling

> This makes scripting spatial data like positions, heading, and rotation more accurate and performant.

***

### 🗂 Using Lua Files

To script in Lua, simply name your files with a `.lua` extension. You don’t need extra setup — FiveM automatically recognizes Lua scripts.

Example manifest line:

```lua
client_script 'myscript.lua'
```

***

### 🔢 Hash Support (Compile-Time Keys)

Lua in FiveM includes built-in support for **compile-time hash keys**, similar to what `GET_HASH_KEY()` does in C++. These are used throughout GTA/RAGE systems to represent models, animations, and more.

#### 🔍 Example:

```lua
-- Load a vehicle model
RequestModel(`adder`)

-- Compare against a specific model
if GetEntityModel(vehicle) == `buzzard` then
  print("This is a Buzzard.")
end
```

> The backticks (`) convert strings like` 'adder'`or`'buzzard'\` into hash keys behind the scenes.

***

### 🧮 Vectors and Quaternions

FiveM provides built-in data types like `vector2`, `vector3`, `vector4`, and `quat`. These are used in almost every part of the game world — including player positions, object movement, and camera control.

#### ✅ Why Use Them?

* Better performance (they’re native types in CfxLua)
* Cleaner syntax (`vector3(x, y, z)`)
* Automatically compatible with many FiveM natives

#### Example:

```lua
local coords = vector3(200.5, -1350.3, 30.0)
SetEntityCoords(PlayerPedId(), coords.x, coords.y, coords.z)
```

***

### 🔗 Using `exports`

Exports let you **reuse functions across different resources** — kind of like calling functions from another script.

#### 👋 Example 1: Inline Export

```lua
-- hello.lua
exports('SayHello', function(str)
  print('Hello, ' .. tostring(str) .. '!')
end)
```

#### 👋 Example 2: Explicit Manifest Export

```lua
-- fxmanifest.lua
client_script 'hello_explicit.lua'
export 'SayHello'

-- hello_explicit.lua
function SayHello(str)
  print('Hello, ' .. tostring(str) .. '!')
end
```

#### 🧪 In Another Resource:

```lua
exports.hello:SayHello('world') -- Prints: Hello, world!
```

> Note: Exported functions only become available after the first game tick.

***

### 📦 Built-in External Libraries

FiveM’s Lua runtime includes a few useful libraries globally:

| Library   | Description                      |
| --------- | -------------------------------- |
| `json`    | Based on dkjson 2.5              |
| `promise` | Promises for async logic         |
| `msgpack` | MessagePack 0.3.3 binary encoder |

These are useful for building advanced logic, API bridges, async workflows, and performance-tuned scripts.

***

### ✅ Summary

* Lua is your main scripting tool in FiveM and is incredibly beginner-friendly.
* CfxLua extends regular Lua with powerful game-specific tools like vectors, quaternions, and hash keys.
* Exports let you share logic across scripts and resources.
* FiveM even includes helpful libraries out of the box to power modern, modular development.
