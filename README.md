# Aki
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Jason Hub",
   LoadingTitle = "Jason Hub Master Script",
   LoadingSubtitle = "By Jason",
   ConfigurationSaving = { Enabled = false }
})

-- Variables
local Plr = game.Players.LocalPlayer
local RunService = game:GetService("RunService")
local Noclip = false
local Frozen = false
local FakeLagEnabled = false

-- Create FPS/Ping GUI
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local UIStroke = Instance.new("UIStroke")
local InfoLabel = Instance.new("TextLabel")

ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "JasonHubStats"

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.Position = UDim2.new(0.05, 0, 0.05, 0)
MainFrame.Size = UDim2.new(0, 150, 0, 50)
MainFrame.Active = true
MainFrame.Draggable = true

UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

UIStroke.Thickness = 2
UIStroke.Color = Color3.fromRGB(60, 60, 60)
UIStroke.Parent = MainFrame

InfoLabel.Name = "InfoLabel"
InfoLabel.Parent = MainFrame
InfoLabel.BackgroundTransparency = 1
InfoLabel.Size = UDim2.new(1, 0, 1, 0)
InfoLabel.Font = Enum.Font.FredokaOne
InfoLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
InfoLabel.TextSize = 16
InfoLabel.Text = "FPS: ... | Ping: ..."

-- Stats Loop
local lastUpdate = 0
RunService.RenderStepped:Connect(function()
    local now = tick()
    if now - lastUpdate >= 0.5 then
        local fps = math.floor(1 / RunService.RenderStepped:Wait())
        local ping = math.floor(game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValue())
        InfoLabel.Text = "FPS: " .. fps .. "\n Ping: " .. ping .. "ms"
        lastUpdate = now
    end
end)

-- Tabs
local MainTab = Window:CreateTab("Main", 4483362458)
local MoveTab = Window:CreateTab("Movement", 4483362458)
local UniTab = Window:CreateTab("Universals", 4483362458)

--- MAIN TAB ---

MainTab:CreateToggle({
   Name = "Fake Lag",
   CurrentValue = false,
   Flag = "FakeLagFlag",
   Callback = function(Value)
       FakeLagEnabled = Value
       task.spawn(function()
           while FakeLagEnabled do
               if Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart") then
                   Plr.Character.HumanoidRootPart.Anchored = true
                   task.wait(0.3)
                   Plr.Character.HumanoidRootPart.Anchored = false
                   task.wait(0.5)
               else
                   task.wait()
               end
           end
       end)
   end,
})

MainTab:CreateButton({
   Name = "Rejoin Server",
   Callback = function()
       game:GetService("TeleportService"):Teleport(game.PlaceId, Plr)
   end,
})

MainTab:CreateToggle({
   Name = "FPS Booster (On/Off)",
   CurrentValue = false,
   Flag = "FPSFlag",
   Callback = function(Value)
       if Value then
           for _, v in pairs(game:GetDescendants()) do
               if v:IsA("Decal") or v:IsA("Texture") then v.Transparency = 1
               elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then v.Enabled = false end
           end
       else
           for _, v in pairs(game:GetDescendants()) do
               if v:IsA("Decal") or v:IsA("Texture") then v.Transparency = 0
               elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then v.Enabled = true end
           end
       end
   end,
})

MainTab:CreateToggle({
   Name = "Freeze Character",
   CurrentValue = false,
   Flag = "FreezeFlag",
   Callback = function(Value)
       Frozen = Value
       if Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart") then
           Plr.Character.HumanoidRootPart.Anchored = Frozen
       end
   end,
})

MainTab:CreateButton({
   Name = "Fly V3 / V4",
   Callback = function()
       loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fly-v3-102059"))()
   end,
})

--- MOVEMENT TAB ---

MoveTab:CreateSlider({
   Name = "Walk Speed",
   Range = {16, 500},
   Increment = 1,
   CurrentValue = 16,
   Flag = "WS",
   Callback = function(Value)
       if Plr.Character and Plr.Character:FindFirstChild("Humanoid") then
           Plr.Character.Humanoid.WalkSpeed = Value
       end
   end,
})

MoveTab:CreateButton({
   Name = "Normal Speed",
   Callback = function()
       if Plr.Character and Plr.Character:FindFirstChild("Humanoid") then
           Plr.Character.Humanoid.WalkSpeed = 16
       end
   end,
})

MoveTab:CreateSlider({
   Name = "Jump Power",
   Range = {50, 500},
   Increment = 1,
   CurrentValue = 50,
   Flag = "JP",
   Callback = function(Value)
       if Plr.Character and Plr.Character:FindFirstChild("Humanoid") then
           Plr.Character.Humanoid.UseJumpPower = true
           Plr.Character.Humanoid.JumpPower = Value
       end
   end,
})

MoveTab:CreateButton({
   Name = "Normal Jump",
   Callback = function()
       if Plr.Character and Plr.Character:FindFirstChild("Humanoid") then
           Plr.Character.Humanoid.JumpPower = 50
       end
   end,
})

MoveTab:CreateSlider({
   Name = "World Gravity",
   Range = {0, 196},
   Increment = 1,
   CurrentValue = 196,
   Flag = "GravSlider",
   Callback = function(Value)
       workspace.Gravity = Value
   end,
})

MoveTab:CreateButton({
   Name = "Normal Gravity",
   Callback = function()
       workspace.Gravity = 196.2
   end,
})

--- UNIVERSALS TAB ---

UniTab:CreateButton({
   Name = "Infinite Yield",
   Callback = function()
       loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Infinite-yeild-fe-14386"))()
   end,
})

UniTab:CreateButton({
   Name = "Nameless Admin (NA)",
   Callback = function()
       loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Nameless-admin-NEWEST-124628"))()
   end,
})

UniTab:CreateButton({
   Name = "Enable Anti-AFK",
   Callback = function()
       local VirtualUser = game:GetService("VirtualUser")
       game.Players.LocalPlayer.Idled:Connect(function()
           VirtualUser:CaptureController()
           VirtualUser:ClickButton2(Vector2.new())
           Rayfield:Notify({Title = "Anti-AFK", Content = "Stayed active to prevent kick!", Duration = 2})
       end)
       Rayfield:Notify({Title = "Jason Hub", Content = "Anti-AFK is now active.", Duration = 3})
   end,
})

Rayfield:Notify({
   Title = "Jason Hub",
   Content = "Are you enjoying it?",
   Duration = 6,
   Image = 4483362458,
})

