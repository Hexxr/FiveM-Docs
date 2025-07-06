# Direct NUI

## 🎥 Direct-Rendered UI (DUI) in FiveM

In addition to full-screen NUI, FiveM supports **Direct NUI (DUI)** — a method of rendering web interfaces directly onto textures inside the game world.

This is perfect for creating:

* In-world screens (e.g., TVs or movie screens)
* HUD overlays
* Interactive UIs projected onto props or surfaces

***

### 🛠️ Core DUI Natives

Here are the main natives used to work with DUI:

| Native                                   | Purpose                                      |
| ---------------------------------------- | -------------------------------------------- |
| `CREATE_DUI`                             | Creates a DUI object with a specified URL    |
| `GET_DUI_HANDLE`                         | Gets the handle to the created DUI           |
| `CREATE_RUNTIME_TEXTURE_FROM_DUI_HANDLE` | Converts a DUI handle into a runtime texture |
| `DESTROY_DUI`                            | Destroys the DUI object                      |
| `IS_DUI_AVAILABLE`                       | Checks if DUI system is available            |
| `SEND_DUI_MESSAGE`                       | Sends a JSON message to the DUI browser      |
| `SET_DUI_URL`                            | Changes the URL of a running DUI instance    |
| `SEND_DUI_MOUSE_*`                       | Simulate mouse events (clicks, wheel, move)  |

***

### 📐 How It Works (Basic Flow)

1. **Create a DUI** with a URL pointing to your interface (hosted or in-resource)
2. **Get its handle** and pass it into a runtime texture
3. **Render it** using `DrawSprite`, `DrawRect`, or apply to a game object
4. Optionally, send messages or mouse input to the DUI

***

### 🔁 Example Workflow

```lua
-- Step 1: Create DUI with dimensions
local duiObj = CreateDui("https://project-ui.site/ui.html", 1280, 720)

-- Step 2: Get DUI handle
local duiHandle = GetDuiHandle(duiObj)

-- Step 3: Create texture
local txd = CreateRuntimeTxd("custom_txd")
local duiTx = CreateRuntimeTextureFromDuiHandle(txd, "custom_texture", duiHandle)

-- Step 4: Draw to screen
DrawSprite("custom_txd", "custom_texture", 0.5, 0.5, 0.6, 0.4, 0.0, 255, 255, 255, 255)
```

***

### 💡 Creative Use Cases

| Use Case              | Description                                     |
| --------------------- | ----------------------------------------------- |
| 🖼️ In-world Screens  | Render YouTube, websites, or game UI onto props |
| 🎯 Interactive Panels | Custom UI on map markers or interactables       |
| 📹 Virtual Cameras    | Project live UI views onto camera feeds         |
| 🧠 Hint Overlays      | Render tutorial UIs or guides over HUD          |
| 🕹️ Cinemas or TVs    | Multi-user video display screens                |

***

### 📩 Interacting With DUI

You can use `SendDuiMessage(duiObj, jsonData)` to pass data into the NUI browser from your script.

You can also simulate clicks and wheel input using:

```lua
SendDuiMouseDown(duiObj, button)
SendDuiMouseUp(duiObj, button)
SendDuiMouseMove(duiObj, x, y)
SendDuiMouseWheel(duiObj, delta)
```

***

### ❌ Cleaning Up

Always remember to destroy DUI objects to prevent memory leaks:

```lua
DestroyDui(duiObj)
```

***

### ✅ Summary

| Feature             | Description                                           |
| ------------------- | ----------------------------------------------------- |
| DUI Creation        | Create a runtime browser UI tied to a texture         |
| Texture Mapping     | Convert DUI into drawable texture                     |
| Flexible Rendering  | Render in screen-space, world-space, or on game props |
| Interaction Support | Send messages, mouse clicks, and updates              |
| Cleanup             | Always destroy unused DUI objects                     |

***

Direct-rendered UIs unlock immersive, interactive experiences that blend seamlessly into the world — from arcade machines to streaming TVs or sci-fi control panels.
