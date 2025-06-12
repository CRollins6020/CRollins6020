
# Cyber Engine Tweaks Scripting Guide – The Rosetta Stone  
**Version:** 1.35.1  
**Audience:** Intermediate to Expert Mod Developers  
**Scope:** Complete reference for Cyberpunk 2077 scripting with CET (Lua, UI, API, patterns, performance, integration)  

---

## Table of Contents  
1. [Introduction](#introduction)  
2. [Environment & Mod Setup](#environment--mod-setup)  
3. [Lua Scripting Fundamentals](#lua-scripting-fundamentals)  
4. [Hotkey and Event Handling](#hotkey-and-event-handling)  
5. [ImGui UI API Reference](#imgui-ui-api-reference)  
6. [Game Object API Reference](#game-object-api-reference)  
7. [Vector4, CName, and TweakDBID](#vector4-cname-and-tweakdbid)  
8. [Item Spawning & Entity Control](#item-spawning--entity-control)  
9. [Modular File Structures & Debug Flags](#modular-file-structures--debug-flags)  
10. [Persistence and Save/Load Data](#persistence-and-saveload-data)  
11. [Performance Tips and Error Handling](#performance-tips-and-error-handling)  
12. [Full UI Panel Example](#full-ui-panel-example)  
13. [Best Practices Checklist](#best-practices-checklist)  
14. [Appendix: CET Lifecycle Events](#appendix-cet-lifecycle-events)  
15. [References](#references)  

---

## Introduction  
This guide is intended to be a one-stop reference for advanced Cyber Engine Tweaks scripting in **Cyberpunk 2077**. It covers every major scripting feature in CET with verified examples, patterns, and safe usage techniques.

---

## Environment & Mod Setup  
Path for mods:  
```
Cyberpunk 2077\bin\x64\plugins\cyber_engine_tweaks\mods\<mod_name>\
```

Every mod must have at least:  
```lua
-- init.lua
registerForEvent("onInit", function()
  print("Mod initialized.")
end)
```

Optional:  
- `module.lua` for helpers  
- `config.lua` for toggle values  
- `data.json` for persistence  

---

## Lua Scripting Fundamentals  
**Types**:  
- `String`, `Number`, `Bool`, `Table`, `Function`, `Nil`  
- CET-specific: `Vector4`, `CName`, `TweakDBID`, `Handle`, `GameInstance`  

**Control**:  
```lua
if player:IsInCombat() then
  print("Combat detected.")
end
```

**Functions & Modules**:  
```lua
function greet(name)
  print("Hello, " .. name)
end
```

Use `require("module")` to split logic across files.

---

## Hotkey and Event Handling  
```lua
registerHotkey("ToggleVision", "Toggle Night Vision", function()
  nightVision = not nightVision
end)

registerForEvent("onOverlayOpen", function()
  print("Overlay shown.")
end)
```

---

## ImGui UI API Reference  
| Function               | Description                         |
|------------------------|-------------------------------------|
| `ImGui.Begin(label)`   | Opens a panel                       |
| `ImGui.Text(text)`     | Adds a label                        |
| `ImGui.InputText()`    | Text input                          |
| `ImGui.Checkbox()`     | Toggle on/off                       |
| `ImGui.SliderFloat()`  | Slider input                        |
| `ImGui.Button()`       | Button widget                       |

Example:
```lua
if ImGui.Begin("Control Panel") then
  ImGui.Text("Status: OK")
  if ImGui.Button("Do Action") then
    performAction()
  end
end
ImGui.End()
```

---

## Game Object API Reference  
```lua
local player = Game.GetPlayer()
local vehicle = player:GetCurrentVehicle()
local sys = Game.GetTargetingSystem()

if vehicle then vehicle:Dispose() end
```

| Common Function             | Purpose                           |
|----------------------------|-----------------------------------|
| `Game.GetPlayer()`         | Returns player handle             |
| `Game.TeleportPlayerToPosition(v4)` | Teleport player           |
| `Game.GetQuestsSystem()`   | Access quest logic                |
| `Game.GetWorkspotSystem()` | Movement/pose API                 |
| `Game.GetStatPoolsSystem()`| HP, stamina, RAM pools            |

---

## Vector4, CName, and TweakDBID  
```lua
Vector4.new(x, y, z, w)     -- World position  
CName.new("CameraEffect")   -- Hash for CET interfaces  
TweakDBID.new("Items.Preset_Nova")  -- Used in spawning  
```

Use TweakDB Explorer to find valid IDs.

---

## Item Spawning & Entity Control  
```lua
local ts = Game.GetTransactionSystem()
local itemID = TweakDBID.new("Items.MilitaryShirt_02")
ts:GiveItem(player, itemID, 1)
```

Spawn by coordinates:
```lua
local pos = Vector4.new(-100, 500, 20, 1)
Game.Spawn("Items.BombGrenade", pos)
```

---

## Modular File Structures & Debug Flags  
**Structure**:
```
mods\MyMod\
├── init.lua
├── module.lua
├── config.lua
└── debug.lua
```

**Debug Flags**:
```lua
DEBUG = true

if DEBUG then
  print("DEBUG: Executing fallback")
end
```

---

## Persistence and Save/Load Data  
```lua
local json = require("modules/json")

function saveData(data)
  local file = io.open("mods/MyMod/data.json", "w")
  file:write(json.encode(data))
  file:close()
end

function loadData()
  local file = io.open("mods/MyMod/data.json", "r")
  return json.decode(file:read("*a"))
end
```

---

## Performance Tips and Error Handling  
- Avoid `print()` inside `onUpdate()`  
- Use `os.clock()` to measure performance  
- Use `pcall()` for protected calls  
```lua
local success, err = pcall(function()
  riskyCall()
end)
if not success then print("Error: " .. err) end
```

---

## Full UI Panel Example  
```lua
registerForEvent("onDraw", function()
  if ImGui.Begin("Control Hub") then
    ImGui.Text("Cyber UI Panel")
    if ImGui.Button("Warp to Safehouse") then
      Game.TeleportPlayerToPosition(Vector4.new(234.3, 900.0, 10.0, 1))
    end
  end
  ImGui.End()
end)
```

---

## Best Practices Checklist  
- [x] Use `registerHotkey` only on `onInit`  
- [x] Wrap risky code in `pcall`  
- [x] Use modular scripts (`require`)  
- [x] Avoid overusing `print()`  
- [x] Use TweakDBID only when confirmed valid  
- [x] Log to file when debugging large mods  

---

## Appendix: CET Lifecycle Events  
| Event            | When It Fires                    |
|------------------|----------------------------------|
| `onInit`         | When the mod loads               |
| `onUpdate`       | Each game frame                  |
| `onDraw`         | Every CET UI render              |
| `onOverlayOpen`  | When the overlay opens           |
| `onOverlayClose` | When the overlay closes          |
| `onShutdown`     | Game or CET shutting down        |

---

## References  
- Yamashi. (2024). *Cyber Engine Tweaks GitHub*. https://github.com/yamashi/CyberEngineTweaks  
- CET Discord Community (2024). *Script Examples and Support*.  
- TweakDB Explorer. (2024). https://github.com/WolvenKit/tweakdb-explorer  
- NexusMods. (2024). *Cyberpunk 2077 Mods Collection*.  


---

## Game-Specific Quirks

Cyberpunk 2077 has several internal quirks when modding through CET:

- **Delayed Game Systems**: Some systems like `WorkspotSystem` or `InkSystem` aren't available immediately on game start. Use `onUpdate` with a `system ~= nil` check.
- **Entity Handles**: Always check if returned objects from functions (e.g., `GetTargetingSystem()`) are `nil` before calling methods.
- **Fast Travel & Teleports**: May fail during active quest scenes. Use `Game.GetTeleportationFacilitySystem():Teleport()` only when the player isn't in dialogue or cinematic.
- **Time System**: `Game.GetTimeSystem():GetGameTimeStamp()` returns game-time seconds, not real-time.

---

## TweakDB Mutation Examples

While most CET mods read from TweakDB, you can also mutate data at runtime:

```lua
local TweakDB = TweakDBInterface

-- Change a weapon’s crit chance
TweakDB.SetFlat("Items.Base_Pistol_inline5.value", 0.50)

-- Change clothing stat modifier
TweakDB.SetFlat("Items.Techie_01_Boots_inline2.value", 0.2)
```

> ⚠️ Changes affect all instances of that entry and persist until game restart.

---

## Cross-Mod Communication

Multiple CET mods can interact via shared globals:

**Mod A:**
```lua
_G.GlobalData = _G.GlobalData or {}
_G.GlobalData["playerState"] = { armor = 200, shield = true }
```

**Mod B:**
```lua
local armor = _G.GlobalData and _G.GlobalData["playerState"] and _G.GlobalData["playerState"].armor
if armor then print("Player armor from Mod A:", armor) end
```

Use keys unique to your mod to avoid name collisions.

---

## Template Projects

Here are starter use cases based on this guide:

### 🔹 Toggleable Night Vision
- Hotkey toggles a `postFX` override using `CName`.
- Modifies screen color and brightness via `CameraSystem`.

### 🔹 Custom Fast Travel Menu
- Spawns buttons via ImGui for known Vector4 positions.
- Uses `Game.TeleportPlayerToPosition()`.

### 🔹 Loot Filter & Highlighter
- Uses `Game.GetTargetingSystem()` to loop nearby items.
- ImGui panel toggles “highlight” glow using `VisualTag`.

### 🔹 Character Stat Debugger
- ImGui interface to inspect:
  - Health pool (`StatPoolsSystem`)
  - Equipped weapon data (`EquipmentSystem`)
  - Cyberware (`PlayerPuppet`)

Each of these can be built in ~100–200 lines using the patterns in this guide.



---

## 🔁 Expanded Event & UI Examples

### onInit vs onUpdate vs onShutdown
```lua
registerForEvent("onInit", function()
  print("Initializing...")
  player = Game.GetPlayer()
end)

registerForEvent("onUpdate", function(delta)
  if player and not player:IsInCombat() then
    print("Idle...")
  end
end)

registerForEvent("onShutdown", function()
  print("Mod shutting down...")
end)
```

### Overlay-Safe Drawing
```lua
registerForEvent("onDraw", function()
  if ImGui.Begin("Efficient Panel", ImGuiWindowFlags.AlwaysAutoResize) then
    ImGui.Text("Frame: " .. tostring(os.clock()))
  end
  ImGui.End()
end)
```

---

## 🧱 Full-Scale Mod Templates

### 📦 1. Save Game Item Duplicator
```lua
registerHotkey("DuplicateItems", "Clone All Items", function()
  local ts = Game.GetTransactionSystem()
  local items = ts:GetItemList(Game.GetPlayer())
  for _, item in ipairs(items) do
    ts:GiveItem(Game.GetPlayer(), item:GetID(), item:GetQuantity())
  end
  print("All items duplicated!")
end)
```

### 🧭 2. Dynamic Quest Tracker Overlay
```lua
registerForEvent("onDraw", function()
  local qm = Game.GetQuestsSystem()
  local current = qm:GetTrackedQuestObjective()
  if current then
    ImGui.Begin("Quest Status")
    ImGui.Text("Current Objective: " .. current.objectiveName.value)
    ImGui.End()
  end
end)
```

### 🛒 3. Custom Vendor UI with Prices
```lua
registerForEvent("onDraw", function()
  if ImGui.Begin("Black Market") then
    if ImGui.Button("Buy Rare Item - $1000") then
      local ts = Game.GetTransactionSystem()
      local item = TweakDBID.new("Items.Preset_Nova")
      ts:GiveItem(Game.GetPlayer(), item, 1)
    end
  end
  ImGui.End()
end)
```

---

## 🔍 CET API Deep Dives

### Game Object Structure Example
```lua
local player = Game.GetPlayer()
for key, value in pairs(player) do
  print(key, type(value))
end
```
Use reflection to discover fields and method stubs from handles.

### Visualizing Object Types
```lua
local ent = Game.GetTargetingSystem():GetLookAtObject(Game.GetPlayer(), false, false)
if ent then
  print("Looking at:", ent:GetClassName().value)
end
```

### CET’s Object Trees (Common)
- `PlayerPuppet` → `CharacterObject`
- `VehicleObject` → `GameObject`
- `AIHumanComponent` attached to NPCs
- `InkSystem` controls HUD elements

---

## 📚 TweakDB Reverse Engineering

### Finding Valid Paths
Use TweakDB Explorer (GitHub: WolvenKit) or dump the tree:
```lua
for _, key in ipairs(TweakDB:GetAllRecords()) do
  print(key)
end
```

### Runtime Override Pattern
```lua
TweakDB:SetFlat("Items.Preset_Nova_inline6.value", 0.5) -- e.g. crit chance
TweakDB:SetFlat("Items.Preset_Nova.displayName", "Overclocked Nova")
```

### Mutation Risks
- Temporary only (cleared on restart)
- Can break vanilla balance
- Some fields are write-locked (engine-controlled)

---

## 🧩 Compatibility Design Patterns

### Detect Another Mod
```lua
if _G.SomeOtherMod then
  print("Compatibility mode enabled")
else
  print("Standalone mode")
end
```

### Register Once, Fall Back
```lua
if not _G.Registered_MyMod then
  _G.Registered_MyMod = true
  registerForEvent("onUpdate", mainLoop)
end
```

### Conflict Resolution
```lua
if _G.GlobalConfig then
  _G.GlobalConfig.myFeature = false -- disable mine if theirs is active
end
```


---

## 📚 Deep Dive: Game Systems Hierarchy

CET provides access to several nested systems via the `Game` object.

### StatPoolsSystem
```lua
local stats = Game.GetStatPoolsSystem()
local hp = stats:GetStatValue(Game.GetPlayer(), 'Health')
ImGui.Text("Health: " .. tostring(hp))
```

### WorkspotSystem
```lua
local workspotSys = Game.GetWorkspotSystem()
local isSeated = workspotSys:IsActorInWorkspot(Game.GetPlayer())
```

### ScriptableSystemsContainer
Used to access systems exposed to REDengine:
```lua
local sys = GameInstance:GetScriptableSystemsContainer()
local time = sys:GetTimeSystem()
print("In-game time:", time:GetGameTimeStamp())
```

---

## 🎨 ImGui Layout Tips

### Columns & Spacing
```lua
ImGui.Columns(2)
ImGui.Text("Label:")
ImGui.NextColumn()
ImGui.Text("Value")
ImGui.Columns(1)
```

### State Preservation
```lua
if ImGui.Begin("Toggle Settings", ImGuiWindowFlags.AlwaysAutoResize) then
  if ImGui.CollapsingHeader("Advanced Options") then
    ImGui.Checkbox("Enable Debug", debugFlag)
  end
end
ImGui.End()
```

---

## 🎨 Styling Your ImGui Panel

Use `PushStyleColor` for themed panels:

```lua
ImGui.PushStyleColor(ImGuiCol.WindowBg, 0.1, 0.1, 0.1, 0.9)
ImGui.Begin("Styled Panel")
ImGui.Text("Custom style active.")
ImGui.End()
ImGui.PopStyleColor()
```

---

## 🧰 Lua Utility Libraries

### Inspect Table
```lua
local inspect = require("modules/inspect")
print(inspect(player))
```

### JSON Tools
```lua
local json = require("modules/json")
local data = { key = "value" }
local str = json.encode(data)
```

Create a `modules/` folder and load tools via `require("modules/tool")`.

---

## 📘 Glossary & Quick Index

| Term         | Description |
|--------------|-------------|
| `Vector4`    | 4D position (x, y, z, w) used for movement and spawning |
| `TweakDBID`  | Hashed string used to reference database entries |
| `onInit`     | Fired when mod is loaded |
| `onDraw`     | Fired every frame CET overlay is drawn |
| `Game.*`     | Primary object for accessing Cyberpunk systems |
| `ImGui.*`    | UI API used for drawing CET overlays |
| `CName`      | Engine name string used in postFX and internal labels |
| `StatPoolsSystem` | System for HP, stamina, RAM, and cooldown tracking |
| `registerHotkey` | Binds key input to CET function |

---

## 🛠 Error Recovery & Diagnostics

### Safe JSON Reads
```lua
function safeLoad(path)
  local ok, result = pcall(function()
    local file = io.open(path, "r")
    return file and json.decode(file:read("*a"))
  end)
  if not ok then return {} end
  return result
end
```

### CET Load Errors
- Wrap `require` in `pcall`
- Use `ImGui.TextColored` to display load state
```lua
local success, result = pcall(require, "modules/config")
if not success then
  ImGui.TextColored(1, 0, 0, 1, "Config failed to load.")
end
```

---

## 🧠 In-World 3D Debug Tips

Use `DrawText3D` (if injected properly) or overlay entity labels:

```lua
local ent = Game.GetTargetingSystem():GetLookAtObject(Game.GetPlayer(), false, false)
if ent then
  local name = ent:GetDisplayName()
  ImGui.Text("Looking at: " .. tostring(name))
end
```

This can be extended to track distances, NPC info, etc.

---

## 🧮 Performance Budgeting

### Frame Budgeting
```lua
local lastTime = os.clock()
registerForEvent("onUpdate", function()
  local now = os.clock()
  local delta = now - lastTime
  if delta > 1.0 then
    print("One second has passed.")
    lastTime = now
  end
end)
```

### Frame-Friendly Loops
Avoid iterating every frame without conditions:
```lua
registerForEvent("onUpdate", function()
  if os.clock() % 2 < 0.1 then
    print("Check")
  end
end)
```

Use flags, timers, or key toggles to throttle expensive operations.



---

## 🏁 Conclusion

This guide was designed as a definitive, end-to-end scripting reference for **Cyber Engine Tweaks v1.35.1**. Whether you're a seasoned mod developer or just beginning your journey into Lua scripting within Cyberpunk 2077, this document gives you:

- A grounded introduction to CET’s mod lifecycle and UI systems  
- Deep visibility into native game systems and data structures  
- Modular scripting practices and compatibility patterns  
- Robust examples of real-world mod functionality  
- Expert tips on debugging, performance, and safe scripting

By combining real code, structural insight, and industry-level commentary, this guide serves not only as a learning resource—but also as a toolkit for creating resilient, impactful mods.

> *"Build responsibly. Break nothing. Surprise everyone."*

Good luck out there in Night City—and may your console stay clean and your overlays never crash.

