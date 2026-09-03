-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

-- Settings
local SpeedValue = 100 -- ปรับความเร็วตรงนี้ได้ตั้งแต่ 1 - 800
local ToggleKey = Enum.KeyCode.F -- ปุ่มเปิด-ปิดสคริปต์ (เปลี่ยนได้ตามต้องการ)
local IsActive = true

-- Variable
local Connection

-- Core Function
local function EnableFastRun()
    if Connection then Connection:Disconnect() end
    
    Connection = RunService.Stepped:Connect(function()
        if IsActive and LocalPlayer.Character then
            local Humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            local RootPart = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            
            if Humanoid and RootPart and Humanoid.MoveDirection.Magnitude > 0 then
                -- คำนวณความเร็วผ่าน AssemblyVelocity เพื่อหลบระบบตรวจจับความเร็วพื้นฐาน
                local CurrentY = RootPart.AssemblyVelocity.Y
                local MoveVector = Humanoid.MoveDirection * SpeedValue
                RootPart.AssemblyVelocity = Vector3.new(MoveVector.X, CurrentY, MoveVector.Z)
            end
        end
    end)
end

-- Toggle Handler
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == ToggleKey then
        IsActive = not IsActive
        print("Fast Run Status:", IsActive and "ON" or "OFF")
    end
end)

-- Execute
EnableFastRun()
LocalPlayer.CharacterAdded:Connect(EnableFastRun)
