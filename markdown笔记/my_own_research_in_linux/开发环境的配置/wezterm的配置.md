/*************************************************************************
 * @File Name: wezterm的配置.md

 * @Description: 配置wezterm，摘自https://www.bboy.app/

 * @Author: Sevens Zhu

 * @Created Time: Tue Jul  8 15:38:02 2025
 ************************************************************************/

 下载好wezterm，打开.config文档，若无创建wezterm文件夹，创建wezterm.lua

```lua
-- Import the wezterm module
local wezterm = require 'wezterm'
-- Creates a config object which we will be adding our config to
local config = wezterm.config_builder()

-- (This is where our config will go)
config.color_scheme = 'Tokyo Night'
config.font =wezterm.font('JetBrains Mono', { weight = 'Bold', italic = false })
config.font_size = 17
config.window_background_opacity = 0.8
config.macos_window_background_blur = 30
config.window_frame = {
  font =wezterm.font('JetBrains Mono', { weight = 'Bold'}),
  font_size = 13,
}

wezterm.on('update-status', function(window)
  -- Grab the utf8 character for the "powerline" left facing
  -- solid arrow.
  local SOLID_LEFT_ARROW = utf8.char(0xe0b2)

  -- Grab the current window's configuration, and from it the
  -- palette (this is the combination of your chosen colour scheme
  -- including any overrides).
  local color_scheme = window:effective_config().resolved_palette
  local bg = color_scheme.background
  local fg = color_scheme.foreground
  local success, stdout, stderr = wezterm.run_child_process { "curl", "-s", "https://realip.cc/simple" }
  local public_ip=stdout
  local date = wezterm.strftime("%Y-%m-%d %H:%M:%S")
  wezterm.sleep_ms(6000)

  if not public_ip then
    public_ip = 'N/A'
  end

  window:set_right_status(wezterm.format({
    -- First, we draw the arrow...
    { Background = { Color = 'none' } },
    { Foreground = { Color = bg } },
    { Text = SOLID_LEFT_ARROW },
    -- Then we draw our text
    { Background = { Color = bg } },
    { Foreground = { Color = fg } },
    { Text = ' ' .. wezterm.hostname() .. ' | IP: ' .. public_ip .. ' | ' .. date ..' ' },
  }))
end)


-- Returns our config to be evaluated. We must always do this at the bottom of this file
return config
```

