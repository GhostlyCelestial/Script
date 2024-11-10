local order = {
    "Storm",
    "Robotic",
    "Stone",
    "Magic",
    "Spike",
    "Strong",
    "Web",
    "Insanity",
    "Dark",
    "Giant"
}

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local touchinterest = {}
local firetouchinterest = firetouchinterest or function(part, part2, value)
    if part and part2 then
        if value == 0 then
            touchinterest[1] = part.CFrame
            part.CFrame = part2.CFrame
        else
            part.CFrame = touchinterest[1]
            touchinterest[1] = nil
        end
    end
end

local OwnerlessTycoon = function(name)
    local tycoon = workspace.Tycoons:FindFirstChild(name)
    local ownerObject = tycoon and tycoon:FindFirstChild("isim")
    return ownerObject and not Players:FindFirstChild(ownerObject.Value)
end

local ClaimTycoon = function(name)
    local tycoon = workspace.Tycoons:FindFirstChild(name)
    local claimDoor = tycoon and tycoon:FindFirstChild("Door")
    local root = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if claimDoor and root then
        for _, v in pairs(claimDoor:GetChildren()) do
            if v:IsA("BasePart") then
                firetouchinterest(root, v, 0)
                firetouchinterest(root, v, 1)
            end
        end
    end
end

local finishedClaiming = false

for _, name in ipairs(order) do
    if finishedClaiming then break end

    if OwnerlessTycoon(name) then
        ClaimTycoon(name)

        local tycoon = workspace.Tycoons:FindFirstChild(name)
        local ownerObject = tycoon and tycoon:FindFirstChild("isim")

        local checks = 0

        while checks < 5 do
            if Players:FindFirstChild(ownerObject.Value) == LocalPlayer then
                finishedClaiming = true
                break
            end
            wait(0.4)
            checks = checks + 1
        end
    end
end
# Script
