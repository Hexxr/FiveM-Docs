# TriggeringEvents

## 🚀 Triggering Events in FiveM

Triggering events allows your scripts to communicate between server/client and between different resources. Events are used to send data, run logic remotely, or sync actions across players and systems.

***

### 🔁 Triggering Local Events

Use this when both **sender and listener** are on the **same side** (client ↔ client or server ↔ server).

#### Lua

```lua
TriggerEvent("eventName", param1, param2)
```

#### JavaScript

```js
emit("eventName", param1, param2);
```

#### C\#

```csharp
TriggerEvent("eventName", param1, param2);
```

***

### 🌐 Triggering Server Events (from the Client)

There are **two main ways** to trigger server events:

| Method                     | When to Use              |
| -------------------------- | ------------------------ |
| `TriggerServerEvent`       | For small, fast messages |
| `TriggerLatentServerEvent` | For large data payloads  |

#### Lua

```lua
TriggerServerEvent("eventName", param1, param2)

-- Latent
TriggerLatentServerEvent("eventName", bps, param1, param2)
```

#### JavaScript

```js
emitNet("eventName", param1, param2)

TriggerLatentServerEvent("eventName", bps, param1, param2)
```

#### C\#

```csharp
TriggerServerEvent("eventName", param1, param2);

TriggerLatentServerEvent("eventName", bps, param1, param2);
```

> ℹ️ `bps` = **bytes per second** — controls how fast large payloads are transferred. Default is `25000`.

***

### 💻 Triggering Client Events (from the Server)

You must specify **which player(s)** to send the event to.

| Target Option | Description                           |
| ------------- | ------------------------------------- |
| `-1`          | Send to **all** clients (Lua/JS only) |
| `player`      | Send to a specific player             |

#### Lua

```lua
TriggerClientEvent("eventName", targetPlayer, param1, param2)

-- Latent
TriggerLatentClientEvent("eventName", targetPlayer, bps, param1, param2)
```

#### JavaScript

```js
emitNet("eventName", targetPlayer, param1, param2)

TriggerLatentClientEvent("eventName", targetPlayer, bps, param1, param2);
```

#### C\#

```csharp
// Send to one player
player.TriggerEvent("eventName", param1, param2);

// Send to all players
TriggerClientEvent("eventName", param1, param2);

// Latent to one player
player.TriggerLatentEvent("eventName", bps, param1, param2);

// Latent to all
TriggerLatentClientEvent("eventName", bps, param1, param2);
```

> ⚠️ High `bps` values (> 10,000,000) can cause timeouts or crashes. Use with caution when sending large data.

***

### ✅ Summary

| Event Type              | Function                             | Where Used       |
| ----------------------- | ------------------------------------ | ---------------- |
| Local                   | `TriggerEvent()` / `emit()`          | Same side only   |
| Server (normal)         | `TriggerServerEvent()` / `emitNet()` | Client to Server |
| Server (large payloads) | `TriggerLatentServerEvent()`         | Client to Server |
| Client (normal)         | `TriggerClientEvent()` / `emitNet()` | Server to Client |
| Client (large payloads) | `TriggerLatentClientEvent()`         | Server to Client |

***

Using the correct trigger method based on size and direction keeps your events efficient and safe. Always use latent events for large data like NUI payloads, logs, or file blobs.
