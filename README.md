local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local humanoid = player.Character and player.Character:WaitForChild("Humanoid")

local autoJumpEnabled = false 
local runService = game:GetService("RunService")

local screenGui = Instance.new("ScreenGui")
screenGui.Parent = playerGui

local statusLabel = Instance.new("TextButton") 
statusLabel.Parent = screenGui
statusLabel.Size = UDim2.new(0, 200, 0, 50)
statusLabel.Position = UDim2.new(0.5, -100, 0.1, 0) 
statusLabel.BackgroundTransparency = 0.2
statusLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
statusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
statusLabel.Font = Enum.Font.GothamBold
statusLabel.TextSize = 24
statusLabel.Text = "Bhop: OFF"
statusLabel.Active = true 
statusLabel.Draggable = true 

-- Credit label (parented to statusLabel)
local creditLabel = Instance.new("TextLabel")
creditLabel.Parent = statusLabel 
creditLabel.Size = UDim2.new(1, 0, 0, 20) 
creditLabel.Position = UDim2.new(0, 0, 1, 5) 
creditLabel.BackgroundTransparency = 1 
creditLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
creditLabel.Font = Enum.Font.Gotham
creditLabel.TextSize = 12 
creditLabel.Text = "Made by kevinivy100"
creditLabel.TextScaled = true 

-- Keybind indicator (non-draggable)
local keybindLabel = Instance.new("TextLabel")
keybindLabel.Parent = screenGui
keybindLabel.Size = UDim2.new(0, 200, 0, 30)
keybindLabel.Position = UDim2.new(0.5, -100, 0.9, 0) 
keybindLabel.BackgroundTransparency = 0.2
keybindLabel.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
keybindLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
keybindLabel.Font = Enum.Font.GothamBold
keybindLabel.TextSize = 16
keybindLabel.Text = "Press J to toggle Bhop"
keybindLabel.TextScaled = true

-- Function to handle instant auto-jumping
local function autoJump()
    local connection
    connection = runService.RenderStepped:Connect(function()
        if autoJumpEnabled and humanoid then
            humanoid.Jump = true
        else
            connection:Disconnect() 
        end
    end)
end

-- Function to toggle auto-jump
local function toggleAutoJump()
    autoJumpEnabled = not autoJumpEnabled
    statusLabel.Text = autoJumpEnabled and "Bhop: ON" or "Bhop: OFF"
    statusLabel.BackgroundColor3 = autoJumpEnabled and Color3.fromRGB(50, 150, 50) or Color3.fromRGB(30, 30, 30)

    if autoJumpEnabled then
        autoJump()
    end
end

-- Allow toggling with both UI click and "J" key
statusLabel.MouseButton1Click:Connect(toggleAutoJump)
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.J then
        toggleAutoJump()
    end
end)
