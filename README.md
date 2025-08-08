# Roblox ESP Library

A modular, high-performance ESP (Extra Sensory Perception) library for Roblox, designed for maximum extensibility and configurability, supporting R6, R15, Bad Business, and accessories.  
Draws 2D boxes, names, health, skeletons, tracers, offscreen arrows, and more.

---

## Features

- Full/Corner 2D bounding box ESP for players
- Name ESP (with optional friendly dot)
- Healthbar ESP (with color and value)
- Equipped item display
- Head box ESP
- Skeleton ESP (draws Motor6D bone lines)
- Tracers (from customizable origins)
- Offscreen arrows
- Team check, distance check/limit
- Accessory support (hair, hats, backpacks)
- Multiple box update and render modes
- Game-specific mappings (Bad Business)
- Performance & quality settings
- Complete utility API for extension

---

## Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Settings](#settings)
- [Types & Modes](#types--modes)
- [API Reference](#api-reference)
- [Utilities](#utilities)
- [Bounding Box Internals](#bounding-box-internals)
- [ESP Class Structure](#esp-class-structure)
- [Advanced Mapping & Visualizer](#advanced-mapping--visualizer)
- [Performance Notes](#performance-notes)
- [Customization](#customization)
- [License & Credits](#license--credits)

---

## Installation

Place the library as a ModuleScript in your Roblox project.  
Require it in your local/client script:

```lua
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Ifykyklolololol/beebebeb/refs/heads/main/ddddd", true))();
library:Load();

Settings
All ESP features are configurable via library.settings.

Setting	Type	Description	Default
enabled	boolean	Master on/off switch for ESP	true
names	boolean	Show player names	true
boxes	boolean	Show ESP boxes	true
health	boolean	Show health bars	true
equipped	boolean	Show equipped item	true
headBoxes	boolean	Show head box	false
skeletons	boolean	Show skeleton lines	false
tracers	boolean	Show tracer lines	false
offscreenArrows	boolean	Show arrows for offscreen players	true
teamCheck	boolean	Hide ESP for same-team players	true
distanceCheck	boolean	Only show ESP within distanceLimit	false
distanceLimit	number	Max distance to show ESP	1500
friendlyIcon	boolean	Show dot for friendly player names	false
includeAccessories	boolean	Include hats/hair/backpacks in box calc	false
boxUpdateMode	string	Box accuracy mode: Quality, Performance, Balanced	"Quality"
boxRenderMode	string	Box style: Full or Corner	"Corner"
boxTransparency	number	Box background transparency	0.75
performanceBoxWidth	number	Box width for Performance mode	2
performanceBoxHeight	number	Box height for Performance mode	3
tracerOrigin	string	Tracer origin: Top, Bottom, Center, Mouse	"Bottom"
arrowOffset	number	Offscreen arrow distance from screen center	350
arrowWidth	number	Offscreen arrow width	12
arrowHeight	number	Offscreen arrow height	18

Change these before or after :Load() as needed.
