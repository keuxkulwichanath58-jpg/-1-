--// ==========================================
--// ROBLOX HITBOX EXPANDER SCRIPT (CUSTOM SIZE & TOGGLE)
--// FOR BATTLEGROUNDS & HOOD GAMES
--// ==========================================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local LocalPlayer = Players.LocalPlayer
--// 1. ป้องกันการรันซ้ำซ้อนและลบ UI เก่า
if CoreGui:FindFirstChild("HitboxExpanderUI") then
CoreGui.HitboxExpanderUI:Destroy()
end
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HitboxExpanderUI"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false
--// --- [ปุ่มเปิด-ปิดเมนูหลัก] ---
local ToggleButton = Instance.new("TextButton")
ToggleButton.Name = "ToggleMenuBtn"
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
ToggleButton.Position = UDim2.new(0, 30, 0, 100)
ToggleButton.Size = UDim2.new(0, 90, 0, 45)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.Text = "HITBOX"
ToggleButton.TextColor3 = Color3.fromRGB(0, 255, 200)
ToggleButton.TextSize = 14
local UICornerBtn = Instance.new("UICorner")
UICornerBtn.CornerRadius = UDim.new(0, 10)
UICornerBtn.Parent = ToggleButton
local UIStrokeBtn = Instance.new("UIStroke")
UIStrokeBtn.Color = Color3.fromRGB(0, 255, 200)
UIStrokeBtn.Thickness = 2
UIStrokeBtn.Parent = ToggleButton
--// --- [หน้าต่างเมนูหลัก Main Frame] ---
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 26)
MainFrame.Position = UDim2.new(0.5, -140, 0.5, -130)
MainFrame.Size = UDim2.new(0, 280, 0, 240)
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
-- หัวข้อเมนู
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Parent = MainFrame
TitleLabel.BackgroundColor3 = Color3.fromRGB(24, 24, 34)
TitleLabel.Size = UDim2.new(1, 0, 0, 45)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "📦 HITBOX EXPANDER 📦"
TitleLabel.TextColor3 = Color3.fromRGB(0, 255, 200)
TitleLabel.TextSize = 13
local UICornerTitle = Instance.new("UICorner")
UICornerTitle.CornerRadius = UDim.new(0, 14)
UICornerTitle.Parent = TitleLabel
ToggleButton.MouseButton1Click:Connect(function()
MainFrame.Visible = not MainFrame.Visible
end)
--// --- [ตัวแปรควบคุมระบบ Hitbox] ---
local hitboxEnabled = false
local hitboxSize = 15 -- ขนาดเริ่มต้น (สามารถปรับเพิ่มลดได้)
local transparencyVal = 0.6 -- ความโปร่งใสของกล่อง Hitbox (สีฟ้าโปร่งแสงแบบในภาพ)
-- ปุ่มเปิด/ปิดระบบ Hitbox
local ToggleHitboxBtn = Instance.new("TextButton")
ToggleHitboxBtn.Parent = MainFrame
ToggleHitboxBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
ToggleHitboxBtn.Position = UDim2.new(0, 15, 0, 60)
ToggleHitboxBtn.Size = UDim2.new(0, 250, 0, 38)
ToggleHitboxBtn.Font = Enum.Font.GothamMedium
ToggleHitboxBtn.Text = "ขยาย Hitbox: OFF"
ToggleHitboxBtn.TextColor3 = Color3.fromRGB(255, 80, 80)
ToggleHitboxBtn.TextSize = 12
local UICornerTgl = Instance.new("UICorner")
UICornerTgl.CornerRadius = UDim.new(0, 8)
UICornerTgl.Parent = ToggleHitboxBtn
ToggleHitboxBtn.MouseButton1Click:Connect(function()
hitboxEnabled = not hitboxEnabled
if hitboxEnabled then
ToggleHitboxBtn.Text = "ขยาย Hitbox: ON"
ToggleHitboxBtn.TextColor3 = Color3.fromRGB(0, 255, 200)
else
ToggleHitboxBtn.Text = "ขยาย Hitbox: OFF"
ToggleHitboxBtn.TextColor3 = Color3.fromRGB(255, 80, 80)
-- คืนค่า Hitbox กลับเป็นปกติเมื่อปิด
for _, p in pairs(Players:GetPlayers()) do
if p ~= LocalPlayer and p.Character then
local hrp = p.Character:FindFirstChild("HumanoidRootPart")
if hrp then
hrp.Size = Vector3.new(2, 2, 1)
hrp.Transparency = 1
hrp.CanCollide = false
end
end
end
end
end)
-- ป้ายแสดงขนาด Hitbox ปัจจุบัน
local SizeLabel = Instance.new("TextLabel")
SizeLabel.Parent = MainFrame
SizeLabel.BackgroundColor3 = Color3.fromRGB(24, 24, 34)
SizeLabel.Position = UDim2.new(0, 15, 0, 110)
SizeLabel.Size = UDim2.new(0, 250, 0, 28)
SizeLabel.Font = Enum.Font.Gotham
SizeLabel.Text = "ขนาด Hitbox: " .. hitboxSize
SizeLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
SizeLabel.TextSize = 12
local UICornerSize = Instance.new("UICorner")
UICornerSize.CornerRadius = UDim.new(0, 6)
UICornerSize.Parent = SizeLabel
-- ปุ่มเพิ่ม/ลดขนาด (-5 และ +5)
local decBtn = Instance.new("TextButton")
decBtn.Parent = MainFrame
decBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 48)
decBtn.Position = UDim2.new(0, 15, 0, 150)
decBtn.Size = UDim2.new(0, 120, 0, 35)
decBtn.Font = Enum.Font.GothamBold
decBtn.Text = "- ลดขนาด"
decBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
decBtn.TextSize = 12
local UICornerDec = Instance.new("UICorner")
UICornerDec.CornerRadius = UDim.new(0, 8)
UICornerDec.Parent = decBtn
local incBtn = Instance.new("TextButton")
incBtn.Parent = MainFrame
incBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 48)
incBtn.Position = UDim2.new(0, 145, 0, 150)
incBtn.Size = UDim2.new(0, 120, 0, 35)
incBtn.Font = Enum.Font.GothamBold
incBtn.Text = "+ เพิ่มขนาด"
incBtn.TextColor3 = Color3.fromRGB(0, 255, 200)
incBtn.TextSize = 12
local UICornerInc = Instance.new("UICorner")
UICornerInc.CornerRadius = UDim.new(0, 8)
UICornerInc.Parent = incBtn
decBtn.MouseButton1Click:Connect(import or function()
hitboxSize = math.clamp(hitboxSize - 5, 2, 50)
SizeLabel.Text = "ขนาด Hitbox: " .. hitboxSize
end)
incBtn.MouseButton1Click:Connect(function()
hitboxSize = math.clamp(hitboxSize + 5, 2, 50)
SizeLabel.Text = "ขนาด Hitbox: " .. hitboxSize
end)
--// --- [ลูปอัปเดตขยาย Hitbox ตลอดเวลา] ---
RunService.RenderStepped:Connect(function()
if hitboxEnabled then
for _, player in pairs(Players:GetPlayers()) do
if player ~= LocalPlayer and player.Character then
local hrp = player.Character:FindFirstChild("HumanoidRootPart")
local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
if hrp and humanoid and humanoid.Health > 0 then
hrp.Size = Vector3.new(hitboxSize, hitboxSize, hitboxSize)
hrp.Transparency = transparencyVal -- ทำให้มองเห็นกล่องสีฟ้าโปร่งแสงแบบในคลิป
hrp.Color = Color3.fromRGB(0, 120, 255)
hrp.CanCollide = false
end
end
end
end
end)
print("Hitbox Expander Script Loaded Successfully!")
