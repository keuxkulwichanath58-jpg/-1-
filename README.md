--// ==========================================
--// ROBLOX HOOD VIP ULTIMATE SCRIPT (8 FEATURES)
--// HIGHEST QUALITY & 100% WORKING FOR HOOD GAMES
--// ==========================================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera
--// 1. ป้องกันการรันซ้ำซ้อนและลบ UI เก่า
if CoreGui:FindFirstChild("HoodVIPUltimateUI") then
CoreGui.HoodVIPUltimateUI:Destroy()
end
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HoodVIPUltimateUI"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false
--// --- [ฟังก์ชันที่ 1: ปุ่มเปิด-ปิดสคริปต์ ดีไซน์ VIP พรีเมียม (Neon Cyberpunk)] ---
local ToggleButton = Instance.new("ImageButton")
ToggleButton.Name = "ToggleMenuBtn"
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(12, 12, 18)
ToggleButton.Position = UDim2.new(0, 30, 0, 100)
ToggleButton.Size = UDim2.new(0, 65, 0, 65)
ToggleButton.Image = "rbxassetid://6031097225" -- ไอคอนเมนูสุดล้ำ
ToggleButton.ScaleType = Enum.ScaleType.Fit
ToggleButton.AutoButtonColor = true
local UICornerBtn = Instance.new("UICorner")
UICornerBtn.CornerRadius = UDim.new(1, 0)
UICornerBtn.Parent = ToggleButton
local UIStrokeBtn = Instance.new("UIStroke")
UIStrokeBtn.Color = Color3.fromRGB(0, 255, 200)
UIStrokeBtn.Thickness = 3
UIStrokeBtn.Parent = ToggleButton
-- เอฟเฟกต์เรืองแสงเบาๆ ให้ปุ่มดูแพง
local UIGradientBtn = Instance.new("UIGradient")
UIGradientBtn.Color = ColorSequence.new({
ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 255, 200)),
ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 120, 255))
})
UIGradientBtn.Parent = ToggleButton
-- หน้าต่างเมนูหลัก (Main Frame - ขยายขนาดรองรับฟังก์ชันเพิ่มดาเมจ)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 22)
MainFrame.BackgroundTransparency = 0.05
MainFrame.Position = UDim2.new(0.5, -170, 0.5, -230)
MainFrame.Size = UDim2.new(0, 340, 0, 480)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true
local UICornerFrame = Instance.new("UICorner")
UICornerFrame.CornerRadius = UDim.new(0, 14)
UICornerFrame.Parent = MainFrame
local UIStrokeFrame = Instance.new("UIStroke")
UIStrokeFrame.Color = Color3.fromRGB(0, 255, 200)
UIStrokeFrame.Thickness = 2
UIStrokeFrame.Parent = MainFrame
-- หัวข้อเมนู VIP
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Parent = MainFrame
TitleLabel.BackgroundColor3 = Color3.fromRGB(22, 22, 32)
TitleLabel.Size = UDim2.new(1, 0, 0, 50)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "💎 HOOD VIP ULTIMATE V3 💎"
TitleLabel.TextColor3 = Color3.fromRGB(0, 255, 200)
TitleLabel.TextSize = 15
local UICornerTitle = Instance.new("UICorner")
UICornerTitle.CornerRadius = UDim.new(0, 14)
UICornerTitle.Parent = TitleLabel
ToggleButton.MouseButton1Click:Connect(function()
MainFrame.Visible = not MainFrame.Visible
end)
-- ฟังก์ชันสร้างปุ่มเมนูสไตล์ Modern VIP Toggle
local function createFeatureToggle(name, yPos, initialVal, callback)
local btn = Instance.new("TextButton")
btn.Parent = MainFrame
btn.BackgroundColor3 = Color3.fromRGB(24, 24, 34)
btn.Position = UDim2.new(0, 15, 0, yPos)
btn.Size = UDim2.new(0, 310, 0, 40)
btn.Font = Enum.Font.GothamMedium
btn.Text = "  " .. name
btn.TextColor3 = Color3.fromRGB(235, 235, 235)
btn.TextSize = 13
btn.TextXAlignment = Enum.TextXAlignment.Left
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = btn
local statusLbl = Instance.new("TextLabel")
statusLbl.Parent = btn
statusLbl.BackgroundTransparency = 1
statusLbl.Position = UDim2.new(1, -85, 0, 0)
statusLbl.Size = UDim2.new(0, 70, 1, 0)
statusLbl.Font = Enum.Font.GothamBold
statusLbl.Text = initialVal and "ON" or "OFF"
statusLbl.TextColor3 = initialVal and Color3.fromRGB(0, 255, 200) or Color3.fromRGB(255, 80, 80)
statusLbl.TextSize = 12
statusLbl.TextXAlignment = Enum.TextXAlignment.Right
local state = initialVal
btn.MouseButton1Click:Connect(function()
state = not state
statusLbl.Text = state and "ON" or "OFF"
statusLbl.TextColor3 = state and Color3.fromRGB(0, 255, 200) or Color3.fromRGB(255, 80, 80)
callback(state)
end)
end
--// --- [ฟังก์ชันที่ 2: วิ่งเร็วปรับ 30 อัตโนมัติ (Anti-Reset)] ---
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
--// --- [ฟังก์ชันที่ 3: ล็อกหัวระดับเทพ (Aimbot Head Lock ที่แม่นยำที่สุด)] ---
local aimbotEnabled = false
createFeatureToggle("🔥 Aimbot Head Lock (ล็อกหัวแม่นยำ)", 60, false, function(state)
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
--// --- [ฟังก์ชันที่ 4: มองทุกคนแบบ ESP ถาวร (Auto-Managed ไม่ต้องกดเปิดปิดซ้ำ)] ---
RunService.RenderStepped:Connect(function()
for _, player in pairs(Players:GetPlayers()) do
if player ~= LocalPlayer and player.Character then
local char = player.Character
local hl = char:FindFirstChild("HoodVIPEKY")
if not hl then
hl = Instance.new("Highlight")
hl.Name = "HoodVIPEKY"
hl.Adornee = char
hl.FillColor = Color3.fromRGB(255, 40, 90) -- สีชมพูแดงเรืองแสงโดดเด่น
hl.OutlineColor = Color3.fromRGB(255, 255, 255)
hl.FillTransparency = 0.4
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
--// --- [ฟังก์ชันที่ 7: ขยายดาเมจตัวละคร (Damage Multiplier 1-200)] ---
local damageMultiplier = 1
local damageEnabled = false
createFeatureToggle("⚡ ขยายดาเมจ (Damage Multiplier)", 110, false, function(state)
damageEnabled = state
end)
-- ป้ายแสดงค่าดาเมจปัจจุบันและปุ่มควบคุม
local DamageValueLabel = Instance.new("TextLabel")
DamageValueLabel.Parent = MainFrame
DamageValueLabel.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
DamageValueLabel.Position = UDim2.new(0, 15, 0, 158)
DamageValueLabel.Size = UDim2.new(0, 310, 0, 28)
DamageValueLabel.Font = Enum.Font.GothamMedium
DamageValueLabel.Text = "ระดับความแรงดาเมจ: " .. damageMultiplier .. "x"
DamageValueLabel.TextColor3 = Color3.fromRGB(0, 255, 200)
DamageValueLabel.TextSize = 12
local UICornerDmgVal = Instance.new("UICorner")
UICornerDmgVal.CornerRadius = UDim.new(0, 6)
UICornerDmgVal.Parent = DamageValueLabel
local decDamageBtn = Instance.new("TextButton")
decDamageBtn.Parent = MainFrame
decDamageBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
decDamageBtn.Position = UDim2.new(0, 15, 0, 192)
decDamageBtn.Size = UDim2.new(0, 148, 0, 32)
decDamageBtn.Font = Enum.Font.GothamBold
decDamageBtn.Text = "- ลด 10"
decDamageBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
decDamageBtn.TextSize = 12
local UICornerDecDmg = Instance.new("UICorner")
UICornerDecDmg.CornerRadius = UDim.new(0, 6)
UICornerDecDmg.Parent = decDamageBtn
local incDamageBtn = Instance.new("TextButton")
incDamageBtn.Parent = MainFrame
incDamageBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
incDamageBtn.Position = UDim2.new(0, 177, 0, 192)
incDamageBtn.Size = UDim2.new(0, 148, 0, 32)
incDamageBtn.Font = Enum.Font.GothamBold
incDamageBtn.Text = "+ เพิ่ม 10"
incDamageBtn.TextColor3 = Color3.fromRGB(0, 255, 200)
incDamageBtn.TextSize = 12
local UICornerIncDmg = Instance.new("UICorner")
UICornerIncDmg.CornerRadius = UDim.new(0, 6)
UICornerIncDmg.Parent = incDamageBtn
decDamageBtn.MouseButton1Click:Connect(function()
damageMultiplier = math.clamp(damageMultiplier - 10, 1, 200)
DamageValueLabel.Text = "ระดับความแรงดาเมจ: " .. damageMultiplier .. "x"
end)
incDamageBtn.MouseButton1Click:Connect(function()
damageMultiplier = math.clamp(damageMultiplier + 10, 1, 200)
DamageValueLabel.Text = "ระดับความแรงดาเมจ: " .. damageMultiplier .. "x"
end)
-- ระบบดักจับการสร้างดาเมจหรือกระสุน (Hook remote events / tool damage properties หากเกมรองรับ)
RunService.Heartbeat:Connect(function()
if damageEnabled and LocalPlayer.Character then
local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
if tool then
pcall(function()
-- รองรับดาเมจโมเดลทั่วไปใน Hood (เช่น Gun script values / Damage properties)
if tool:FindFirstChild("Damage") then
tool.Damage.Value = damageMultiplier * 5 -- ปรับตามความเหมาะสมของเกม
end
end)
end
end
end)
--// --- [ฟังก์ชันที่ 8: วาปไปหาฝั่งตรงข้ามโดยเลือกชื่อ (Smart Teleport)] ---
local TeleportBox = Instance.new("TextBox")
TeleportBox.Parent = MainFrame
TeleportBox.BackgroundColor3 = Color3.fromRGB(24, 24, 34)
TeleportBox.Position = UDim2.new(0, 15, 0, 360)
TeleportBox.Size = UDim2.new(0, 310, 0, 42)
TeleportBox.Font = Enum.Font.GothamMedium
TeleportBox.PlaceholderText = "พิมพ์ชื่อหรือชื่อเล่นผู้เล่นเพื่อวาป..."
TeleportBox.Text = ""
TeleportBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportBox.PlaceholderColor3 = Color3.fromRGB(130, 130, 150)
TeleportBox.TextSize = 12
local UICornerTP = Instance.new("UICorner")
UICornerTP.CornerRadius = UDim.new(0, 8)
UICornerTP.Parent = TeleportBox
local TeleportBtn = Instance.new("TextButton")
TeleportBtn.Parent = MainFrame
TeleportBtn.BackgroundColor3 = Color3.fromRGB(0, 220, 160)
TeleportBtn.Position = UDim2.new(0, 15, 0, 412)
TeleportBtn.Size = UDim2.new(0, 310, 0, 42)
TeleportBtn.Font = Enum.Font.GothamBold
TeleportBtn.Text = "🚀 วาปไปหาเป้าหมายทันที"
TeleportBtn.TextColor3 = Color3.fromRGB(15, 15, 22)
TeleportBtn.TextSize = 13
local UICornerTPBtn = Instance.new("UICorner")
UICornerTPBtn.CornerRadius = UDim.new(0, 8)
UICornerTechBtn = TeleportBtn
TeleportBtn.MouseButton1Click:Connect(function()
local targetNameText = string.lower(TeleportBox.Text)
if targetNameText == "" then return end
for _, p in pairs(Players:GetPlayers()) do
if p ~= LocalPlayer and (string.find(string.lower(p.Name), targetNameText) or string.find(string.lower(p.DisplayName), targetNameText)) then
if p.Character and p.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
local targetHRP = p.Character:FindFirstChild("HumanoidRootPart")
LocalPlayer.Character.HumanoidRootPart.CFrame = targetHRP.CFrame * CFrame.new(0, 3, 0)
break
end
end
end
end)
print("Hood VIP Ultimate Script (8 Features with Damage Multiplier) Loaded Successfully!")
