-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Settings
local SpeedValue = 60 -- แนะนำเริ่มที่ 30-80 เพื่อไม่ให้โดนแบนด์
local IsActive = true
local Connection
local NoClipConnection

-- Create Mobile UI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AntiTP_SpeedUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 160, 0, 90)
MainFrame.Position = UDim2.new(0.05, 0, 0.4, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BackgroundTransparency = 0.15
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

-- Toggle Button
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0.9, 0, 0.45, 0)
ToggleBtn.Position = UDim2.new(0.05, 0, 0.08, 0)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 80)
ToggleBtn.Text = "วิ่งเร็ว (กัน TP): ON"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.TextSize = 13
ToggleBtn.Font = Enum.Font.SourceSansBold
ToggleBtn.Parent = MainFrame

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 6)
BtnCorner.Parent = ToggleBtn

-- Speed Input Box
local SpeedBox = Instance.new("TextBox")
SpeedBox.Size = UDim2.new(0.9, 0, 0.35, 0)
SpeedBox.Position = UDim2.new(0.05, 0, 0.58, 0)
SpeedBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
SpeedBox.Text = tostring(SpeedValue)
SpeedBox.PlaceholderText = "ปรับความเร็ว (10-200)"
SpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedBox.TextSize = 12
SpeedBox.Font = Enum.Font.SourceSans
SpeedBox.Parent = MainFrame

local BoxCorner = Instance.new("UICorner")
BoxCorner.CornerRadius = UDim.new(0, 6)
BoxCorner.Parent = SpeedBox

-- Linear Velocity Setup (Bypass Anti-Cheat)
local function SetupSpeed()
    if Connection then Connection:Disconnect() end
    if NoClipConnection then NoClipConnection:Disconnect() end
    
    -- ลบแรงเดิมถ้ามีอยู่
    local Char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local Root = Char:WaitForChild("HumanoidRootPart")
    if Root:FindFirstChild("AntiTPSpeed") then
        Root.AntiTPSpeed:Destroy()
    end
    if Root:FindFirstChild("SpeedAttachment") then
        Root.SpeedAttachment:Destroy()
    end

    local Attachment = Instance.new("Attachment")
    Attachment.Name = "SpeedAttachment"
    Attachment.Parent = Root

    local LV = Instance.new("LinearVelocity")
    LV.Name = "AntiTPSpeed"
    LV.MaxForce = 999999
    LV.VelocityConstraintMode = Enum.VelocityConstraintMode.Vector
    LV.RelativeTo = Enum.ActuatorRelativeTo.World
    LV.Attachment0 = Attachment
    LV.VectorVelocity = Vector3.new(0, 0, 0)
    LV.Parent = Root

    -- Loop ปรับทิศทางตามการเดิน
    Connection = RunService.Stepped:Connect(function()
        if IsActive and Char and Char:FindFirstChild("Humanoid") then
            local Hum = Char.Humanoid
            if Hum.MoveDirection.Magnitude > 0 then
                LV.VectorVelocity = Hum.MoveDirection * SpeedValue
            else
                LV.VectorVelocity = Vector3.new(0, 0, 0)
            end
        else
            LV.VectorVelocity = Vector3.new(0, 0, 0)
        end
    end)

    -- NoClip เพื่อลดแรงต้านการชนวัตถุที่ทำให้ติดเทเลพอร์ต
    NoClipConnection = RunService.Stepped:Connect(function()
        if IsActive and Char then
            for _, part in pairs(Char:GetDescendants()) do
                if part:IsA("BasePart") and part.CanCollide then
                    part.CanCollide = false
                end
            end
        end
    end)
end

-- Handlers
ToggleBtn.MouseButton1Click:Connect(function()
    IsActive = not IsActive
    if IsActive then
        ToggleBtn.Text = "วิ่งเร็ว (กัน TP): ON"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 80)
    else
        ToggleBtn.Text = "วิ่งเร็ว (กัน TP): OFF"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
    end
end)

SpeedBox.FocusLost:Connect(function()
    local Num = tonumber(SpeedBox.Text)
    if Num then
        SpeedValue = math.clamp(Num, 1, 800)
        SpeedBox.Text = tostring(SpeedValue)
    else
        SpeedBox.Text = tostring(SpeedValue)
    end
end)

-- Execute
SetupSpeed()
LocalPlayer.CharacterAdded:Connect(SetupSpeed)
