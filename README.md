# Roblox ESP Library

A modular, extensible, high-performance ESP (Extra Sensory Perception) library for Roblox,
supporting R6, R15, custom rigs, accessories, tracers, offscreen arrows, and more.

------------------------------------------------------------------------------

FEATURES

- Box ESP: Full or Corner style bounding boxes
- Player Names: Customizable, with optional friendly dot
- Health Bars: Dynamic color and health value
- Equipped Item Display
- Head Box: Optional overlay on head
- Skeleton ESP: Draws lines between player joints
- Tracers: Origin options (bottom, center, mouse, etc)
- Offscreen Arrows: Points to players not visible
- Team & Distance Filtering
- Accessory Support: Hair, hats, backpack
- Box Mode: Quality, Balanced, Performance
- Game-Specific Rigs: (e.g. Bad Business)
- Automatic player management
- Extensive utility API
- Performance optimizations

------------------------------------------------------------------------------

QUICK START

    local ESP = loadstring(game:HttpGet("https://raw.githubusercontent.com/Ifykyklolololol/beebebeb/refs/heads/main/ddddd", true))();
    ESP:Load()
    
------------------------------------------------------------------------------

SETTINGS

Configure all features via ESP.settings (can be changed anytime):

+-----------------------+-----------+-------------------------------------------------------+-----------+
| Setting               | Type      | Description                                           | Default   |
+-----------------------+-----------+-------------------------------------------------------+-----------+
| enabled               | boolean   | Master on/off switch                                  | true      |
| names                 | boolean   | Show player names                                     | true      |
| boxes                 | boolean   | Draw ESP boxes                                        | true      |
| health                | boolean   | Show health bars                                      | true      |
| equipped              | boolean   | Show equipped item                                    | true      |
| headBoxes             | boolean   | Draw a head box                                       | false     |
| skeletons             | boolean   | Draw skeleton lines                                   | false     |
| tracers               | boolean   | Draw tracer lines                                     | false     |
| offscreenArrows       | boolean   | Show arrows for offscreen players                     | true      |
| teamCheck             | boolean   | Hide ESP for same-team players                        | true      |
| distanceCheck         | boolean   | Only show ESP within distanceLimit                    | false     |
| distanceLimit         | number    | Max ESP distance                                      | 1500      |
| friendlyIcon          | boolean   | Show dot for friendly player names                    | false     |
| includeAccessories    | boolean   | Include hats/hair/backpacks in box calc               | false     |
| boxUpdateMode         | string    | Box accuracy: Quality, Performance, Balanced          | Quality   |
| boxRenderMode         | string    | Box style: Full or Corner                             | Corner    |
| boxTransparency       | number    | Box background transparency (0–1)                     | 0.75      |
| performanceBoxWidth   | number    | Box width for Performance mode                        | 2         |
| performanceBoxHeight  | number    | Box height for Performance mode                       | 3         |
| tracerOrigin          | string    | Top, Bottom, Center, or Mouse                         | Bottom    |
| arrowOffset           | number    | Offscreen arrow distance from center                  | 350       |
| arrowWidth            | number    | Offscreen arrow width                                 | 12        |
| arrowHeight           | number    | Offscreen arrow height                                | 18        |
+-----------------------+-----------+-------------------------------------------------------+-----------+

------------------------------------------------------------------------------

TYPES & MODES

CharacterType:
    - "R6" – Classic 6-part rig
    - "R15" – Roblox 15-part rig
    - "BadBusiness" – Custom for Bad Business
    - (Extendable for your games!)

BoxUpdateMode:
    - "Quality" — Per-part bounding (slowest, most accurate)
    - "Balanced" — Model bounding box (fast)
    - "Performance" — Fixed-size box at root (fastest)

BoxRenderMode:
    - "Full" — Full rectangle
    - "Corner" — Only corners drawn

TracerOrigin:
    - "Top", "Bottom", "Center", "Mouse"

------------------------------------------------------------------------------

API REFERENCE

ESP:Load(customLoader)
    - Initializes the ESP library and creates the GUI.
    - Automatically adds/removes ESP for all other players.
    - customLoader (optional): function called after GUI creation.

ESP:AddClass(title, classTable)
    - Add a custom ESP class/instance (can be used for NPCs/objects).

ESP:DestroyClass(title)
    - Removes and destroys a registered ESP class/instance.

------------------------------------------------------------------------------

ESP Instance (Per Player)

Each player ESP instance provides:

    :Update()   -- Updates ESP state, positions, and data
    :Render()   -- Renders ESP GUI, lines, text, arrows
    :Destroy()  -- Cleans up all GUI objects for this instance

------------------------------------------------------------------------------

UTILITIES

Accessible via ESP.utils:

    GetCharacter(player)             --> (character, rootPart)
    GetCharacterType(character)      --> "R6", "R15", "BadBusiness"
    GetHead(character)               --> Head part
    IsAlive(player, character)       --> boolean
    IsFriendly(player)               --> boolean
    IsAdmin(player)                  --> boolean
    GetEquipped(player, character)   --> string
    GetHealthData(player, character) --> (health, maxHealth)
    CreateSkeletonMap(model)         --> {[number]: {[number]: BasePart}}

------------------------------------------------------------------------------

BOUNDING BOX INTERNALS

- Supports accurate part-mapping for R6, R15, Bad Business (extendable).
- Accessory mapping for Hair, Hats, Backpacks.
- Per-mode optimization:
    - Quality: Corner mapping per part/accessory (most accurate)
    - Balanced: Bounding box corners only
    - Performance: Fixed-size at HumanoidRootPart

------------------------------------------------------------------------------

ESP Class Structure

ESP instances are created per player and handled internally by the library.
They automatically update and render on RenderStepped/Heartbeat.

------------------------------------------------------------------------------

ADVANCED MAPPING & VISUALIZER

To debug box mappings, use the Character Map Visualizer:

    local function highlightParts(model)
        for _, v in model:GetChildren() do
            local object = v
            if v:IsA("Accessory") then
                object = v:FindFirstChild("Handle")
            end
            if object and object:IsA("BasePart") and object.Transparency < 1 then
                local cf, size = object.CFrame, object.Size
                local sx, sy, sz = size.X / 2, size.Y / 2, size.Z / 2
                local pos, front, right, up = cf.Position, cf.LookVector, cf.RightVector, cf.UpVector

                local points = {
                    ["pos - front * sz - right * sx - up * sy"] = pos - front * sz - right * sx - up * sy,
                    ["pos - front * sz - right * sx + up * sy"] = pos - front * sz - right * sx + up * sy,
                    ["pos - front * sz + right * sx - up * sy"] = pos - front * sz + right * sx - up * sy,
                    ["pos - front * sz + right * sx + up * sy"] = pos - front * sz + right * sx + up * sy,
                    ["pos + front * sz - right * sx - up * sy"] = pos + front * sz - right * sx - up * sy,
                    ["pos + front * sz - right * sx + up * sy"] = pos + front * sz - right * sx + up * sy,
                    ["pos + front * sz + right * sx - up * sy"] = pos + front * sz + right * sx - up * sy,
                    ["pos + front * sz + right * sx + up * sy"] = pos + front * sz + right * sx + up * sy
                }

                local x = Instance.new("Folder", model)
                x.Name = v.Name

                for name, v2 in points do
                    local y = Instance.new("Part", x)
                    y.Anchored = true
                    y.CanCollide = false
                    y.Color = Color3.new(1, 0, 0)
                    y.Position = v2
                    y.Size = Vector3.new(0.1, 0.1, 0.1)
                    y.Name = name
                end
            end
        end
    end

    highlightParts(workspace["YourCharacter"])

------------------------------------------------------------------------------

PERFORMANCE NOTES

- Performance mode: Best for crowded servers or low-end PCs.
- Quality mode: Most accurate, may lag in huge lobbies.
- Accessory and skeleton drawing: Most expensive.
- Head boxes and healthbars: Minimal impact.

Example: R6 scan counts
    - "Quality": 20 points/player
    - "Balanced": 4 points/player
    - "Performance": 0 (root only)

------------------------------------------------------------------------------

CUSTOMIZATION

- UI appearance can be edited by changing the create() utility or any UI properties.
- Extend utility methods (like GetEquipped) for your own game’s logic.
- Add custom rig mappings to characterMaps for more games or custom rigs.
- Use ESP:AddClass for NPCs, mobs, or special ESP objects.
- Team/friendly logic is customizable.

------------------------------------------------------------------------------

NOTES

- All code is fully commented for developers.
- Example/utility code (such as the visualizer) included at the bottom of the module.
- Pull requests and issues are welcome!

------------------------------------------------------------------------------

LICENSE & CREDITS

MIT License (or your choice)
Written by [Your Name/Handle]
Inspired by open-source ESP libraries and the Roblox dev community.

*Happy hacking!*
