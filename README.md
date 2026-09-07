-- STREAMING_CHUNK:Initializing Roblox Universal Script GUI...
-- Ultimate Roblox Script with 5 Core Features + Master Toggle Switch
-- Compatible with Synapse Z, Kiriot, Delta, Arceus X, CodeX, and Hydrogen executors.
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local LocalPlayer = Players.LocalPlayer
-- Prevent duplicate GUI execution
if CoreGui:FindFirstChild("RobloxScriptPanel") then
CoreGui.RobloxScriptPanel:Destroy()
end
-- STREAMING_CHUNK:Creating Main GUI ScreenGui and Theme...
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "RobloxScriptPanel"
ScreenGui.Parent = CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 23, 42) -- Slate 950
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.5, -225, 0.5, -200)
MainFrame.Size = UDim2.new(0, 450, 0, 410)
MainFrame.Active = true
MainFrame.Draggable = true
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = MainFrame
local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(51, 65, 85) -- Slate 700
UIStroke.Thickness = 1.5
UIStroke.Parent = MainFrame
-- STREAMING_CHUNK:Creating Title Bar and Master Switch (Feature 6)...
local TitleBar = Instance.new("Frame")
TitleBar.Parent = MainFrame
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 41, 59) -- Slate 800
TitleBar.Size = UDim2.new(1, 0, 0, 50)
local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 12)
TitleCorner.Parent = TitleBar
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Parent = TitleBar
TitleLabel.BackgroundTransparency = 1
TitleLabel.Position = UDim2.new(0, 15, 0, 0)
TitleLabel.Size = UDim2.new(0, 250, 1, 0)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "ROBLOX SCRIPT v4.0"
TitleLabel.TextColor3 = Color3.fromRGB(248, 250, 252)
TitleLabel.TextSize = 14
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
-- 6. Master Toggle Button (เปิด/ปิดหลัก)
local MasterButton = Instance.new("TextButton")
MasterButton.Parent = TitleBar
MasterButton.BackgroundColor3 = Color3.fromRGB(16, 185, 129) -- Emerald 500 (ON)
MasterButton.Position = UDim2.new(1, -95, 0.5, -15)
MasterButton.Size = UDim2.new(0, 80, 0, 30)
MasterButton.Font = Enum.Font.GothamBold
MasterButton.Text = "ACTIVE"
MasterButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MasterButton.TextSize = 12
local MasterCorner = Instance.new("UICorner")
MasterCorner.CornerRadius = UDim.new(0, 6)
MasterCorner.Parent = MasterButton
-- STREAMING_CHUNK:Creating Content Scroll Container...
local Container = Instance.new("ScrollingFrame")
Container.Parent = MainFrame
Container.BackgroundTransparency = 1
Container.Position = UDim2.new(0, 12, 0, 60)
Container.Size = UDim2.new(1, -24, 1, -70)
Container.CanvasSize = UDim2.new(0, 0, 0, 520)
Container.ScrollBarThickness = 4
local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = Container
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)
-- State Variables for 5 Features
local scriptEnabled = true
local hitboxSize = 2.5
local espActive = true
local noclipActive = false
local infiniteJumpActive = false
local selectedTargetPlayer = nil
-- STREAMING_CHUNK:Building Feature 1 (Hitbox Expander & Head Multiplier)...
local F1Frame = Instance.new("Frame")
F1Frame.Parent = Container
F1Frame.BackgroundColor3 = Color3.fromRGB(30, 41, 59)
F1Frame.Size = UDim2.new(1, 0, 0, 85)
local F1Corner = Instance.new("UICorner")
F1Corner.CornerRadius = UDim.new(0, 8)
F1Corner.Parent = F1Frame
local F1Label = Instance.new("TextLabel")
F1Label.Parent = F1Frame
F1Label.BackgroundTransparency = 1
F1Label.Position = UDim2.new(0, 12, 0, 8)
F1Label.Size = UDim2.new(1, -24, 0, 20)
F1Label.Font = Enum.Font.GothamBold
F1Label.Text = "1. ฮิตบอกซ์ขยายดาเมจหัว (Hitbox Multiplier: 2.5x)"
F1Label.TextColor3 = Color3.fromRGB(226, 232, 240)
F1Label.TextSize = 12
F1Label.TextXAlignment = Enum.TextXAlignment.Left
-- Hitbox Loop Execution
RunService.RenderStepped:Connect(function()
if not scriptEnabled then return end
if hitboxSize > 1 then
for _, plr in ipairs(Players:GetPlayers()) do
if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
local head = plr.Character:FindFirstChild("Head")
if head then
head.Size = Vector3.new(hitboxSize * 2, hitboxSize * 2, hitboxSize * 2)
head.Transparency = 0.6
head.CanCollide = false
end
end
end
end
end)
-- STREAMING_CHUNK:Building Feature 2 (Persistent ESP Wallhack)...
local F2Frame = Instance.new("Frame")
F2Frame.Parent = Container
F2Frame.BackgroundColor3 = Color3.fromRGB(30, 41, 59)
F2Frame.Size = UDim2.new(1, 0, 0, 60)
local F2Corner = Instance.new("UICorner")
F2Corner.CornerRadius = UDim.new(0, 8)
F2Corner.Parent = F2Frame
local F2Label = Instance.new("TextLabel")
F2Label.Parent = F2Frame
F2Label.BackgroundTransparency = 1
F2Label.Position = UDim2.new(0, 12, 0, 0)
F2Label.Size = UDim2.new(0, 280, 1, 0)
F2Label.Font = Enum.Font.GothamBold
F2Label.Text = "2. มองทะลุกำแพง (ESP - ทำงานต่อเนื่อง)"
F2Label.TextColor3 = Color3.fromRGB(226, 232, 240)
F2Label.TextSize = 12
F2Label.TextXAlignment = Enum.TextXAlignment.Left
local F2Toggle = Instance.new("TextButton")
F2Toggle.Parent = F2Frame
F2Toggle.BackgroundColor3 = Color3.fromRGB(16, 185, 129)
F2Toggle.Position = UDim2.new(1, -75, 0.5, -15)
F2Toggle.Size = UDim2.new(0, 60, 0, 30)
F2Toggle.Font = Enum.Font.GothamBold
F2Toggle.Text = "ON"
F2Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
F2Toggle.TextSize = 12
local F2TCorner = Instance.new("UICorner")
F2TCorner.CornerRadius = UDim.new(0, 6)
F2TCorner.Parent = F2Toggle
-- Persistent ESP logic (Highlight instances added automatically)
RunService.RenderStepped:Connect(function()
if not scriptEnabled or not espActive then return end
for _, plr in ipairs(Players:GetPlayers()) do
if plr ~= LocalPlayer and plr.Character then
if not plr.Character:FindFirstChild("HighlightESP") then
local hl = Instance.new("Highlight")
hl.Name = "HighlightESP"
hl.Adornee = plr.Character
hl.FillColor = Color3.fromRGB(99, 102, 241)
hl.OutlineColor = Color3.fromRGB(255, 255, 255)
hl.Parent = plr.Character
end
end
end
end)
-- STREAMING_CHUNK:Building Feature 3 (Noclip / Walk Through Walls)...
local F3Frame = Instance.new("Frame")
F3Frame.Parent = Container
F3Frame.BackgroundColor3 = Color3.fromRGB(30, 41, 59)
F3Frame.Size = UDim2.new(1, 0, 0, 60)
local F3Corner = Instance.new("UICorner")
F3Corner.CornerRadius = UDim.new(0, 8)
F3Corner.Parent = F3Frame
local F3Label = Instance.new("TextLabel")
F3Label.Parent = F3Frame
F3Label.BackgroundTransparency = 1
F3Label.Position = UDim2.new(0, 12, 0, 0)
F3Label.Size = UDim2.new(0, 280, 1, 0)
F3Label.Font = Enum.Font.GothamBold
F3Label.Text = "3. เดินทะลุกำแพง (Noclip)"
F3Label.TextColor3 = Color3.fromRGB(226, 232, 240)
F3Label.TextSize = 12
F3Label.TextXAlignment = Enum.TextXAlignment.Left
local F3Toggle = Instance.new("TextButton")
F3Toggle.Parent = F3Frame
F3Toggle.BackgroundColor3 = Color3.fromRGB(239, 68, 68) -- Red (OFF)
F3Toggle.Position = UDim2.new(1, -75, 0.5, -15)
F3Toggle.Size = UDim2.new(0, 60, 0, 30)
F3Toggle.Font = Enum.Font.GothamBold
F3Toggle.Text = "OFF"
F3Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
F3Toggle.TextSize = 12
local F3TCorner = Instance.new("UICorner")
F3TCorner.CornerRadius = UDim.new(0, 6)
F3TCorner.Parent = F3Toggle
RunService.Stepped:Connect(function()
if not scriptEnabled or not noclipActive then return end
if LocalPlayer.Character then
for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do
if part:IsA("BasePart") then
part.CanCollide = false
end
end
end
end)
-- STREAMING_CHUNK:Building Feature 4 (Infinite Jump)...
local F4Frame = Instance.new("Frame")
F4Frame.Parent = Container
F4Frame.BackgroundColor3 = Color3.fromRGB(30, 41, 59)
F4Frame.Size = UDim2.new(1, 0, 0, 60)
local F4Corner = Instance.new("UICorner")
F4Corner.CornerRadius = UDim.new(0, 8)
F4Corner.Parent = F4Frame
local F4Label = Instance.new("TextLabel")
F4Label.Parent = F4Frame
F4Label.BackgroundTransparency = 1
F4Label.Position = UDim2.new(0, 12, 0, 0)
F4Label.Size = UDim2.new(0, 280, 1, 0)
F4Label.Font = Enum.Font.GothamBold
F4Label.Text = "4. กระโดดไม่จำกัด (Infinite Air Jump)"
F4Label.TextColor3 = Color3.fromRGB(226, 232, 240)
F4Label.TextSize = 12
F4Label.TextXAlignment = Enum.TextXAlignment.Left
local F4Toggle = Instance.new("TextButton")
F4Toggle.Parent = F4Frame
F4Toggle.BackgroundColor3 = Color3.fromRGB(16, 185, 129)
F4Toggle.Position = UDim2.new(1, -75, 0.5, -15)
F4Toggle.Size = UDim2.new(0, 60, 0, 30)
F4Toggle.Font = Enum.Font.GothamBold
F4Toggle.Text = "ON"
F4Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
F4Toggle.TextSize = 12
local F4TCorner = Instance.new("UICorner")
F4TCorner.CornerRadius = UDim.new(0, 6)
F4TCorner.Parent = F4Toggle
UserInputService.JumpRequest:Connect(function()
if scriptEnabled and infiniteJumpActive and LocalPlayer.Character then
local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if humanoid then
humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
end
end
end)
-- STREAMING_CHUNK:Building Feature 5 (Click Player to Teleport without Typing)...
local F5Frame = Instance.new("Frame")
F5Frame.Parent = Container
F5Frame.BackgroundColor3 = Color3.fromRGB(30, 41, 59)
F5Frame.Size = UDim2.new(1, 0, 0, 150)
local F5Corner = Instance.new("UICorner")
F5Corner.CornerRadius = UDim.new(0, 8)
F5Corner.Parent = F5Frame
local F5Label = Instance.new("TextLabel")
F5Label.Parent = F5Frame
F5Label.BackgroundTransparency = 1
F5Label.Position = UDim2.new(0, 12, 0, 8)
F5Label.Size = UDim2.new(1, -24, 0, 20)
F5Label.Font = Enum.Font.GothamBold
F5Label.Text = "5. เลือกผู้เล่นเพื่อวาร์ปทันที (คลิกที่ชื่อ ไม่ต้องพิมพ์)"
F5Label.TextColor3 = Color3.fromRGB(226, 232, 240)
F5Label.TextSize = 12
F5Label.TextXAlignment = Enum.TextXAlignment.Left
local PlayerScroll = Instance.new("ScrollingFrame")
PlayerScroll.Parent = F5Frame
PlayerScroll.BackgroundColor3 = Color3.fromRGB(15, 23, 42)
PlayerScroll.Position = UDim2.new(0, 12, 0, 35)
PlayerScroll.Size = UDim2.new(1, -24, 1, -45)
PlayerScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
PlayerScroll.ScrollBarThickness = 3
local PScrollCorner = Instance.new("UICorner")
PScrollCorner.CornerRadius = UDim.new(0, 6)
PScrollCorner.Parent = PlayerScroll
local PListLayout = Instance.new("UIListLayout")
PListLayout.Parent = PlayerScroll
PListLayout.SortOrder = Enum.SortOrder.LayoutOrder
PListLayout.Padding = UDim.new(0, 4)
-- Function to populate player list dynamically
local function refreshPlayerList()
for _, child in ipairs(PlayerScroll:GetChildren()) do
if child:IsA("TextButton") then
child:Destroy()
end
end
for _, plr in ipairs(Players:GetPlayers()) do
if plr ~= LocalPlayer then
local pBtn = Instance.new("TextButton")
pBtn.Parent = PlayerScroll
pBtn.BackgroundColor3 = Color3.fromRGB(51, 65, 85)
pBtn.Size = UDim2.new(1, 0, 0, 28)
pBtn.Font = Enum.Font.GothamSemibold
pBtn.Text = "  " .. plr.Name
pBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
pBtn.TextSize = 11
pBtn.TextXAlignment = Enum.TextXAlignment.Left
local bCorner = Instance.new("UICorner")
bCorner.CornerRadius = UDim.new(0, 4)
bCorner.Parent = pBtn
-- Teleport Action on click
pBtn.MouseButton1Click:Connect(function()
if scriptEnabled and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
LocalPlayer.Character.HumanoidRootPart.CFrame = plr.Character.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
end
end)
end
end
PlayerScroll.CanvasSize = UDim2.new(0, 0, 0, #Players:GetPlayers() * 32)
end
Players.PlayerAdded:Connect(refreshPlayerList)
Players.PlayerRemoving:Connect(refreshPlayerList)
refreshPlayerList()
-- STREAMING_CHUNK:Connecting Master Toggle & Feature Buttons UI Actions...
MasterButton.MouseButton1Click:Connect(function()
scriptEnabled = not scriptEnabled
if scriptEnabled then
MasterButton.Text = "ACTIVE"
MasterButton.BackgroundColor3 = Color3.fromRGB(16, 185, 129)
MainFrame.Transparency = 0
else
MasterButton.Text = "PAUSED"
MasterButton.BackgroundColor3 = Color3.fromRGB(239, 68, 68)
end
end)
F2Toggle.MouseButton1Click:Connect(function()
espActive = not espActive
F2Toggle.Text = espActive and "ON" or "OFF"
F2Toggle.BackgroundColor3 = espActive and Color3.fromRGB(16, 185, 129) or Color3.fromRGB(239, 68, 68)
end)
F3Toggle.MouseButton1Click:Connect(function()
noclipActive = not noclipActive
F3Toggle.Text = noclipActive and "ON" or "OFF"
F3Toggle.BackgroundColor3 = noclipActive and Color3.fromRGB(16, 185, 129) or Color3.fromRGB(239, 68, 68)
end)
F4Toggle.MouseButton1Click:Connect(function()
infiniteJumpActive = not infiniteJumpActive
F4Toggle.Text = infiniteJumpActive and "ON" or "OFF"
F4Toggle.BackgroundColor3 = infiniteJumpActive and Color3.fromRGB(16, 185, 129) or Color3.fromRGB(239, 68, 68)
end)
print("Roblox Universal Script loaded successfully!")
