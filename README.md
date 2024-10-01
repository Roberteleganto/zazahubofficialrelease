local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local Window = Rayfield:CreateWindow({
   Name = "Zazahub🍃",
   LoadingTitle = "Zazahub🍃",
   LoadingSubtitle = "ui by Sirius and scripts made by @droidhmp1",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Zazahub🍃"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },
   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided",
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})
local Tab = Window:CreateTab("Main", 4483362458) -- Title, Image
local Button = Tab:CreateButton({
   Name = "Camlock (Click e) (locks to nearest player)",
   Callback = function()
   -- The function that takes place when the button is pressed
   local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- Variable to track if the mouse is locked
local isMouseLocked = false
local targetPlayer = nil

-- Function to get the closest player
local function getClosestPlayer()
    local closestPlayer = nil
    local closestDistance = math.huge

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (LocalPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).magnitude
            if distance < closestDistance then
                closestDistance = distance
                closestPlayer = player
            end
        end
    end

    return closestPlayer
end

-- Function to lock the mouse onto the target player
local function lockMouse()
    targetPlayer = getClosestPlayer()

    if targetPlayer and targetPlayer.Character then
        local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
        Camera.CFrame = CFrame.new(Camera.CFrame.Position, targetPosition)
    end
end

-- Function to update the camera to follow the target player
local function updateCamera()
    if isMouseLocked and targetPlayer and targetPlayer.Character then
        local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
        Camera.CFrame = CFrame.new(Camera.CFrame.Position, targetPosition)
    end
end

-- Input handling
UIS.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end

    if input.KeyCode == Enum.KeyCode.E then
        isMouseLocked = not isMouseLocked
        if isMouseLocked then
            lockMouse()
        else
            targetPlayer = nil
        end
    end
end)

-- RunService RenderStep to keep updating the camera
RunService.RenderStepped:Connect(updateCamera)
   end,
})
local Button = Tab:CreateButton({
   Name = "OP orbit gui made by @droidhmp1 on discord",
   Callback = function()
   -- The function that takes place when the button is pressed
   -- Variables for the player and target to orbit
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local rootPart = character:WaitForChild("HumanoidRootPart")

-- Orbit parameters
local orbiting = false
local orbitDistance = 10
local orbitSpeed = 50
local targetPlayer = nil -- Target player to orbit around
local angle = 0

-- Services
local runService = game:GetService("RunService")
local players = game:GetService("Players")

-- Create GUI for controlling orbit
local screenGui = Instance.new("ScreenGui", player.PlayerGui)
screenGui.Name = "OrbitGui"

-- Create a draggable frame
local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 400, 0, 350) -- Larger size
frame.Position = UDim2.new(0.3, 0, 0.2, 0) -- Initial position
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BackgroundTransparency = 0.2 -- 20% transparency
frame.Active = true
frame.Draggable = true -- Make it draggable

-- Create a background for the GUI
local trollFaceImage = Instance.new("ImageLabel", frame)
trollFaceImage.Size = UDim2.new(1, 0, 1, 0)
trollFaceImage.Position = UDim2.new(0, 0, 0, 0)
trollFaceImage.Image = "" -- Trollface image asset ID
trollFaceImage.BackgroundTransparency = 1

-- Function to create styled text labels
local function createTextLabel(parent, text, pos, size)
    local label = Instance.new("TextLabel", parent)
    label.Text = text
    label.Position = pos
    label.Size = size
    label.TextColor3 = Color3.new(0, 0, 0.55) -- Dark blue text
    label.BackgroundColor3 = Color3.new(0, 0, 0)
    label.BackgroundTransparency = 0.5
    label.Font = Enum.Font.SourceSans
    label.TextScaled = true
    return label
end

-- Create Distance label and slider
createTextLabel(frame, "Distance", UDim2.new(0, 10, 0, 30), UDim2.new(0.4, 0, 0, 40))

local distanceSlider = Instance.new("TextBox", frame)
distanceSlider.Position = UDim2.new(0.5, 0, 0, 30)
distanceSlider.Size = UDim2.new(0.4, 0, 0, 40)
distanceSlider.Text = tostring(orbitDistance)
distanceSlider.TextColor3 = Color3.new(0, 0, 0.55)
distanceSlider.BackgroundColor3 = Color3.new(0, 0, 0)
distanceSlider.BackgroundTransparency = 0.5
distanceSlider.Font = Enum.Font.SourceSans
distanceSlider.TextScaled = true

-- Create Speed label and slider
createTextLabel(frame, "Speed", UDim2.new(0, 10, 0, 100), UDim2.new(0.4, 0, 0, 40))

local speedSlider = Instance.new("TextBox", frame)
speedSlider.Position = UDim2.new(0.5, 0, 0, 100)
speedSlider.Size = UDim2.new(0.4, 0, 0, 40)
speedSlider.Text = tostring(orbitSpeed)
speedSlider.TextColor3 = Color3.new(0, 0, 0.55)
speedSlider.BackgroundColor3 = Color3.new(0, 0, 0)
speedSlider.BackgroundTransparency = 0.5
speedSlider.Font = Enum.Font.SourceSans
speedSlider.TextScaled = true

-- Create Target Player label and input
createTextLabel(frame, "Target Player", UDim2.new(0, 10, 0, 170), UDim2.new(0.9, 0, 0, 40))

local targetInput = Instance.new("TextBox", frame)
targetInput.Position = UDim2.new(0, 0, 0, 220)
targetInput.Size = UDim2.new(1, 0, 0, 40)
targetInput.Text = ""
targetInput.TextColor3 = Color3.new(0, 0, 0.55)
targetInput.BackgroundColor3 = Color3.new(0, 0, 0)
targetInput.BackgroundTransparency = 0.5
targetInput.Font = Enum.Font.SourceSans
targetInput.TextScaled = true

-- Button to start/stop orbiting
local toggleButton = Instance.new("TextButton", frame)
toggleButton.Text = "Start Orbit"
toggleButton.Position = UDim2.new(0, 0, 1, -50)
toggleButton.Size = UDim2.new(1, 0, 0, 40)
toggleButton.TextColor3 = Color3.new(0, 0, 0.55)
toggleButton.BackgroundColor3 = Color3.new(0, 0, 0)
toggleButton.BackgroundTransparency = 0.5
toggleButton.Font = Enum.Font.SourceSans
toggleButton.TextScaled = true

-- Function to get the target player by name
local function getTargetPlayer(name)
    for _, otherPlayer in pairs(players:GetPlayers()) do
        if otherPlayer.Name == name and otherPlayer ~= player then
            return otherPlayer
        end
    end
    return nil
end

-- Function to start orbiting
local function startOrbit()
    if not targetPlayer then return end

    local targetCharacter = targetPlayer.Character
    if not targetCharacter then return end

    local targetRoot = targetCharacter:WaitForChild("HumanoidRootPart")
    orbiting = true
    toggleButton.Text = "Stop Orbit"

    -- Orbit logic
    runService.RenderStepped:Connect(function(deltaTime)
        if orbiting and targetRoot then
            angle = angle + (orbitSpeed * deltaTime) -- Update angle by speed

            -- Calculate new position based on angle and distance
            local x = targetRoot.Position.X + math.cos(angle) * orbitDistance
            local z = targetRoot.Position.Z + math.sin(angle) * orbitDistance

            -- Set player's position to the new orbit point
            rootPart.CFrame = CFrame.new(Vector3.new(x, targetRoot.Position.Y, z))
        end
    end)
end

-- Function to stop orbiting
local function stopOrbit()
    orbiting = false
    toggleButton.Text = "Start Orbit"
end

-- Toggle orbit when the button is clicked
toggleButton.MouseButton1Click:Connect(function()
    if orbiting then
        stopOrbit()
    else
        targetPlayer = getTargetPlayer(targetInput.Text)
        if targetPlayer then
            startOrbit()
        end
    end
end)

-- Update orbit distance when the input changes
distanceSlider.FocusLost:Connect(function()
    local value = tonumber(distanceSlider.Text)
    if value then
        orbitDistance = math.clamp(value, 10, 999) -- Clamp between 10 and 999
    end
end)

-- Update orbit speed when the input changes
speedSlider.FocusLost:Connect(function()
    local value = tonumber(speedSlider.Text)
    if value then
        orbitSpeed = math.clamp(value, 10, 999) -- Clamp between 10 and 999
    end
end)
   end,
})
local Toggle = Tab:CreateToggle({
   Name = "Esp also made by me kinda works idk if toggle works",
   CurrentValue = false,
   Flag = "Toggle1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
   -- The function that takes place when the toggle is pressed
   -- The variable (Value) is a boolean on whether the toggle is true or false
   local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = game.Workspace.CurrentCamera

-- Function to create or update highlights for other players
local function highlightOtherPlayers()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local character = player.Character
            if character and character:FindFirstChild("Humanoid") then
                -- Create or update the Highlight
                local highlight = character:FindFirstChild("Highlight") or Instance.new("Highlight")
                highlight.Adornee = character
                highlight.FillColor = player.TeamColor.Color
                highlight.FillTransparency = 0.5
                highlight.OutlineColor = player.TeamColor.Color
                highlight.OutlineTransparency = 0.5
                highlight.Parent = character

                -- Create a BillboardGui for the name, health, and distance
                local billboardGui = character:FindFirstChild("BillboardGui") or Instance.new("BillboardGui")
                billboardGui.Adornee = character
                billboardGui.Size = UDim2.new(0, 200, 0, 50)
                billboardGui.StudsOffset = Vector3.new(3, 1, 0) -- Adjust to the right side
                billboardGui.Parent = character

                -- Create a TextLabel to show the player's info
                local textLabel = billboardGui:FindFirstChild("TextLabel") or Instance.new("TextLabel")
                textLabel.Size = UDim2.new(1, 0, 1, 0)
                textLabel.BackgroundTransparency = 1
                textLabel.TextColor3 = Color3.new(1, 1, 1)
                textLabel.TextStrokeTransparency = 0.5
                textLabel.Font = Enum.Font.SourceSans -- Change to Enum.Font.Montserrat if available
                textLabel.TextScaled = true
                textLabel.Parent = billboardGui
            end
        end
    end
end

-- Function to update player information
local function updatePlayerInfo()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local character = player.Character
            if character and character:FindFirstChild("Humanoid") then
                local humanoid = character.Humanoid
                local billboardGui = character:FindFirstChild("BillboardGui")
                
                if billboardGui then
                    local distance = (LocalPlayer.Character.HumanoidRootPart.Position - character.HumanoidRootPart.Position).magnitude
                    local playerName = player.Name:upper() -- Convert player name to uppercase
                    local health = humanoid.Health
                    local maxHealth = humanoid.MaxHealth
                    
                    -- Update text label with distance, name, and health
                    local textLabel = billboardGui:FindFirstChild("TextLabel")
                    if textLabel then
                        textLabel.Text = string.format("%s\nDistance: %.2f\nHealth: %d/%d", playerName, distance, health, maxHealth)
                    end
                end
            end
        end
    end
end

-- Main loop to continuously update
while true do
    highlightOtherPlayers()
    updatePlayerInfo()
    wait(1) -- Update every second
end

   end,
})
local Button = Tab:CreateButton({
   Name = "Invisible toggle press e.",
   Callback = function()
   -- The function that takes place when the button is pressed
   --[[Invisibility Toggle

You can find the orginal concept here: https://v3rmillion.net/showthread.php?tid=544634

This method clones the character locally, teleports the real character to a safe location, then sets the character to the clone.
Basically, your real character is in the sky while you are on the ground.


Because of the way this works, games with a decent anti-cheat will fuck this up.
If you turn it off, you have to go to a safe place before going invisible.

Written by: BitingTheDust ; https://v3rmillion.net/member.php?action=profile&uid=1628149
]]
--Settings:
local ScriptStarted = false
local Keybind = "E" --Set to whatever you want, has to be the name of a KeyCode Enum.
local Transparency = true --Will make you slightly transparent when you are invisible. No reason to disable.
local NoClip = false --Will make your fake character no clip.

local Player = game:GetService("Players").LocalPlayer
local RealCharacter = Player.Character or Player.CharacterAdded:Wait()

local IsInvisible = false

RealCharacter.Archivable = true
local FakeCharacter = RealCharacter:Clone()
local Part
Part = Instance.new("Part", workspace)
Part.Anchored = true
Part.Size = Vector3.new(200, 1, 200)
Part.CFrame = CFrame.new(0, -500, 0) --Set this to whatever you want, just far away from the map.
Part.CanCollide = true
FakeCharacter.Parent = workspace
FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

for i, v in pairs(RealCharacter:GetChildren()) do
  if v:IsA("LocalScript") then
      local clone = v:Clone()
      clone.Disabled = true
      clone.Parent = FakeCharacter
  end
end
if Transparency then
  for i, v in pairs(FakeCharacter:GetDescendants()) do
      if v:IsA("BasePart") then
          v.Transparency = 0.7
      end
  end
end
local CanInvis = true
function RealCharacterDied()
  CanInvis = false
  RealCharacter:Destroy()
  RealCharacter = Player.Character
  CanInvis = true
  isinvisible = false
  FakeCharacter:Destroy()
  workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid

  RealCharacter.Archivable = true
  FakeCharacter = RealCharacter:Clone()
  Part:Destroy()
  Part = Instance.new("Part", workspace)
  Part.Anchored = true
  Part.Size = Vector3.new(200, 1, 200)
  Part.CFrame = CFrame.new(9999, 9999, 9999) --Set this to whatever you want, just far away from the map.
  Part.CanCollide = true
  FakeCharacter.Parent = workspace
  FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

  for i, v in pairs(RealCharacter:GetChildren()) do
      if v:IsA("LocalScript") then
          local clone = v:Clone()
          clone.Disabled = true
          clone.Parent = FakeCharacter
      end
  end
  if Transparency then
      for i, v in pairs(FakeCharacter:GetDescendants()) do
          if v:IsA("BasePart") then
              v.Transparency = 0.7
          end
      end
  end
 RealCharacter.Humanoid.Died:Connect(function()
 RealCharacter:Destroy()
 FakeCharacter:Destroy()
 end)
 Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
end
RealCharacter.Humanoid.Died:Connect(function()
 RealCharacter:Destroy()
 FakeCharacter:Destroy()
 end)
Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
local PseudoAnchor
game:GetService "RunService".RenderStepped:Connect(
  function()
      if PseudoAnchor ~= nil then
          PseudoAnchor.CFrame = Part.CFrame * CFrame.new(0, 5, 0)
      end
       if NoClip then
      FakeCharacter.Humanoid:ChangeState(11)
       end
  end
)

PseudoAnchor = FakeCharacter.HumanoidRootPart
local function Invisible()
  if IsInvisible == false then
      local StoredCF = RealCharacter.HumanoidRootPart.CFrame
      RealCharacter.HumanoidRootPart.CFrame = FakeCharacter.HumanoidRootPart.CFrame
      FakeCharacter.HumanoidRootPart.CFrame = StoredCF
      RealCharacter.Humanoid:UnequipTools()
      Player.Character = FakeCharacter
      workspace.CurrentCamera.CameraSubject = FakeCharacter.Humanoid
      PseudoAnchor = RealCharacter.HumanoidRootPart
      for i, v in pairs(FakeCharacter:GetChildren()) do
          if v:IsA("LocalScript") then
              v.Disabled = false
          end
      end

      IsInvisible = true
  else
      local StoredCF = FakeCharacter.HumanoidRootPart.CFrame
      FakeCharacter.HumanoidRootPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
     
      RealCharacter.HumanoidRootPart.CFrame = StoredCF
     
      FakeCharacter.Humanoid:UnequipTools()
      Player.Character = RealCharacter
      workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid
      PseudoAnchor = FakeCharacter.HumanoidRootPart
      for i, v in pairs(FakeCharacter:GetChildren()) do
          if v:IsA("LocalScript") then
              v.Disabled = true
          end
      end
      IsInvisible = false
  end
end

game:GetService("UserInputService").InputBegan:Connect(
  function(key, gamep)
      if gamep then
          return
      end
      if key.KeyCode.Name:lower() == Keybind:lower() and CanInvis and RealCharacter and FakeCharacter then
          if RealCharacter:FindFirstChild("HumanoidRootPart") and FakeCharacter:FindFirstChild("HumanoidRootPart") then
              Invisible()
          end
      end
  end
)
local Sound = Instance.new("Sound",game:GetService("SoundService"))
Sound.SoundId = "rbxassetid://232127604"
Sound:Play()
game:GetService("StarterGui"):SetCore("SendNotification",{["Title"] = "Invisible Toggle Loaded",["Text"] = "Press "..Keybind.." to become change visibility.",["Duration"] = 20,["Button1"] = "Okay."})

   end,
})
local Tab = Window:CreateTab("Admin guis", 4483362458) -- Title, Image
local Section = Tab:CreateSection("Guis")
local Button = Tab:CreateButton({
   Name = "Inf yield remasatered",
   Callback = function()
   -- The function that takes place when the button is pressed
   loadstring(game:HttpGet("https://storage.iyr.lol/legacy-iyr/source"))()
   end,
})
