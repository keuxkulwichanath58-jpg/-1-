local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- ลบ UI เก่าทิ้งถ้ามีอยู่
if LocalPlayer.PlayerGui:FindFirstChild("SpeedGui") then
    LocalPlayer.PlayerGui.SpeedGui:Destroy()
end

-- สร้าง UI ใหม่
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SpeedGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Position = UDim2.new(0.05, 0, 0.3, 0)
Frame.Size = UDim2.new(0, 160, 0, 80)
Frame.Active = true
Frame.Draggable = true

local TextLabel = Instance.new("TextLabel")
TextLabel.Parent = Frame
TextLabel.Size = UDim2.new(1, 0, 0.4, 0)
TextLabel.Text = "Speed (1-500)"
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

local currentSpeed = nil
local loopConnection = nil

-- ฟังก์ชันบังคับความเร็วทุกเฟรม
local function startSpeedLoop()
    if loopConnection then loopConnection:Disconnect() end
    
    loopConnection = RunService.RenderStepped:Connect(function()
        if currentSpeed and LocalPlayer.Character then
            local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid and humanoid.WalkSpeed ~= currentSpeed then
                humanoid.WalkSpeed = currentSpeed
            end
        end
    end)
end

TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local val = tonumber(TextBox.Text)
        if val then
            currentSpeed = math.clamp(val, 1, 500)
            TextBox.Text = tostring(currentSpeed)
            startSpeedLoop()
        end
    end
end)

-- ต่อลูปทำงานทันทีเมื่อตัวละครตายแล้วเกิดใหม่
LocalPlayer.CharacterAdded:Connect(function()
    task.wait(0.5)
    if currentSpeed then
        startSpeedLoop()
    end
end)
