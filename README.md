-- Settings
local defaultSpeed = 16
local targetSpeed = 100 -- ปรับความเร็วตรงนี้ได้ตั้งแต่ 1 ถึง 800
local maxAllowedSpeed = 800

-- Main Logic
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local function applySmoothSpeed(character)
    local humanoid = character:WaitForChild("Humanoid")
    local rootPart = character:WaitForChild("HumanoidRootPart")
    
    -- ล็อคไม่ให้เกิน 800
    targetSpeed = math.clamp(targetSpeed, 1, maxAllowedSpeed)
    
    -- ปรับ WalkSpeed พื้นฐาน
    humanoid.WalkSpeed = targetSpeed

    -- Bypass Anti-Cheat โดยการประมวลผลตำแหน่งแบบรวดเร็วแต่แนบเนียน
    RunService.Heartbeat:Connect(function(deltaTime)
        if humanoid and rootPart and humanoid.MoveDirection.Magnitude > 0 then
            humanoid.WalkSpeed = targetSpeed
            -- เพิ่มแรงส่งเล็กน้อยเพื่อลดอาการติดดักจับความเร็วจากเซิร์ฟเวอร์
            local extraVelocity = humanoid.MoveDirection * (targetSpeed * 0.1)
            rootPart.AssemblyLinearVelocity = Vector3.new(
                extraVelocity.X,
                rootPart.AssemblyLinearVelocity.Y,
                extraVelocity.Z
            )
        end
    end)
end

-- ทำงานทันทีและทำงานซ้ำเมื่อตัวละครเกิดใหม่
if LocalPlayer.Character then
    applySmoothSpeed(LocalPlayer.Character)
end

LocalPlayer.CharacterAdded:Connect(function(char)
    applySmoothSpeed(char)
end)
