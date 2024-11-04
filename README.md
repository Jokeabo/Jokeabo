local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Create the main window
local Window = Rayfield:CreateWindow({
   Name = "One Star Hub (MM2)",
   LoadingTitle = "Made by Resh",
   LoadingSubtitle = "Loading...",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "MurderMysteryFre", -- Custom folder for your hub/game
      FileName = "OneStarScript"
   }
})

-- Create Tabs
local TradingTab = Window:CreateTab("Trading", 4483362458)
local RolesTab = Window:CreateTab("Roles", 4483362458)
local MiscTab = Window:CreateTab("Misc", 4483362458)

-- Trading Section
local TradingSection = TradingTab:CreateSection("Trade Options")

-- Player Name Input
local playerNameTextbox = ""
local TextBox = TradingTab:CreateInput({
    Name = "Player Name",
    PlaceholderText = "Enter player name...",
    RemoveTextAfterFocusLost = false,
    Callback = function(Text)
        playerNameTextbox = Text
    end,
})

-- Force Trade Button
TradingTab:CreateButton({
    Name = "Force Trade",
    Callback = function()
        if playerNameTextbox and playerNameTextbox ~= "" then
            local player = game.Players:FindFirstChild(playerNameTextbox)

            if player then
                local args = { [1] = player }
                game:GetService("ReplicatedStorage"):WaitForChild("Trade"):WaitForChild("SendRequest"):InvokeServer(unpack(args))
                game:GetService("ReplicatedStorage"):WaitForChild("Trade"):WaitForChild("AcceptRequest"):FireServer()

                Rayfield:Notify({
                    Title = "Trade System",
                    Content = "Force Traded Player: " .. playerNameTextbox,
                    Duration = 5,
                    Image = 4483362458,
                    Actions = {
                        Ignore = { Name = "Okay!", Callback = function() end }
                    },
                })
            else
                Rayfield:Notify({ Title = "Trade System", Content = "Player not found.", Duration = 5, Image = 4483362458 })
            end
        else
            Rayfield:Notify({ Title = "Trade System", Content = "Please enter a valid player name.", Duration = 5, Image = 4483362458 })
        end
    end,
})

-- Roles Section
local RolesSection = RolesTab:CreateSection("Role Info")

-- Chat Expose Roles Button
RolesTab:CreateButton({
    Name = "Chat Expose Roles",
    Callback = function()
        local allPlayers = game.Players:GetPlayers()
        for _, player in pairs(allPlayers) do
            local backpack = player:FindFirstChild("Backpack")
            if backpack then
                if backpack:FindFirstChild("Knife") then
                    game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest"):FireServer(player.Name .. ": Has The Knife", "normalchat")
                end
                if backpack:FindFirstChild("Gun") then
                    game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest"):FireServer(player.Name .. ": Has The Gun", "normalchat")
                end
            end
        end
    end,
})

-- Labels for Murderer and Sheriff
local MurdererLabel = RolesTab:CreateLabel("Murderer is: Unknown")
local SheriffLabel = RolesTab:CreateLabel("Sheriff is: Unknown")

-- Function to Update Roles
local function updateRolesInfo()
    while true do
        local players = game:GetService("Players"):GetPlayers()
        local murderer, sheriff = "Unknown", "Unknown"
        for _, player in ipairs(players) do
            if player.Character then
                local backpack = player.Backpack
                if backpack then
                    if backpack:FindFirstChild("Knife") then murderer = player.Name end
                    if backpack:FindFirstChild("Gun") then sheriff = player.Name end
                end
            end
        end
        MurdererLabel:Set("Murderer is: " .. murderer)
        SheriffLabel:Set("Sheriff is: " .. sheriff)
        wait(1)
    end
end
coroutine.wrap(updateRolesInfo)()

-- Miscellaneous Tab
local MiscSection = MiscTab:CreateSection("Miscellaneous Options")

-- Get Every Emote Button
MiscTab:CreateButton({
    Name = "Get Every Emote",
    Callback = function()
        local PlayerGui = game.Players.LocalPlayer:WaitForChild("PlayerGui")
        local Emotes = PlayerGui:WaitForChild("MainGUI"):WaitForChild("Game"):FindFirstChild("Emotes")
        if Emotes then
            require(game:GetService("ReplicatedStorage").Modules.EmoteModule).GeneratePage({"headless", "zombie", "zen", "ninja", "floss", "dab", "sit"}, Emotes, "Free Emotes")
            Rayfield:Notify({ Title = "Emotes", Content = "Successfully added emotes!", Duration = 6.5, Image = 4483362458 })
        end
    end,
})

-- Get Every Gun/Knife Button
MiscTab:CreateButton({
    Name = "Get Every Gun/Knife!",
    Callback = function()
        local WeaponOwnRange = { min = 999999999, max = 999999999 }
        local DataBase, PlayerData = getrenv()._G.Database, getrenv()._G.PlayerData
        local newOwned = {}
        for i, v in next, DataBase.Item do
            newOwned[i] = math.random(WeaponOwnRange.min, WeaponOwnRange.max) -- newOwned[Weapon]: ItemCount
        end
        local PlayerWeapons = PlayerData.Weapons
        game:GetService("RunService"):BindToRenderStep("InventoryUpdate", 0, function()
            PlayerWeapons.Owned = newOwned
        end)
        game.Players.LocalPlayer.Character.Humanoid.Health = 0
        Rayfield:Notify({ Title = "Weapon Update", Content = "Got every weapon/gun (Client)", Duration = 3 })
    end,
})

-- Search Feature
local SearchBox = Window:CreateInput({
    Name = "Search Scripts",
    PlaceholderText = "Search for a script...",
    RemoveTextAfterFocusLost = false,
    Callback = function(Text)
        -- Implement search functionality here
        -- You can filter the buttons based on the Text input
    end,
})

-- Animation and UI Improvements
-- Add animations for button presses, transitions, and visual effects as needed.

-- Finalize GUI
Rayfield:Notify({
    Title = "Welcome!",
    Content = "Welcome to One Star Hub!",
    Duration = 5,
    Image = 4483362458
})

-- Update for any additional features and tweaks as necessary.
