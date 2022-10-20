+++
title = "Productivity with Hammerspoon"
date = "2022-01-06"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["development"]
+++

[Hammerspoon](https://www.hammerspoon.org/) is a tool for automating OS X tasks. In this post, I'll share my [configuration](https://github.com/ben-maclaurin/hammerspoon/blob/main/init.lua).

For window management, I've modelled the manipulation on Vim keybindings:

- `⌘` + `Shift` + `H` = width / 2 and left align
- `⌘` + `Shift` + `J` = height / 2 and bottom align
- `⌘` + `Shift` + `K` = height / 2 and top align
- `⌘` + `Shift` + `L` = width / 2 and right align

The `⌘` + `Shift` + `↵` combination, which is less synonymous, maximises the target window. The same action is achieveable via the middle mouse button. Here is a snippet of Lua to demonstrate how that works (for the mouse):

```lua
local mouseButton = e:getProperty(hs.eventtap.event.properties['mouseEventButtonNumber'])

if mouseButton == 2 then
    local win = hs.window.focusedWindow()
    local f = win:frame()
    local screen = win:screen()
    local max = screen:frame()

    f.x = max.x 
    f.y = max.y
    f.w = max.w
    f.h = max.h 
    win:setFrame(f)
end
```

I've published the full config file on [GitHub](https://github.com/ben-maclaurin/hammerspoon/blob/main/init.lua).

## Update 20/10/2022

As of 10/22, I use [Rectangle]() with the same keymappings.