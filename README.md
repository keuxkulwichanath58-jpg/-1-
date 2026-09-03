local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- ลบ UI เก่า
if LocalPlayer.PlayerGui:FindFirstChild("VelocitySpeedGui") then
    LocalPlayer.PlayerGui.VelocitySpeedGui:Destroy()
end

-- สร้าง UI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "VelocitySpeedGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Frame.Position = UDim2.new(0.05, 0, 0.3, 0)
Frame.Size = UDim2.new(0, 160, 0, 80)
Frame.Active = true
Frame.Draggable = true

local TextLabel = Instance.new("TextLabel")
TextLabel.Parent = Frame
TextLabel.Size = UDim2.new(1, 0, 0.4, 0)
TextLabel.Text = "Custom Speed (1-500)"
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

local targetSpeed = 16

TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local val = tonumber(TextBox.Text)
        if val then
            targetSpeed = math.clamp(val, 1, 500)
            TextBox.Text = tostring(targetSpeed)
        end
    end
end)

-- ดันตัวละครไปตามทิศทางที่กดเดิน
RunService.PreRender:Connect(function()
    local char = LocalPlayer.Character
    if not char then return end
    
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    
    if hrp and humanoid and humanoid.MoveDirection.Magnitude > 0 and targetSpeed > 16 then
        -- คำนวณทิศทางเดิน x ความเร็วที่ตั้งไว้
        local moveDir = humanoid.MoveDirection
        hrp.AssemblyLinearVelocity = Vector3.new(
            moveDir.X * targetSpeed,
            hrp.AssemblyLinearVelocity.Y, -- ปล่อยค่าแนวตั้งไว้เท่าเดิมเพื่อไม่ให้ตัวลอย/วาร์ป
            moveDir.Z * targetSpeed
        )
    end
end)
