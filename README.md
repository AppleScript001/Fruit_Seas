local Library = loadstring(game:HttpGet("https://pastebin.com/raw/KNBp0LRy"), "Coastified UI")()
repeat wait() until game:IsLoaded()
game:GetService("Players").LocalPlayer.Idled:connect(function()
game:GetService("VirtualUser"):ClickButton2(Vector2.new())
end)

local Window = Library:NewWindow("Pirate's Destiny")

local Section = Window:NewSection("AutoFarm")

local PlayerTP1
local TweenService = game:GetService("TweenService")

local Dropdown = Section:CreateDropdown("Select Mobs!", {"Bandit", "Bandit Leader"}, 0, function(t)
    PlayerTP1 = t
end)

local Toggle = Section:CreateToggle("Auto [Attack]", function(Value)
_G.Attack = Value
while _G.Attack do
wait(1)  -- Wait for 1 second before checking for enemies
pcall(function()
for i,v in pairs(workspace.Mobs.FooshaVillage:GetDescendants()) do
if v.Name == PlayerTP1 then
if v.Humanoid.Health > 0 then
repeat
    local toolName = "Combat" -- Replace "YourToolNameHere" with the name of your tool
    
    local LocalPlayer = game:GetService("Players").LocalPlayer
for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
    if v:IsA('Tool') and v.Name == toolName then
        v.Parent = LocalPlayer.Character
        break -- Stop the loop after picking up the tool
    end
end
local args = {
    [1] = workspace:WaitForChild("Mobs"):WaitForChild("FooshaVillage"):WaitForChild(PlayerTP1):WaitForChild("HumanoidRootPart"),
    [2] = "1",
    [3] = 2
}

game:GetService("ReplicatedStorage"):WaitForChild("RemoteEvents"):WaitForChild("MeleeHitEvent"):FireServer(unpack(args))

game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,5)
wait()  -- Wait a short time before checking again
until not _G.Attack or v.Humanoid.Health <= 0
end
end
end
end)
local Player = game.Players.LocalPlayer
local function onCharacterAdded(character)
    character.Archivable = false
    character:WaitForChild("HumanoidRootPart").Anchored = false
    if Player.Character then
            onCharacterAdded(Player.Character)
    end
end
    
Player.CharacterAdded:Connect(onCharacterAdded)
end
end)
