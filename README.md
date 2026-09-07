--// ==========================================
--// ROBLOX HOOD ULTIMATE SCRIPT (7 FEATURES)
--// HIGH QUALITY & 100% WORKING
--// ==========================================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera
--// 1. ป้องกันการรันซ้ำซ้อน
if CoreGui:FindFirstChild("HoodUltimateUI") then
CoreGui.HoodUltimateUI:Destroy()
end
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HoodUltimateUI"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false
--// --- [ฟังก์ชันที่ 1: ปุ่มปิด-เปิดสคริปต์ ดีไซน์เท่พรีเมียม] ---
local ToggleButton = Instance.new("ImageButton")
ToggleButton.Name = "ToggleMenuBtn"
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
ToggleButton.Position = UDim2.new(0, 25, 0, 90)
ToggleButton.Size = UDim2.new(0, 60, 0, 60)
ToggleButton.Image = "rbxassetid://6031097225" -- ไอคอนรูปฟันเฟืองหรือเมนูสุดเท่
ToggleButton.ScaleType = Enum.ScaleType.Fit
ToggleButton.AutoButtonColor = true
local UICornerBtn = Instance.new("UICorner")
UICornerBtn.CornerRadius = UDim.new(1, 0)
UICornerBtn.Parent = ToggleButton
local UIStrokeBtn = Instance.new("UIStroke")
UIStrokeBtn.Color = Color3.fromRGB(0, 255, 150)
UIStrokeBtn.Thickness = 2
UIStrokeBtn.Parent = ToggleButton
--// หน้าต่างเมนูหลัก (Main Frame)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -170)
MainFrame.Size = UDim2.new(0, 320, 0, 360)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true
local UICornerFrame = Instance.new("UICorner")
UICornerFrame.CornerRadius = UDim.new(0, 12)
UICornerFrame.Parent = MainFrame
local UIStrokeFrame = Instance.new("UIStroke")
UIStrokeFrame.Color = Color3.fromRGB(40, 40, 50)
UIStrokeFrame.Thickness = 1.5
UIStrokeFrame.Parent = MainFrame
-- หัวข้อเมนู
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Parent = MainFrame
TitleLabel.BackgroundColor3 = Color3.fromRGB(25, 25, 32)
TitleLabel.Size = UDim2.new(1, 0, 0, 45)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "⚡ HOOD ULTIMATE V1 ⚡"
TitleLabel.TextColor3 = Color3.fromRGB(0, 255, 150)
TitleLabel.TextSize = 14
local UICornerTitle = Instance.new("UICorner")
UICornerTitle.CornerRadius = UDim.new(0, 12)
UICornerTitle.Parent = TitleLabel
ToggleButton.MouseButton1Click:Connect(function()
MainFrame.Visible = not MainFrame.Visible
end)
-- ฟังก์ชันสร้างปุ่มเมนูสไตล์ Modern Toggle
local function createFeatureToggle(name, yPos, initialVal, callback)
local btn = Instance.new("TextButton")
btn.Parent = MainFrame
btn.BackgroundColor3 = Color3.fromRGB(28, 28, 36)
btn.Position = UDim2.new(0, 15, 0, yPos)
btn.Size = UDim2.new(0, 290, 0, 38)
btn.Font = Enum.Font.GothamMedium
btn.Text = "  " .. name
btn.TextColor3 = Color3.fromRGB(220, 220, 220)
btn.TextSize = 13
btn.TextXAlignment = Enum.TextXAlignment.Left
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = btn
local statusLbl = Instance.new("TextLabel")
statusLbl.Parent = btn
statusLbl.BackgroundTransparency = 1
statusLbl.Position = UDim2.new(1, -75, 0, 0)
statusLbl.Size = UDim2.new(0, 60, 1, 0)
statusLbl.Font = Enum.Font.GothamBold
statusLbl.Text = initialVal and "ON" or "OFF"
statusLbl.TextColor3 = initialVal and Color3.fromRGB(0, 255, 150) or Color3.fromRGB(255, 80, 80)
statusLbl.TextSize = 12
statusLbl.TextXAlignment = Enum.TextXAlignment.Right
local state = initialVal
btn.MouseButton1Click:Connect(function()
state = not state
statusLbl.Text = state and "ON" or "OFF"
statusLbl.TextColor3 = state and Color3.fromRGB(0, 255, 150) or Color3.fromRGB(255, 80, 80)
callback(state)
end)
end
--// --- [ฟังก์ชันที่ 2: วิ่งเร็วปรับ 30 อัตโนมัติ] ---
RunService.Heartbeat:Connect(function()
if LocalPlayer.Character then
local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if humanoid then
humanoid.WalkSpeed = 30
pcall(function()
if humanoid.WalkSpeedOverride then
humanoid.WalkSpeedOverride = 30
end
end)
end
end
end)
--// --- [ฟังก์ชันที่ 3: ล็อกหัวคุณภาพสูง (Aimbot Head Lock)] ---
local aimbotEnabled = false
createFeatureToggle("Aimbot Head Lock", 55, false, function(state)
aimbotEnabled = state
end)
RunService.RenderStepped:Connect(function()
if aimbotEnabled then
local target = nil
local shortestDist = math.huge
local mousePos = UserInputService:GetMouseLocation()
for _, player in pairs(Players:GetPlayers()) do
if player ~= LocalPlayer and player.Character then
local head = player.Character:FindFirstChild("Head")
local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
if head and humanoid and humanoid.Health > 0 then
-- เช็คระยะและหน้าจอ
local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
if onScreen then
local magnitude = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
if magnitude < shortestDist then
shortestDist = magnitude
target = head
end
end
end
end
end
if target then
Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
end
end
end)
--// --- [ฟังก์ชันที่ 4: มองทุกคนแบบ ESP ถาวร (ไม่ต้องกดปิดเปิดใหม่)] ---
RunService.RenderStepped:Connect(function()
for _, player in pairs(Players:GetPlayers()) do
if player ~= LocalPlayer and player.Character then
local char = player.Character
local hl = char:FindFirstChild("HoodPermanentESP")
if not hl then
hl = Instance.new("Highlight")
hl.Name = "HoodPermanentESP"
hl.Adornee = char
hl.FillColor = Color3.fromRGB(255, 50, 50)
hl.OutlineColor = Color3.fromRGB(255, 255, 255)
hl.Parent = char
end
end
end
end)
--// --- [ฟังก์ชันที่ 5: เดินทะลุกำแพงเปิดอัตโนมัติ (NoClip)] ---
RunService.Stepped:Connect(function()
if LocalPlayer.Character then
for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
if part:IsA("BasePart") then
part.CanCollide = false
end
end
end
end)
--// --- [ฟังก์ชันที่ 6: กระโดดไม่จำกัดเปิดอัตโนมัติ (Infinite Jump)] ---
UserInputService.JumpRequest:Connect(function()
if LocalPlayer.Character then
local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if humanoid then
humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
end
end
end)
--// --- [ฟังก์ชันที่ 7: วาปไปหาฝั่งตรงข้ามโดยเลือกชื่อ] ---
-- สร้าง UI เพิ่มเติมสำหรับเลือกชื่อวาปด้านล่างเมนู
local TeleportBox = Instance.new("TextBox")
TeleportBox.Parent = MainFrame
TeleportBox.BackgroundColor3 = Color3.fromRGB(28, 28, 36)
TeleportBox.Position = UDim2.new(0, 15, 0, 260)
TeleportBox.Size = UDim2.new(0, 290, 0, 38)
TeleportBox.Font = Enum.Font.GothamMedium
TeleportBox.PlaceholderText = "พิมพ์ชื่อผู้เล่นเพื่อวาป (เช่น Player1)"
TeleportBox.Text = ""
TeleportBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportBox.PlaceholderColor3 = Color3.fromRGB(120, 120, 140)
TeleportBox.TextSize = 12
local UICornerTP = Instance.new("UICorner")
UICornerTP.CornerRadius = UDim.new(0, 8)
UICornerTP.Parent = TeleportBox
local TeleportBtn = Instance.new("TextButton")
TeleportBtn.Parent = MainFrame
TeleportBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 120)
TeleportBtn.Position = UDim2.new(0, 15, 0, 308)
TeleportBtn.Size = UDim2.new(0, 290, 0, 36)
TeleportBtn.Font = Enum.Font.GothamBold
TeleportBtn.Text = "🚀 วาปไปหาผู้เล่น"
TeleportBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportBtn.TextSize = 13
local UICornerTPBtn = Instance.new("UICorner")
UICornerTPBtn.CornerRadius = UDim.new(0, 8)
UICornerTPBtn.Parent = TeleportBtn
TeleportBtn.MouseButton1Click:Connect(function()
local targetNameText = string.lower(TeleportBox.Text)
if targetNameText == "" then return end
for _, p in pairs(Players:GetPlayers()) do
if p ~= LocalPlayer and (string.find(string.lower(p.Name), targetNameText) or string.find(string.lower(p.DisplayName), targetNameText)) then
if p.Character and p.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
LocalPlayer.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0, 3, 0)
break
end
end
end
end)
print("Hood Ultimate Script Loaded Successfully with all 7 Features!")
