-- สร้าง GUI ปรับความเร็ว
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local TextBox = Instance.new("TextBox")
local TextLabel = Instance.new("TextLabel")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Frame.Position = UDim2.new(0.05, 0, 0.4, 0)
Frame.Size = UDim2.new(0, 160, 0, 80)
Frame.Active = true
Frame.Draggable = true -- สามารถลาก GUI ไปมาได้

TextLabel.Parent = Frame
TextLabel.Size = UDim2.new(1, 0, 0.4, 0)
TextLabel.Text = "Speed (1-500)"
TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1

TextBox.Parent = Frame
TextBox.Position = UDim2.new(0.1, 0, 0.45, 0)
TextBox.Size = UDim2.new(0.8, 0, 0.4, 0)
TextBox.PlaceholderText = "ใส่ตัวเลข..."
TextBox.Text = ""
TextBox.TextColor3 = Color3.fromRGB(0, 0, 0)
TextBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

-- ฟังก์ชันเปลี่ยนความเร็ว
local function updateSpeed()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    
    local speedValue = tonumber(TextBox.Text)
    if speedValue and humanoid then
        -- กำหนดให้อยู่ในช่วง 1 - 500
        speedValue = math.clamp(speedValue, 1, 500)
        humanoid.WalkSpeed = speedValue
        TextBox.Text = tostring(speedValue)
    end
end

TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        updateSpeed()
    end
end)
