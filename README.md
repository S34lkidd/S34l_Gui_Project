-- S34l GUI + Jumpscare Edition (GitHub Version)
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer

local gui = Instance.new("ScreenGui")
gui.Name = "S34lGui"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.8, 0, 0.7, 0)
frame.Position = UDim2.new(0.1, 0, 0.15, 0)
frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
frame.BorderSizePixel = 3
frame.Parent = gui

-- Draggable
local dragging, dragStart, startPos
frame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
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

UserInputService.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

local function btn(name, text, pos)
	local b = Instance.new("TextButton")
	b.Name = name
	b.Size = UDim2.new(0.45, 0, 0.15, 0)
	b.Position = pos
	b.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
	b.BorderSizePixel = 2
	b.Text = text
	b.TextColor3 = Color3.new(1,1,1)
	b.TextScaled = true
	b.Font = Enum.Font.GothamBold
	b.Parent = frame
	return b
end

local Sealify      = btn("Sealify",      "Sealify",        UDim2.new(0.05, 0, 0.08, 0))
local Unanchor     = btn("Unanchor",     "Unanchor All",   UDim2.new(0.50, 0, 0.08, 0))
local KickSkids    = btn("KickSkids",    "Kick All Skids", UDim2.new(0.05, 0, 0.28, 0))
local KillAll      = btn("KillAll",      "Kill All",       UDim2.new(0.50, 0, 0.28, 0))
local Jumpscare    = btn("Jumpscare",    "Jumpscare All",  UDim2.new(0.05, 0, 0.48, 0))
local Close        = btn("Close",        "Close GUI",      UDim2.new(0.50, 0, 0.48, 0))

gui.Parent = player:WaitForChild("PlayerGui")

-- Functions
Sealify.MouseButton1Click:Connect(function()
	local id = "rbxassetid://96982772730126"
	local sky = Instance.new("Sky", Lighting)
	sky.SkyboxBk = sky.SkyboxDn = sky.SkyboxFt = sky.SkyboxLf = sky.SkyboxRt = sky.SkyboxUp = id

	for _, v in ipairs(Workspace:GetDescendants()) do
		if v:IsA("BasePart") then
			local d = Instance.new("Decal", v)
			d.Texture = id
			d.Face = Enum.NormalId.Top
		end
	end

	local s = Instance.new("Sound", gui)
	s.SoundId = "rbxassetid://128186476216166"
	s.Volume = 10
	s.Looped = true
	s:Play()
end)

Unanchor.MouseButton1Click:Connect(function()
	for _, v in ipairs(Workspace:GetDescendants()) do
		if v:IsA("BasePart") then v.Anchored = false end
	end
end)

KickSkids.MouseButton1Click:Connect(function()
	for _, p in ipairs(Players:GetPlayers()) do
		p:Kick("Get S34led skid!")
	end
end)

KillAll.MouseButton1Click:Connect(function()
	for _, p in ipairs(Players:GetPlayers()) do
		if p.Character and p.Character:FindFirstChild("Humanoid") then
			p.Character.Humanoid.Health = 0
		end
	end
end)

Jumpscare.MouseButton1Click:Connect(function()
	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= player then
			local js = Instance.new("ScreenGui", p:WaitForChild("PlayerGui"))
			js.Name = "Jumpscare"
			local img = Instance.new("ImageLabel", js)
			img.Size = UDim2.new(1,0,1,0)
			img.BackgroundTransparency = 1
			img.Image = "rbxassetid://96982772730126"
			local snd = Instance.new("Sound", js)
			snd.SoundId = "rbxassetid://7113216707"
			snd.Volume = 10
			snd:Play()
			wait(0.6)
			js:Destroy()
		end
	end
end)

Close.MouseButton1Click:Connect(function()
	gui:Destroy()
end)