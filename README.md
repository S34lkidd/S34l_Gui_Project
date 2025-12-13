-- // S34l GUI + Jumpscare Edition (Require Version) \\
-- Just do: require(scriptId).Load("YourNameHere")

local module = {}

function module.Load(ownerName)
	local Players = game:GetService("Players")
	local Workspace = game:GetService("Workspace")
	local Lighting = game:GetService("Lighting")
	local UserInputService = game:GetService("UserInputService")
	local RunService = game:GetService("RunService")

	local player = Players:FindFirstChild(ownerName)
	if not player then return end

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
	local dragging, dragInput, dragStart, startPos
	local function update(input)
		local delta = input.Position - dragStart
		frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end

	frame.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
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
		if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
			dragInput = input
		end
	end)

	UserInputService.InputChanged:Connect(function(input)
		if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
			update(input)
		end
	end)

	local function makeButton(name, text, pos)
		local btn = Instance.new("TextButton")
		btn.Name = name
		btn.Size = UDim2.new(0.45, 0, 0.15, 0)
		btn.Position = pos
		btn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
		btn.BorderSizePixel = 2
		btn.Text = text
		btn.TextColor3 = Color3.new(1, 1, 1)
		btn.TextScaled = true
		btn.Font = Enum.Font.GothamBold
		btn.Parent = frame
		return btn
	end

	local Sealify        = makeButton("Sealify",        "Sealify",          UDim2.new(0.05, 0, 0.08, 0))
	local Unanchor       = makeButton("Unanchor",       "Unanchor All",     UDim2.new(0.50, 0, 0.08, 0))
	local KickThoseSkids = makeButton("KickSkids",      "Kick All Skids",   UDim2.new(0.05, 0, 0.28, 0))
	local KillAll        = makeButton("KillAll",        "Kill All",         UDim2.new(0.50, 0, 0.28, 0))
	local JumpscareAll    = makeButton("JumpscareAll",   "Jumpscare All",    UDim2.new(0.05, 0, 0.48, 0))
	local CloseBtn       = makeButton("Close",          "Close GUI",        UDim2.new(0.50, 0, 0.48, 0))

	gui.Parent = player:WaitForChild("PlayerGui")

	-- === FUNCTIONS ===

	Sealify.MouseButton1Click:Connect(function()
		local sky = Instance.new("Sky", Lighting)
		local id = "rbxassetid://96982772730126"
		sky.SkyboxBk = id; sky.SkyboxDn = id; sky.SkyboxFt = id
		sky.SkyboxLf = id; sky.SkyboxRt = id; sky.SkyboxUp = id

		for _, part in ipairs(Workspace:GetDescendants()) do
			if part:IsA("BasePart") then
				local decal = Instance.new("Decal", part)
				decal.Texture = id
				decal.Face = Enum.NormalId.Top
			end
		end

		local sound = Instance.new("Sound", gui)
		sound.SoundId = "rbxassetid://128186476216166"
		sound.Looped = true
		sound.Volume = 10
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