# Fullscreen NUI

## 🖥️ Fullscreen NUI in FiveM

A fullscreen NUI (New User Interface) is a powerful way to display immersive, interactive web-based UI in FiveM. These interfaces run in Chromium, overlaid on top of the game screen, and support HTML, CSS, and JavaScript.

***

### 🔧 Setting Up a Fullscreen NUI Page

To declare a fullscreen NUI for your resource, you need to define a `ui_page` in your resource's `fxmanifest.lua` or `__resource.lua`.

#### fxmanifest.lua

```lua
-- Define the entry HTML file
ui_page 'index.html'

files {
    'index.html',
    'build/main.js'
}
```

> 📡 You can also host UI pages externally:

```lua
ui_page 'https://my-frontend.cfx.project.com/b20210591/index.html'
```

***

### 🌐 NUI File Access (cfx-nui)

You can reference your in-resource JS/CSS assets using the `https://cfx-nui-` protocol. Example:

```html
<script type="text/javascript" src="https://cfx-nui-my-resource/production.js" async></script>
```

> ⚠️ The old `nui://` format is deprecated in modern browsers and no longer secure.

***

### 🛠️ Developer Tools

Debug your NUI using Chrome DevTools:

* Open: [http://localhost:13172/](http://localhost:13172/)
* Or in-game: open console (`F8`) and run:

```
nui_devTools
```

> Make sure **Developer Mode** is enabled in your game settings.

***

### 🧭 Controlling Focus with SET\_NUI\_FOCUS

Use the `SetNuiFocus` native to control input:

```lua
-- Lua: focus both keyboard and mouse
SetNuiFocus(true, true)

-- Remove focus
SetNuiFocus(false, false)
```

* The most recently focused resource is **on top** of the focus stack.
* NUI pages are fullscreen iframes — there's **no click-through** between layers.

***

### 📬 Communicating With NUI Pages

#### 🡒 Sending From Client (Lua)

```lua
SendNUIMessage({
    type = 'open'
})
```

#### 🡐 Receiving in JS

```js
window.addEventListener('message', (event) => {
    if (event.data.type === 'open') {
        doOpen();
    }
});
```

***

### ✅ Summary

| Feature          | Description                                   |
| ---------------- | --------------------------------------------- |
| `ui_page`        | Defines your entry HTML for NUI               |
| `SendNUIMessage` | Sends JSON from client to NUI                 |
| `SetNuiFocus`    | Grants/revokes keyboard & mouse focus         |
| Dev Tools        | Access at `localhost:13172` or `nui_devTools` |
| File Paths       | Use `https://cfx-nui-` for in-resource assets |

***

Use fullscreen NUI for menus, phones, laptops, dashboards, and more — just remember to manage focus and events cleanly for smooth UX.
