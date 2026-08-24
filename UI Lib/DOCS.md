# WindUI Flat Tabs

`WindUI.lua` is a WindUI build with a flatter default visual style and browser-like page tabs.

## Load

```luau
local WindUI = loadstring(readfile("UI Lib/WindUI.lua"))()
```

The loader must run in an environment that provides `readfile` and `loadstring`.

## CreateWindow

The normal WindUI window options remain available. This build adds `TopTabs` and `CenterTitle`.

```luau
local Window = WindUI:CreateWindow({
    Title = "My Hub",
    Author = "by author",
    Icon = "panels-top-left",
    Size = UDim2.fromOffset(680, 440),

    -- Flat Tabs additions
    TopTabs = true,       -- default: true; puts Window:Tab pages under the topbar
    CenterTitle = true,   -- default: true; centers Title and Author in the topbar

    -- Existing WindUI options
    Radius = 8,
    ElementsRadius = 8,
    Topbar = {
        Height = 46,
        ButtonsType = "Default", -- "Default" or "Mac"
    },
    OpenButton = {
        Title = "Open My Hub",
        Icon = "panels-top-left",
        Enabled = true,
        Draggable = false,
    },
})
```

### New options

| Option | Type | Default | Effect |
| --- | --- | --- | --- |
| `TopTabs` | `boolean` | `true` | Renders tabs as a full-width strip beneath the window topbar. Set `false` to use the original left sidebar. |
| `CenterTitle` | `boolean` | `true` | Centers `Title` and `Author` in the topbar. Set `false` for the original left alignment. |

### Flat defaults

The default window and element radius is `8` instead of the original larger rounded appearance. Pass `Radius` or `ElementsRadius` to override it.

## Tabs

Use the official `Window:Tab` API. In `TopTabs` mode each tab is distributed across the available width; long names are truncated instead of overflowing.

```luau
local Home = Window:Tab({
    Title = "Home",
    Icon = "house",
    Border = true,
})

local Controls = Window:Tab({
    Title = "Controls",
    Icon = "sliders-horizontal",
    Border = true,
})

local Settings = Window:Tab({
    Title = "Settings",
    Icon = "settings",
    Border = true,
})

Window:SelectTab(1)
```

Supported official tab fields include `Title`, `Desc`, `Icon`, `IconColor`, `IconShape`, `IconThemed`, `Locked`, `Border`, `ShowTabTitle`, and `TabTitleAlign`.

## Restoring a closed window

When `OpenButton.Enabled` is true, `Window:Close()` exposes the Open Button. Clicking it calls `Window:Open()` and restores the window. This works on desktop and mobile.

```luau
Window:Close()
-- The configured OpenButton becomes visible.
```

## Minimal example

```luau
local WindUI = loadstring(readfile("UI Lib/WindUI.lua"))()

local Window = WindUI:CreateWindow({
    Title = "Flat Tabs Demo",
    Author = "Example",
    TopTabs = true,
    CenterTitle = true,
    OpenButton = { Title = "Open Demo", Enabled = true },
})

local Home = Window:Tab({ Title = "Home", Icon = "house" })
Home:Button({
    Title = "Notify",
    Icon = "bell",
    Callback = function()
        WindUI:Notify({ Title = "Demo", Content = "Ready" })
    end,
})

Window:SelectTab(1)
```

## Upstream API

For all original elements and APIs, refer to the official WindUI documentation:

- https://footagesus.github.io/treehub-web/docs/windui
- https://footagesus.github.io/treehub-web/docs/windui/window
- https://footagesus.github.io/treehub-web/docs/windui/tab
