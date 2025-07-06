# Text Formatting

## 🎨 Text Formatting in FiveM (Rockstar UI Style)

FiveM supports classical Rockstar-style UI text formatting using **tilde codes** (`~`). These can change colors, styles, add icons, input hints, and even subtitles formatting.

***

### 🎨 Basic Formatting Example

```
Demolish the ~r~enemy~s~.
Press ~INPUT_CONTEXT~ near the ~b~object~s~.
```

***

### 🟥 Color Codes

| Code      | Description             |
| --------- | ----------------------- |
| `~r~`     | Red (enemy)             |
| `~g~`     | Green (pickup)          |
| `~b~`     | Blue (friendly)         |
| `~f~`     | Friendly alternate      |
| `~y~`     | Yellow (destination)    |
| `~o~`     | Orange (team color)     |
| `~p~`     | Purple (team color)     |
| `~q~`     | Pink (Arena War)        |
| `~m~`     | Mid gray (silver)       |
| `~l~`     | Black                   |
| `~d~`     | Dark blue (team object) |
| `~s~`     | Reset to default color  |
| `~w~`     | White                   |
| `~v~`     | Script variable color 1 |
| `~u~`     | Script variable color 2 |
| `~HC_13~` | HUD color by index      |

***

### 🔤 Visual Formatting

| Code                      | Description              |
| ------------------------- | ------------------------ |
| `~n~`                     | Line break               |
| `~h~` or `~bold~`         | Bold text                |
| `~italic~`                | Italic text              |
| `~ws~` or `~wanted_star~` | Wanted star              |
| `~nrt~`                   | "No return top" line pad |
| `<C>~a~</C>`              | Condensed gamer tag      |
| `~EX_R*~`                 | Rockstar logo            |

***

### 📦 Content Formatting

| Code    | Description                           |
| ------- | ------------------------------------- |
| `~a~`   | Text label placeholder                |
| `~1~`   | Number placeholder                    |
| `~a_0~` | Reordered placeholder for translation |
| `~z~`   | Hidden subtitle if subtitles off      |

***

### 🎮 Input Controls

| Code                   | Description            |
| ---------------------- | ---------------------- |
| `~INPUT_CONTEXT~`      | Shows assigned key     |
| `~PAD_A~`, etc.        | Gamepad button display |
| `~ACCEPT~`, `~CANCEL~` | Prompt buttons         |
| `~INPUTGROUP_...~`     | Input group name       |

***

### 🗺️ Map/Blip Icons

| Code               | Description              |
| ------------------ | ------------------------ |
| `~BLIP_BENNYS~`    | Shows specific blip icon |
| `~HUD_COLOUR_...~` | HUD color reference      |

***

### ✅ Summary

* Use **tilde codes (`~`)** to format in-game text.
* Always **close formatting** with `~s~` or matching end tags.
* Great for **notifications, objectives, help prompts, and UI displays**.
