--// Universal Fix Script UI (PlayerGui Version)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

--// ลบ UI เก่าทิ้งเพื่อป้องกันการซ้ำซ้อน
if PlayerGui:FindFirstChild("FixedScriptUI") then
    PlayerGui.FixedScriptUI:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FixedScriptUI"
ScreenGui.Parent = PlayerGui
ScreenGui.ResetOnSpawn = false

--// ปุ่มเปิด-ปิดเมนู
local ToggleButton = Instance.new("TextButton")
ToggleButton.Name = "ToggleBtn"
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
ToggleButton.Position = UDim2.new(0, 30, 0, 100)
ToggleButton.Size = UDim2.new(0, 60, 0, 60)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.Text = "MENU"
ToggleButton.TextColor3 = Color3.fromRGB(0, 0, 0)
ToggleButton.TextSize = 13

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(1, 0)
btnCorner.Parent = ToggleButton

--// หน้าต่างหลัก
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.Position = UDim2.new(0.5, -130, 0.5, -130)
MainFrame.Size = UDim2.new(0, 260, 0, 220)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true

local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0, 10)
frameCorner.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Font = Enum.Font.GothamBold
Title.Text = "⚡ SAFE SCRIPT MENU ⚡"
Title.TextColor3 = Color3.fromRGB(0, 255, 150)
Title.TextSize = 14

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 10)
titleCorner.Parent = Title

ToggleButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

--// ฟังก์ชันวิ่งเร็วอัตโนมัติ (WalkSpeed = 30)
RunService.RenderStepped:Connect(function()
    if LocalPlayer.Character then
        local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = 30
        end
    end
end)

print("Fixed Universal Script Loaded Successfully via PlayerGui!")
