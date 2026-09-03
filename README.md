local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- ลบ UI เก่า
if LocalPlayer.PlayerGui:FindFirstChild("SafeSpeedGui") then
    LocalPlayer.PlayerGui.SafeSpeedGui:Destroy()
end

-- สร้าง UI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SafeSpeedGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Frame.Position = UDim2.new(0.05, 0, 0.3, 0)
Frame.Size = UDim2.new(0, 160, 0, 80)
Frame.Active = true
Frame.Draggable = true

local TextLabel = Instance.new("TextLabel")
TextLabel.Parent = Frame
TextLabel.Size = UDim2.new(1, 0, 0.4, 0)
TextLabel.Text = "Safe Speed (1-500)"
TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1

local TextBox = Instance.new("TextBox")
TextBox.Parent = Frame
TextBox.Position = UDim2.new(0.1, 0, 0.45, 0)
TextBox.Size = UDim2.new(0.8, 0, 0.4, 0)
TextBox.PlaceholderText = "ใส่เลข 1-500"
TextBox.Text = ""
TextBox.TextColor3 = Color3.fromRGB(0, 0, 0)
TextBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

local multiplier = 1

-- ระบบคูณความเร็วการเดินโดยไม่แก้ WalkSpeed
local oldMove = nil
oldMove = hookmetamethod(game, "__namecall", function(self, ...)
    local method = getnamecallmethod()
    if method == "Move" and self:IsA("Humanoid") and multiplier > 1 then
        local args = {...}
        if args[1] then
            args[1] = args[1] * multiplier
            return oldMove(self, unpack(args))
        end
    end
    return oldMove(self, ...)
end)

TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local val = tonumber(TextBox.Text)
        if val then
            val = math.clamp(val, 1, 500)
            -- คำนวณอัตราคูณจากความเร็วปกติ (16)
            multiplier = val / 16
            TextBox.Text = tostring(val)
        end
    end
end)
