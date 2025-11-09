# Zone Creator

A self-contained **zone creation tool** for FiveM.
Drop it into any resource — no dependencies required.

Support honest development. Keep the original license and credit intact.

## Screenshots

<details>
  <summary>Click To Preview</summary>

  <img width="1274" height="719" alt="image" src="https://github.com/user-attachments/assets/73a77b05-2549-483e-bbc3-e117db3ae001" />
  <img width="1277" height="717" alt="image" src="https://github.com/user-attachments/assets/fc447232-16d4-4842-b758-133066cdd959" />
  <img width="1271" height="719" alt="image" src="https://github.com/user-attachments/assets/9d00e2b9-58df-48e4-af7d-10eea94a63e5" />

</details>

## Setup

1. Add the script to your resource as a **shared script** in `fxmanifest.lua`:

```lua
shared_script 'zones.lua'
```

2. Add ACE permissions in your `server.cfg`:

```cfg
add_ace group.admin zone_creator.use allow
add_principal identifier.fivem:YOUR_IDENTIFIER group.admin
```

## Commands

All commands are server-side and require ACE permission.

| Command         | Description               |
| --------------- | ------------------------- |
| `/zones:create` | Starts the zone creator   |
| `/zones:debug`  | Toggles the debug overlay |

## Zone Creation Controls

| Key           | Action                   |
| ------------- | ------------------------ |
| W / A / S / D | Move                     |
| Q / E         | Move up / down           |
| SHIFT         | Fast movement            |
| CTRL          | Slow movement            |
| F             | Add a point              |
| X             | Undo last point          |
| ENTER         | Finish and save the zone |
| G             | Toggle debug overlay     |
| BACKSPACE     | Exit creator             |

## Zone Data

When a zone is finished, the client triggers:

```lua
TriggerEvent(resource_name .. ":zone_created", {
    name = zone_name,
    zone = { vector3(x, y, z), ... },
    player = {
        source = GetPlayerServerId(PlayerId()),
        name = GetPlayerName(PlayerId())
    }
})
```

## Events

These events can be used in your own scripts to react to zone state changes.

| Event                        | Description                                |
| ---------------------------- | ------------------------------------------ |
| `resource_name:zone_created` | Fired when a zone is created and saved     |
| `resource_name:entered_zone` | Fired when a player enters a zone          |
| `resource_name:inside_zone`  | Fired while a player remains inside a zone |
| `resource_name:left_zone`    | Fired when a player leaves a zone          |

Example usage:

```lua
AddEventHandler("my_resource:entered_zone", function(name)
    print("Player entered zone:", name)
end)
```

## Notes

* Uses native draw calls for UI, keeping it lightweight and dependency-free.
* Zone checks only occur when the player moves to minimize resource usage.
* If persistent zones are required, save the `all_zones` table server-side.
* Modify freely, but retain license and credit.

## License

Licensed under the MIT License.
See the `LICENSE` file in the root directory for details.
