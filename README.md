-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Settings
local StepSpeed = 20 -- ปรับระดับความเร็ว (แนะนำ 10-40 จะไม่ติดเทเลพอร์ต)
local IsActive = true

-- UI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BypassBlinkSpeedUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 160, 0, 90)
MainFrame.Position = UDim2.new(0.05, 0, 0.4, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BackgroundTransparency = 0.2
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0.9, 0, 0.45, 0)
ToggleBtn.Position = UDim2.new(0.05, 0, 0.08, 0)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 80)
ToggleBtn.Text = "วิ่งเร็ว (Bypass): ON"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.TextSize = 12
ToggleBtn.Font = Enum.Font.SourceSansBold
ToggleBtn.Parent = MainFrame

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 5)
BtnCorner.Parent = ToggleBtn

local SpeedBox = Instance.new("TextBox")
SpeedBox.Size = UDim2.new(0.9, 0, 0.35, 0)
SpeedBox.Position = UDim2.new(0.05, 0, 0.58, 0)
SpeedBox.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
SpeedBox.Text = tostring(StepSpeed)
SpeedBox.PlaceholderText = "ปรับระดับ (10-50)"
SpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedBox.TextSize = 12
SpeedBox.Font = Enum.Font.SourceSans
SpeedBox.Parent = MainFrame

local BoxCorner = Instance.new("UICorner")
BoxCorner.CornerRadius = UDim.new(0, 5)
BoxCorner.Parent = SpeedBox

-- Core Loop Bypass (Pulse Teleportation)
task.spawn(function()
    while true do
        task.wait(0.03) -- เว้นจังหวะส่งค่าให้เซิร์ฟเวอร์ยอมรับ
        if IsActive and LocalPlayer.Character then
            local Hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            local Root = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            
            if Hum and Root and Hum.MoveDirection.Magnitude > 0 then
                -- ขยับระยะทางสั้นๆ ถี่ๆ หลบระบบตรวจจับ
                local Distance = Hum.MoveDirection * (StepSpeed / 10)
                Root.CFrame = Root.CFrame + Distance
            end
        end
    end
end)

-- Handlers
ToggleBtn.MouseButton1Click:Connect(function()
    IsActive = not IsActive
    if IsActive then
        ToggleBtn.Text = "วิ่งเร็ว (Bypass): ON"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 80)
    else
        ToggleBtn.Text = "วิ่งเร็ว (Bypass): OFF"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
    end
end)

SpeedBox.FocusLost:Connect(function()
    local Num = tonumber(SpeedBox.Text)
    if Num then
        StepSpeed = math.clamp(Num, 1, 800)
        SpeedBox.Text = tostring(StepSpeed)
    else
        SpeedBox.Text = tostring(StepSpeed)
    end
end)
