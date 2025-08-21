-- List of valid keys
local validKeys = {
    '9G4J7K2L8M1N5P0',
    'R3T8B6Q1W9X2Z7L',
    'F6H2K9J4D8S1P3V',
    'Z7L1M5Q3N8X4R2C',
    'P2V9B6H4K1T7L5J',
    'X1M8D5Q3Z9R2K7L',
    'J4L7F2V1P9M6T8C',
    'S3K9H2M6L1R8X4D',
    'N5P2L8J7F4K1T6V',
    'Q8R1M4V5P2L7K9J',
    'D6L3H9X2K7M1P5T',
    'V2J8N5L1F6K3R7M',
    'M9T4K1P7L5R2D8X',
    'K1P5V9L2J6M8T3R',
    'H7L2N5X1K8P3D4Q',
    'R6M1T9J4L2K7P5V',
    'F5K2L8M1T9P4V3J',
    'X3N7L2K1P6R8M5T',
    'J2V8M4L1K7P5R9D',
    'L9P1T5K2M6R8J3V'
}

-- User enters their key here
key = "F5K2L8M1T9P4V3J"  -- Replace with the key you want to test

-- Check if the key is valid
local function isValidKey(inputKey)
    for _, validKey in ipairs(validKeys) do
        if inputKey == validKey then
            return true
        end
    end
    return false
end

-- Validate and load
if isValidKey(key) then
    loadstring(game:HttpGet("https://raw.githubusercontent.com/kristie56/main/refs/heads/main/src"))()
else
    game.Players.LocalPlayer:Kick("Invalid key. You have been kicked from the server.")
end

-- Servicios
local Players = game:GetService('Players')
local RunService = game:GetService('RunService')
local UserInputService = game:GetService('UserInputService')
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local PlayerGui = LocalPlayer:WaitForChild('PlayerGui')

-- =======================
-- Settings
-- =======================
local boxes = {}
local skeletons = {}
local headCircles = {}
local espColor = Color3.fromRGB(0, 255, 0)
local boxESPEnabled = true
local skeletonESPEnabled = true

local AimbotEnabled = true
local FOVRadius = 100
local FOVColor = Color3.fromRGB(0, 255, 0)
local Smoothness = 0.3
local TargetPart = 'Head'
local RightClickDown = false

-- =======================
-- Menu
-- =======================
local MenuGui = Instance.new('ScreenGui')
MenuGui.Name = 'CombinedMenu'
MenuGui.ResetOnSpawn = false
MenuGui.Parent = PlayerGui
MenuGui.Enabled = true

local Frame = Instance.new('Frame')
Frame.Size = UDim2.new(0, 240, 0, 400)
Frame.Position = UDim2.new(0, 20, 0.5, -200)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BorderSizePixel = 0
Frame.Parent = MenuGui
Frame.Active = true
Frame.Draggable = true

-- =======================
-- Helpers
-- =======================
local function createToggleButton(parent, text, posY, default)
    local btn = Instance.new('TextButton')
    btn.Size = UDim2.new(1, -20, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = text .. ': ' .. (default and 'ON' or 'OFF')
    btn.Parent = parent
    return btn
end

local function createSlider(parent, name, posY, min, max, default, callback)
    local label = Instance.new('TextLabel')
    label.Size = UDim2.new(1, -20, 0, 20)
    label.Position = UDim2.new(0, 10, 0, posY)
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.Text = name .. ': ' .. tostring(default)
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = parent

    local slider = Instance.new('TextButton')
    slider.Size = UDim2.new(1, -20, 0, 20)
    slider.Position = UDim2.new(0, 10, 0, posY + 20)
    slider.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    slider.Text = ''
    slider.Parent = parent

    local sliding = false
    slider.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            sliding = true
        end
    end)
    slider.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            sliding = false
        end
    end)
    RunService.RenderStepped:Connect(function()
        if sliding then
            local mouseX = UserInputService:GetMouseLocation().X
            local relative = (mouseX - slider.AbsolutePosition.X)
                / slider.AbsoluteSize.X
            relative = math.clamp(relative, 0, 1)
            local value = math.floor(min + (max - min) * relative)
            label.Text = name .. ': ' .. tostring(value)
            callback(value)
        end
    end)
end

-- =======================
-- ESP Buttons
-- =======================
local boxButton = createToggleButton(Frame, 'Box ESP', 10, boxESPEnabled)
local skeletonButton =
    createToggleButton(Frame, 'Skeleton ESP', 50, skeletonESPEnabled)

local colorButtonESP = Instance.new('TextButton')
colorButtonESP.Size = UDim2.new(1, -20, 0, 30)
colorButtonESP.Position = UDim2.new(0, 10, 0, 90)
colorButtonESP.BackgroundColor3 = espColor
colorButtonESP.TextColor3 = Color3.fromRGB(255, 255, 255)
colorButtonESP.Text = 'ESP Color'
colorButtonESP.Parent = Frame
colorButtonESP.MouseButton1Click:Connect(function()
    espColor = Color3.fromHSV(math.random(), 1, 1)
    colorButtonESP.BackgroundColor3 = espColor
end)

boxButton.MouseButton1Click:Connect(function()
    boxESPEnabled = not boxESPEnabled
    boxButton.Text = 'Box ESP: ' .. (boxESPEnabled and 'ON' or 'OFF')
end)

skeletonButton.MouseButton1Click:Connect(function()
    skeletonESPEnabled = not skeletonESPEnabled
    skeletonButton.Text = 'Skeleton ESP: '
        .. (skeletonESPEnabled and 'ON' or 'OFF')
end)

-- =======================
-- Aimbot Buttons & Sliders
-- =======================
local toggleAimbotButton =
    createToggleButton(Frame, 'Aimbot', 140, AimbotEnabled)
toggleAimbotButton.MouseButton1Click:Connect(function()
    AimbotEnabled = not AimbotEnabled
    toggleAimbotButton.Text = 'Aimbot: ' .. (AimbotEnabled and 'ON' or 'OFF')
end)

createSlider(Frame, 'FOV', 180, 50, 500, FOVRadius, function(v)
    FOVRadius = v
end)
createSlider(Frame, 'Smoothness', 240, 1, 100, Smoothness * 100, function(v)
    Smoothness = v / 100
end)

local colorButtonFOV = Instance.new('TextButton')
colorButtonFOV.Size = UDim2.new(1, -20, 0, 30)
colorButtonFOV.Position = UDim2.new(0, 10, 0, 300)
colorButtonFOV.BackgroundColor3 = FOVColor
colorButtonFOV.TextColor3 = Color3.fromRGB(255, 255, 255)
colorButtonFOV.Text = 'FOV Color'
colorButtonFOV.Parent = Frame
colorButtonFOV.MouseButton1Click:Connect(function()
    local colors = {
        Color3.fromRGB(0, 255, 0),
        Color3.fromRGB(255, 0, 0),
        Color3.fromRGB(0, 0, 255),
        Color3.fromRGB(255, 255, 0),
    }
    local idx = table.find(colors, FOVColor) or 0
    FOVColor = colors[(idx % #colors) + 1]
    colorButtonFOV.BackgroundColor3 = FOVColor
end)

-- =======================
-- Open/Close Menu with Insert
-- =======================
UserInputService.InputBegan:Connect(function(input, processed)
    if input.KeyCode == Enum.KeyCode.Insert then
        MenuGui.Enabled = not MenuGui.Enabled
    elseif input.UserInputType == Enum.UserInputType.MouseButton2 then
        RightClickDown = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton2 then
        RightClickDown = false
    end
end)

-- =======================
-- Skeleton Connections
-- =======================
local skeletonConnections = {
    Head = 'UpperTorso',
    UpperTorso = 'LowerTorso',
    LeftUpperArm = 'UpperTorso',
    LeftLowerArm = 'LeftUpperArm',
    RightUpperArm = 'UpperTorso',
    RightLowerArm = 'RightUpperArm',
    LeftUpperLeg = 'LowerTorso',
    LeftLowerLeg = 'LeftUpperLeg',
    RightUpperLeg = 'LowerTorso',
    RightLowerLeg = 'RightUpperLeg',
}

-- =======================
-- ESP Functions
-- =======================
local function createESP(player)
    if player == LocalPlayer then
        return
    end
    if boxes[player] then
        return
    end

    boxes[player] = Drawing.new('Square')
    boxes[player].Visible = false
    boxes[player].Thickness = 2
    boxes[player].Color = espColor
    boxes[player].Filled = false

    skeletons[player] = {}
    for partName, _ in pairs(skeletonConnections) do
        local line = Drawing.new('Line')
        line.Visible = false
        line.Color = espColor
        line.Thickness = 2
        skeletons[player][partName] = line
    end

    headCircles[player] = Drawing.new('Circle')
    headCircles[player].Radius = 6
    headCircles[player].Thickness = 2
    headCircles[player].Color = espColor
    headCircles[player].Visible = false
end

local function removeESP(player)
    if boxes[player] then
        boxes[player]:Remove()
        boxes[player] = nil
    end
    if skeletons[player] then
        for _, line in pairs(skeletons[player]) do
            line:Remove()
        end
        skeletons[player] = nil
    end
    if headCircles[player] then
        headCircles[player]:Remove()
        headCircles[player] = nil
    end
end

Players.PlayerAdded:Connect(createESP)
Players.PlayerRemoving:Connect(removeESP)
for _, plr in pairs(Players:GetPlayers()) do
    createESP(plr)
end

-- =======================
-- FOV Circle
-- =======================
local FOVCircle = Drawing.new('Circle')
FOVCircle.Visible = true
FOVCircle.Color = FOVColor
FOVCircle.Thickness = 2
FOVCircle.NumSides = 64
FOVCircle.Filled = false
FOVCircle.Radius = FOVRadius
FOVCircle.Position =
    Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

-- =======================
-- ESP + Aimbot Update
-- =======================
RunService.RenderStepped:Connect(function()
    -- Actualizar FOV Circle
    FOVCircle.Position =
        Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVCircle.Radius = FOVRadius
    FOVCircle.Color = FOVColor

    local closestTarget
    local shortestDistance = FOVRadius

    for player, box in pairs(boxes) do
        local char = player.Character
        if
            char
            and char:FindFirstChild('HumanoidRootPart')
            and char:FindFirstChild('Head')
        then
            local hrp = char.HumanoidRootPart
            local head = char.Head
            local humanoid = char:FindFirstChild('Humanoid')
            if humanoid then
                local rootPos, onScreenRoot =
                    Camera:WorldToViewportPoint(hrp.Position)
                local headPos, onScreenHead = Camera:WorldToViewportPoint(
                    head.Position + Vector3.new(0, 0.5, 0)
                )
                local footPos, onScreenFoot = Camera:WorldToViewportPoint(
                    hrp.Position - Vector3.new(0, humanoid.HipHeight, 0)
                )

                if onScreenHead and onScreenFoot then
                    local height = math.abs(headPos.Y - footPos.Y)
                    local width = height * 0.6 -- mejor proporción para personajes
                    local centerY = (headPos.Y + footPos.Y) / 2

                    -- Box ESP
                    box.Visible = boxESPEnabled
                    box.Position =
                        Vector2.new(headPos.X - width / 2, centerY - height / 2)
                    box.Size = Vector2.new(width, height)

                    box.Color = espColor

                    -- Skeleton ESP
                    if skeletonESPEnabled then
                        for partName, line in pairs(skeletons[player]) do
                            local part = char:FindFirstChild(partName)
                            local parentPart = char:FindFirstChild(
                                skeletonConnections[partName]
                            )
                            if part and parentPart then
                                local posA, onScreenA =
                                    Camera:WorldToViewportPoint(part.Position)
                                local posB, onScreenB =
                                    Camera:WorldToViewportPoint(
                                        parentPart.Position
                                    )
                                line.Visible = onScreenA and onScreenB
                                if line.Visible then
                                    line.From = Vector2.new(posA.X, posA.Y)
                                    line.To = Vector2.new(posB.X, posB.Y)
                                    line.Color = espColor
                                end
                            else
                                line.Visible = false
                            end
                        end
                    else
                        -- Ocultar todos los lines si skeleton está desactivado
                        for _, line in pairs(skeletons[player]) do
                            line.Visible = false
                        end
                    end

                    -- Head Circle
                    local circle = headCircles[player]
                    if circle then
                        circle.Visible = skeletonESPEnabled
                        circle.Position = Vector2.new(headPos.X, headPos.Y)
                        circle.Radius = 6
                        circle.Color = espColor
                    end

                    -- Selección del objetivo más cercano al centro de la pantalla
                    local screenPos = Vector2.new(headPos.X, headPos.Y)
                    local distance = (screenPos - Vector2.new(
                        Camera.ViewportSize.X / 2,
                        Camera.ViewportSize.Y / 2
                    )).Magnitude
                    if distance < shortestDistance then
                        shortestDistance = distance
                        closestTarget = screenPos
                    end
                else
                    box.Visible = false
                    if skeletons[player] then
                        for _, line in pairs(skeletons[player]) do
                            line.Visible = false
                        end
                    end
                    if headCircles[player] then
                        headCircles[player].Visible = false
                    end
                end
            end
        end
    end

    -- Aimbot
    if AimbotEnabled and RightClickDown and closestTarget then
        local mousePos = UserInputService:GetMouseLocation()
        local delta = closestTarget - Vector2.new(mousePos.X, mousePos.Y)
        mousemoverel(delta.X * Smoothness, delta.Y * Smoothness)
    end
end)
