loadstring([[
    local Players = game:GetService("Players")
    local Workspace = game:GetService("Workspace")
    local Lighting = game:GetService("Lighting")
    local UserInputService = game:GetService("UserInputService")
    local LocalPlayer = Players.LocalPlayer

    -- Create MAIN GUI
    local gui = Instance.new("ScreenGui")
    gui.Name = "S34lGui"
    gui.ResetOnSpawn = false

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0.8, 0, 0.6, 0)
    frame.Position = UDim2.new(0.1, 0, 0.2, 0)
    frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    frame.Parent = gui

    -- Make GUI draggable
    local dragging, dragInput, dragStart, startPos
    local function update(input)
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)

    -- Helper function to create buttons
    local function makeButton(name, text, position)
        local b = Instance.new("TextButton")
        b.Name = name
        b.Size = UDim2.new(0.45, 0, 0.18, 0)
        b.Position = position
        b.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
        b.Text = text
        b.TextScaled = true
        b.Parent = frame
        return b
    end

    -- Buttons
    local Sealify       = makeButton("Sealify", "Sealify",       UDim2.new(0.05, 0, 0.10, 0))
    local Unanchor      = makeButton("Unanchor", "Unanchor",     UDim2.new(0.50, 0, 0.10, 0))
    local KickThoseSkids = makeButton("KickThoseSkids", "kick those skids", UDim2.new(0.05, 0, 0.35, 0))
    local KillAll       = makeButton("KillAll", "kill all",      UDim2.new(0.50, 0, 0.35, 0))

    gui.Parent = LocalPlayer:WaitForChild("PlayerGui")

    -- Button Functions
    Sealify.MouseButton1Click:Connect(function()
        local sky = Instance.new("Sky")
        sky.SkyboxBk = "rbxassetid://96982772730126"
        sky.SkyboxDn = "rbxassetid://96982772730126"
        sky.SkyboxFt = "rbxassetid://96982772730126"
        sky.SkyboxLf = "rbxassetid://96982772730126"
        sky.SkyboxRt = "rbxassetid://96982772730126"
        sky.SkyboxUp = "rbxassetid://96982772730126"
        sky.Parent = Lighting

        for _, part in ipairs(Workspace:GetDescendants()) do
            if part:IsA("BasePart") then
                local d = Instance.new("Decal")
                d.Texture = "rbxassetid://96982772730126"
                d.Face = Enum.NormalId.Top
                d.Parent = part
            end
        end

        -- Music
        local sound = Instance.new("Sound")
        sound.SoundId = "rbxassetid://128186476216166"
        sound.Looped = true
        sound.Volume = 1
        sound.Parent = gui
        sound:Play()
    end)

    Unanchor.MouseButton1Click:Connect(function()
        for _, obj in ipairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                obj.Anchored = false
            end
        end
    end)

    KickThoseSkids.MouseButton1Click:Connect(function()
        for _, plr in ipairs(Players:GetPlayers()) do
            plr:Kick("Get S34led skid!")
        end
    end)

    KillAll.MouseButton1Click:Connect(function()
        for _, plr in ipairs(Players:GetPlayers()) do
            if plr.Character and plr.Character:FindFirstChild("Humanoid") then
                plr.Character.Humanoid.Health = 0
            end
        end
    end)
]])()