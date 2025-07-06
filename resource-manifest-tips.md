# Resource Manifest Tips

## Resource manifest

The **resource manifest** is a file named `fxmanifest.lua` (or previously, `__resource.lua`), placed in a [resource folder](https://docs.fivem.net/docs/scripting-manual/introduction/introduction-to-resources/) on the server.

It is a Lua file, ran in a separate runtime from any Lua scripts in the resource, set up with a semi-declarative syntax to be used for defining metadata.

### Example <a href="#example" id="example"></a>

An example resource manifest for a hypothetical resource looks as follows:

```lua
-- Resource Metadata
fx_version 'cerulean'
games { 'rdr3', 'gta5' }

author 'John Doe <j.doe@example.com>'
description 'Example resource'
version '1.0.0'

-- What to run
client_scripts {
    'client.lua',
    'client_two.lua'
}
server_script 'server.lua'

-- Extra data can be used as well
my_data 'one' { two = 42 }
my_data 'three' { four = 69 }

-- due to Lua syntax, the following works too:
my_data('nine')({ninety = "nein"})

-- metadata keys can be arbitrary
pizza_topping 'pineapple'

```

Internally, this creates the following metadata entries:

* **fx\_version**: cerulean
* **game**: gta5
* **game**: rdr3
* **author**: John Doe <[j.doe@example.com](mailto:j.doe@example.com)>
* **description**: Example resource
* **version**: 1.0.0
* **client\_script**: client.lua
* **client\_script**: client\_two.lua (note the `s` table being expanded)
* **server\_script**: server.lua
* **my\_data**: one
* **my\_data**: three
* **my\_data**: nine
* **my\_data\_extra**: `{"two":42}` (as JSON)
* **my\_data\_extra**: `{"four":69}`
* **my\_data\_extra**: `{"ninety":"nein"}`
* **pizza\_topping**: pineapple

You can also obtain this metadata from scripts using [GetNumResourceMetadata](https://docs.fivem.net/natives/?_0x776E864) and [GetResourceMetadata](https://docs.fivem.net/natives/?_0x964BAB1D).

### Globbing <a href="#globbing" id="globbing"></a>

Some entry types may support 'globbing' for multiple files. These take a pattern syntax as follows:

| Example       | Matches                                       |
| ------------- | --------------------------------------------- |
| `*.lua`       | `a.lua`, `b.lua` (non-recursively)            |
| `dir/*.dll`   | `dir/a.dll`, `b.dll` (non-recursively)        |
| `**/*.lua`    | `dir1/a.lua`, `dir2/b.lua`, `dir1/dir2/f.lua` |
| `**.lua`      | same as above                                 |
| `**/cl_*.lua` | `dir1/cl_hi.lua`, etc.                        |

Support for globbing is specified under each entry type.

### Resource manifest entries <a href="#resource-manifest-entries" id="resource-manifest-entries"></a>

A list of built-in resource manifest entries follows. A resource can also contain custom metadata entries, which can be useful for script.

#### fx\_version <a href="#fx_version" id="fx_version"></a>

Defines the supported functionality for the resource. This has to be one of a specific set of words. Each entry inherits properties from the previous one. The current FXv2 resource version is **cerulean**.

#### game <a href="#game" id="game"></a>

Defines the supported game API sets for the resource.

| Name   | Meaning                                                                      |
| ------ | ---------------------------------------------------------------------------- |
| common | Runs on any game, but can't access game-specific APIs - only CitizenFX APIs. |
| gta4   | Runs on LibertyM.                                                            |
| gta5   | Runs on FiveM.                                                               |
| rdr3   | Runs on RedM.                                                                |

#### resource\_manifest\_version <a href="#resource_manifest_version" id="resource_manifest_version"></a>

**Deprecated**

You should be using `fxmanifest.lua` and `fx_version` instead.

Defines the supported functionality for the resource. This has to be one of a specific set of GUIDs. Each GUID inherits properties from the previous one. The current resource manifest version is **44febabe-d386-4d18-afbe-5e627f4af937**.

#### client\_script <a href="#client_script" id="client_script"></a>

**Note**

This directive supports globbing.

Defines a script to be loaded on the client, and implicitly adds the file to the resource packfile. The extension determines which script loader will handle the file:

| Extension    | File handler             | Meaning                                                                                             |
| ------------ | ------------------------ | --------------------------------------------------------------------------------------------------- |
| **.lua**     | `citizen:scripting:lua`  | Lua source code                                                                                     |
| **.net.dll** | `citizen:scripting:mono` | .NET assembly referencing [CitizenFX.Core.Client](https://nuget.org/packages/CitizenFX.Core.Client) |
| **.js**      | `citizen:scripting:v8`   | JavaScript source code (client only)                                                                |

#### server\_script <a href="#server_script" id="server_script"></a>

**Note**

This directive supports globbing. Reference [CitizenFX.Core.Server](https://nuget.org/packages/CitizenFX.Core.Server) for a .NET assembly.

Defines a script to be loaded on the server. The extension determines which script loader will handle the file, as with [client\_script](https://docs.fivem.net/docs/scripting-reference/resource-manifest/resource-manifest/#client-script).

#### shared\_script <a href="#shared_script" id="shared_script"></a>

**Note**

This directive supports globbing.

Defines a script to be loaded on both sides, and adds the file to the resource packfile. The extension determines which script loader will handle the file, as with [client\_script](https://docs.fivem.net/docs/scripting-reference/resource-manifest/resource-manifest/#client-script).

#### export <a href="#export" id="export"></a>

Defines a global function to be exported by a client script for Lua/JS. In Lua, this will export `_G[exportName]` as `exportName`.

Instead of using this, try using the `exports('name', ..)` or `Exports.Add` functions.

**Defining an export**

**Lua**

```lua
exports {
    'setWidget',
    'getWidget'
}

```

```lua
local lastWidget

function setWidget(widget)
    lastWidget = widget
end

function getWidget()
    return lastWidget
end

```

**Consuming an export**

**Lua**

```lua
exports.myresource:setWidget(50)

```

**C#**

```csharp
int widget = Exports["myresource"].getWidget();

```

#### server\_export <a href="#server_export" id="server_export"></a>

Defines a global function to be [exported](https://docs.fivem.net/docs/scripting-reference/resource-manifest/resource-manifest/#export) by a server script.

#### ui\_page <a href="#ui_page" id="ui_page"></a>

Sets the resource's [NUI](https://docs.fivem.net/docs/scripting-manual/nui-development/full-screen-nui/) page to the defined file or URL. If specifying a a file, the file (along with its dependencies) has to be referenced using [files](https://docs.fivem.net/docs/scripting-reference/resource-manifest/resource-manifest/#file).

```lua
ui_page 'html/index.html'
file 'html/index.html'

```

```lua
-- this also supports absolute URLs
ui_page 'https://ui-frontend.cfx.example.com/b20210501/index.html'

```

#### before\_level\_meta <a href="#before_level_meta" id="before_level_meta"></a>

Loads the specified level meta in the resource before the primary level meta.

**Deprecated**

Wherever possible you should use data files.

#### after\_level\_meta <a href="#after_level_meta" id="after_level_meta"></a>

Loads the specified level meta in the resource after the primary level meta.

**Deprecated**

Wherever possible you should use data files.

#### replace\_level\_meta <a href="#replace_level_meta" id="replace_level_meta"></a>

Replaces the level meta (usually `common:/data/levels/gta5/gta5.meta`) with the specified file in the resource. This has to be referenced using [files](https://docs.fivem.net/docs/scripting-reference/resource-manifest/resource-manifest/#file).

```lua
replace_level_meta 'mymap'
files {
    'mymap.meta'
}

```

#### data\_file <a href="#data_file" id="data_file"></a>

**Note**

This directive supports globbing in the filename field.

Adds a [data file](https://docs.fivem.net/docs/game-references/data-files/) of a specified type to the game extra content system.

```lua
files {
    'audio/mywaves/stupidcar.awc',
    'myvehicles.meta',
    'metas/*_handling.meta',
}

data_file 'AUDIO_WAVEPACK' 'audio/mywaves'
data_file 'VEHICLE_METADATA_FILE' 'myvehicles.meta'
data_file 'HANDLING_FILE' 'metas/*_handling.meta'

```

#### this\_is\_a\_map <a href="#this_is_a_map" id="this_is_a_map"></a>

Marks this resource as being a GTA map, and reloads the map storage when the resource gets loaded.

```lua
this_is_a_map 'yes' -- can be any value

```

#### server\_only <a href="#server_only" id="server_only"></a>

Marks the resource as being server-only. This stops clients from downloading anything of this resource.

```lua
server_only 'yes' -- can be any value

```

#### loadscreen <a href="#loadscreen" id="loadscreen"></a>

Sets the HTML file specified as the game loading screen.

```lua
loadscreen 'html/loadscreen.html'
file 'html/loadscreen.html'

```

#### loadscreen\_manual\_shutdown <a href="#loadscreen_manual_shutdown" id="loadscreen_manual_shutdown"></a>

Replacement for unsupported `SET_MANUAL_SHUTDOWN_LOADING_SCREEN_NUI` native.

```lua
loadscreen_manual_shutdown 'yes'

```

#### file <a href="#file" id="file"></a>

Adds the specified file to the resource packfile, to be downloaded by clients upon loading the resource.

```lua
file 'main.net.dll.mdb'

```

#### dependency <a href="#dependency" id="dependency"></a>

Requires the specified resource to load before the current resource.

```lua
dependency 'myresource-base'

dependencies {
    'myresource-base',
    'utility-resource'
}

```

**Runtime constraints**

The `dependency` field can also be used to specify requirements for the resource to run, such as a minimum server version, a server policy value, or a game build. These are specified using the following syntax:

```lua
dependencies {
    '/server:4500',                -- requires at least server build 4500
    '/policy:subdir_file_mapping', -- requires the server key to have 'subdir_file_mapping' granted
    '/onesync',                    -- requires state awareness to be enabled
    '/gameBuild:h4',               -- requires at least game build 2189
    '/native:0xE27C97A0',          -- requires native 0xE27C97A0 to be supported
}

```

The valid constraint types are as follows:

| Type      | Requirement                                         | Values                                             |
| --------- | --------------------------------------------------- | -------------------------------------------------- |
| server    | A minimum server version (build >= \[arg])          | Any number.                                        |
| policy    | A specific policy being granted.                    | subdir\_file\_mapping ('clothing support'), others |
| onesync   | State awareness not being disabled.                 | No value.                                          |
| gameBuild | Game build being set to at least this build.        | The same values as sv\_enforceGameBuild.           |
| native    | The specified native being supported on the server. | Any server-side native hash.                       |

#### lua54 <a href="#lua54" id="lua54"></a>

Enables Lua 5.4. You can read more about Lua 5.4 at [http://www.lua.org/manual/5.4/manual.html](http://www.lua.org/manual/5.4/manual.html)

```lua
lua54 'yes'

```

#### provide <a href="#provide" id="provide"></a>

Marks the current resource as a replacement for the specified resource. This means it'll start instead of the specified resource, if another resource requires it, and will act as if it is said resource if started.

```lua
provide 'mysql-async'

```

#### use\_experimental\_fxv2\_oal <a href="#use_experimental_fxv2_oal" id="use_experimental_fxv2_oal"></a>

This will enable the usage of OAL (One Argument List) for Lua. This aims to correct return-types of natives and provide better performance via faster native calls.

```lua
use_experimental_fxv2_oal "yes"

```

This feature is still experimental and **requires** [Lua 5.4](https://docs.fivem.net/docs/scripting-reference/resource-manifest/resource-manifest/#lua54) to be used.

**Vector unpacking**

Vector unpacking does not work when using OAL, this means that you will have to manually unpack coordinates instead of providing the `vector3`.

```lua
local coords = vector3(1, 2, 3)

SetEntityCoords(ped, coords) -- would normally work, but NOT with OAL
SetEntityCoords(ped, coords.x, coords.y, coords.z) -- works both with OAL, and without

```

**Downsides**

OALs main downside is that it cannot be used if the parameter type is wrong, as internally it will be converted to whatever the underlying type is.

This means that for natives that are undocumented or don't have the right types OAL will break in unexpected ways, or likely just not working at all.

#### clr\_disable\_task\_scheduler <a href="#clr_disable_task_scheduler" id="clr_disable_task_scheduler"></a>

When present, disables the custom C# task scheduler on the server. This will increase compatibility with third-party libraries using the .NET TPL, but make it more likely you'll have to `await Delay(0);` to end up back on the main thread.

This is already enabled by default if using `fx_version` of `bodacious` or higher.

```lua
clr_disable_task_scheduler 'yes'

```

#### convar\_category <a href="#convar_category" id="convar_category"></a>

When present adds the specified convars to the 'Project Settings' page in FxDK.

**Convar types:**

`CV_STRING`: Normal text input. `title, convar_name, "CV_STRING", default`

`CV_BOOL`: True / False input in the form of a checkbox. `title, convar_name, "CV_BOOL", default[, "label"]`

`CV_INT`: Manual number input with optional minimum and maximum. `title, convar_name, "CV_INT", default[, min, max]`

`CV_SLIDER`: Slider number input. `title, convar_name, "CV_SLIDER", default, min, max`

`CV_COMBI`: Number input with slider and manual input. `title, convar_name, "CV_COMBI", default, min, max`

`CV_PASSWORD`: Masked text input. `title, convar_name, "CV_SLIDER", default`

`CV_MULTI`: A drop-down selection menu, the first entry in Items is the default value. `title, convar_name, "CV_MULTI", items[{name, value}]`

If your convars are replacted (`setr`) you will need to prepend `$` to the convar name: `{ "foo", "$my_convar", "CV_STRING", "bar" }`

If your convars are server info (`sets`) you will need to prepend `#` to the convar name: `{ "Discord", "#my_convar", "CV_STRING", "discord.gg/fivem" }`

Example:

```lua
convar_category 'MySQL' {
    "GHMattiMySQL Configuration Options",
    {
        { "Connection String", "mysql_connection_string", "CV_STRING", "" },
        { "Debug Mode", "mysql_debug", "CV_MULTI", {
            { "None", "None" },
            { "Console", "Console" },
            { "File", "File"},
            { "FileAndConsole", "FileAndConsole" }
        }},
        { "Slow Query Warning", "mysql_slow_query_warning", "CV_COMBI", 100, 0, 5000 },
        { "Log Level", "mysql_log_level", "CV_INT", 15, 1, 15 },
        { "Log File Format", "mysql_log_file_format", "CV_STRING", "%s-%d.log" },
    }
}

```

### FXv2 versions <a href="#fxv2-versions" id="fxv2-versions"></a>

The resource manifest has to specify a particular FXv2 version for the resource to adhere to. A list of version names and features they are associated with is shown on this page.

Each manifest version includes all features from manifest versions above, except where they would overrule one another, in which case the latest version is used.

#### FX version `cerulean` (2020-05) <a href="#fx-version-cerulean-2020-05" id="fx-version-cerulean-2020-05"></a>

* Loads NUI resources in a 'secure context' to support WASM and fetch APIs, but requires callbacks to be changed to `https://` instead of `http://`.
