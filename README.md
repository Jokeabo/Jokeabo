-- Library setup with OrionLib
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Window = OrionLib:MakeWindow({
    Name = "Cat Gojo Executor",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "CatGojoExecutorConfig",
    IntroEnabled = true,
    IntroText = "Welcome to Cat Gojo Executor",
    IntroIcon = "rbxassetid://12345678",  -- Replace with actual icon asset ID
    Icon = "rbxassetid://12345678",  -- Replace with actual icon asset ID
    CloseCallback = function() print("Cat Gojo Executor Closed") end
})

-- Sidebar and Scripts Section
local scripts = {
    {name = "The Strongest Battlegrounds", icon = "rbxassetid://Icon1"},
    {name = "Pet Simulator 99", icon = "rbxassetid://Icon2"},
    {name = "Blox Fruits", icon = "rbxassetid://Icon3"},
    {name = "King Legacy", icon = "rbxassetid://Icon4"},
    {name = "Public Bathroom Simulator", icon = "rbxassetid://Icon5"},
    {name = "Jujutsu Shenanigans", icon = "rbxassetid://Icon6"},
    {name = "Doors", icon = "rbxassetid://Icon7"},
}

-- Main Section for Executor GUI
local ExecutorTab = Window:MakeTab({
    Name = "Executor",
    Icon = "rbxassetid://IconExecutor",
    PremiumOnly = false
})

-- Script Selection Dropdown
ExecutorTab:AddDropdown({
    Name = "Select Script",
    Default = "Choose a script",
    Options = {
        "The Strongest Battlegrounds", 
        "Pet Simulator 99", 
        "Blox Fruits", 
        "King Legacy", 
        "Public Bathroom Simulator", 
        "Jujutsu Shenanigans", 
        "Doors"
    },
    Callback = function(selectedScript)
        print("Selected script:", selectedScript)
    end
})

-- Execute Button
ExecutorTab:AddButton({
    Name = "Execute Script",
    Callback = function()
        print("Executing selected script...")
        -- Place script execution functionality here
    end
})

-- Credits Section
ExecutorTab:AddParagraph("Credits", "Made by Cat Gojo\nSpecial thanks to my friends")

-- Configuration Toggle for Cleaner Layout
local SettingsTab = Window:MakeTab({
    Name = "Settings",
    Icon = "rbxassetid://IconSettings",
    PremiumOnly = false
})

SettingsTab:AddToggle({
    Name = "Enable Notifications",
    Default = false,
    Callback = function(state)
        print("Notifications enabled:", state)
    end
})

SettingsTab:AddButton({
    Name = "Reset Settings",
    Callback = function()
        print("Settings have been reset")
        -- Reset functionality can be added here
    end
})

SettingsTab:AddParagraph("Info", "Config saved in CatGojoExecutorConfig folder")

-- Initialization of UI
OrionLib:Init()
