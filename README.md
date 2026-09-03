-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Settings
local SpeedValue = 50 -- ปรับความเร็ว CFrame (แนะนำตั้ง 10-100 ก่อน เพราะพุ่งไวมาก)
local IsActive = true
local Connection

-- Create Mobile UI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SpeedMobileUI_V2"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 160, 0, 90)
MainFrame.Position = UDim2.new(0.05, 0, 0.4, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BackgroundTransparency = 0.2
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
ToggleBtn.Text = "วิ่งเร็ว: ON"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.TextSize = 14
ToggleBtn.Font = Enum.Font.SourceSansBold
ToggleBtn.Parent = MainFrame

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 6)
BtnCorner.Parent = ToggleBtn

-- Speed Input Box
local SpeedBox = Instance.new("TextBox")
SpeedBox.Size = UDim2.new(0.9, 0, 0.35, 0)
SpeedBox.Position = UDim2.new(0.05, 0, 0.58, 0)
SpeedBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SpeedBox.Text = tostring(SpeedValue)
SpeedBox.PlaceholderText = "ใส่ความเร็ว (1-800)"
SpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedBox.TextSize = 12
SpeedBox.Font = Enum.Font.SourceSans
SpeedBox.Parent = MainFrame

local BoxCorner = Instance.new("UICorner")
BoxCorner.CornerRadius = UDim.new(0, 6)
BoxCorner.Parent = SpeedBox

-- Core Speed Logic (CFrame Position Teleporting Method)
local function EnableFastRun()
    if Connection then Connection:Disconnect() end
    
    Connection = RunService.RenderStepped:Connect(function(deltaTime)
        if IsActive and LocalPlayer.Character then
            local Humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            
            if Humanoid and Humanoid.MoveDirection.Magnitude > 0 then
                -- ย้ายตำแหน่งตัวละครไปข้างหน้าตามทิศทางจอยสติ๊กสัมผัส
                local MoveDir = Humanoid.MoveDirection
                local Offset = MoveDir * (SpeedValue * deltaTime * 5)
                LocalPlayer.Character:PivotTo(LocalPlayer.Character:GetPivot() + Offset)
            end
        end
    end)
end

-- Handlers
ToggleBtn.MouseButton1Click:Connect(function()
    IsActive = not IsActive
    if IsActive then
        ToggleBtn.Text = "วิ่งเร็ว: ON"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 80)
    else
        ToggleBtn.Text = "วิ่งเร็ว: OFF"
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
EnableFastRun()
LocalPlayer.CharacterAdded:Connect(EnableFastRun)
