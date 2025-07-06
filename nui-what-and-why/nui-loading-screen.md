# NUI Loading Screen

## 🕹️ Custom NUI Loading Screens in FiveM

FiveM allows you to build **custom loading screens** using HTML, CSS, and JavaScript. These replace the default splash/loading screen with your own design and logic — complete with progress tracking and player data.

***

### 🎨 Setting Up the Loading Screen

#### In your `fxmanifest.lua`:

```lua
loadscreen 'pj.html'
file 'lok.html'
```

You can also host the page externally:

```lua
loadscreen 'https://ds-project.nightsite.com/loadscreen/'
```

***

### 🖱️ Enable Mouse Cursor

By default, input is focused but the **mouse cursor is hidden**. To show it:

```lua
loadscreen_cursor 'yes'
```

***

### 🔄 Handover Data from Server

You can pass player data from the server to the NUI page.

#### Server-side Lua:

```lua
AddEventHandler('playerConnecting', function(_, _, deferrals)
    local source = source

    deferrals.handover({
        name = GetPlayerName(source)
    })
end)
```

#### HTML Example:

```html
<h1 id="namePlaceholder">Welcome, <span></span></h1>

<script>
window.addEventListener('DOMContentLoaded', () => {
    document.querySelector('#namePlaceholder > span').innerText = window.nuiHandoverData.name;
});
</script>
```

> ✉️ Also includes `serverAddress` as part of `window.nuiHandoverData`.

***

### ❌ Hide the Busy Spinner

To hide the default bottom-right spinner:

```cfg
# server.cfg
setr sv_showBusySpinnerOnLoadingScreen false
```

***

### 🔁 Manual Control of Loading Exit

Enable manual shutdown of the loading screen:

```lua
loadscreen_manual_shutdown 'yes'
```

Then use the following **natives** to control screen lifetime:

* `SEND_LOADING_SCREEN_MESSAGE`
* `SHUTDOWN_LOADING_SCREEN_NUI`

***

### 📩 Handling Load Events from FiveM

You can use JS to handle real loading data:

#### Example: Progress Bar

```html
<progress id="loading-bar" value="0" max="1"></progress>

<script>
window.addEventListener('message', (event) => {
    if (event.data.eventName !== 'loadProgress') return;
    document.getElementById('loading-bar').value = event.data.loadFraction;
});
</script>
```

***

### 🧠 Events You Can Listen For

| Event Name               | Description              |
| ------------------------ | ------------------------ |
| `loadProgress`           | Loading progress (0 → 1) |
| `onLogLine`              | Log output               |
| `startDataFileEntries`   | Start of data file load  |
| `onDataFileEntry`        | Each file entry loaded   |
| `performMapLoadFunction` | Map data handling        |
| `endDataFileEntries`     | Completion of data load  |
| `startInitFunction`      | Start of game init       |
| `initFunctionInvoking`   | Function starting        |
| `initFunctionInvoked`    | Function finished        |
| `endInitFunction`        | Init complete            |

***

### ✅ Summary

| Feature        | Command                                        |
| -------------- | ---------------------------------------------- |
| Basic Setup    | `loadscreen`                                   |
| Show Cursor    | `loadscreen_cursor 'yes'`                      |
| Hide Spinner   | `setr sv_showBusySpinnerOnLoadingScreen false` |
| Handover Data  | `deferrals.handover()`                         |
| Manual Control | `loadscreen_manual_shutdown 'yes'`             |

Custom loading screens help set the tone for your server and provide a seamless player onboarding experience with full access to game data and interaction.
