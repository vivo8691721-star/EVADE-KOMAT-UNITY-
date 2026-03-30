local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local player = game.Players.LocalPlayer

local Window = WindUI:CreateWindow({
Title = "KOMAT UNITY Hub",
Icon = "door-open",
Author = "by .komat",
Folder = "MySuperHub",

Size = UDim2.fromOffset(580, 460),  
MinSize = Vector2.new(560, 350),  
MaxSize = Vector2.new(850, 560),  
Transparent = true,  
Theme = "Dark",  
Resizable = true,  
SideBarWidth = 200,  
BackgroundImageTransparency = 0.42,  
HideSearchBar = true,  
ScrollBarEnabled = false,

})

local SettingsTab = Window:Tab({Title="Settings", Icon="wrench"})
local MainTab     = Window:Tab({Title="Main Menu", Icon="star"})
local TeleportTab = Window:Tab({Title="Teleport", Icon="navigation"})
local VisualsTab  = Window:Tab({Title="Visuals", Icon="eye"})
local ExtraTab    = Window:Tab({Title="Extras", Icon="tag"})
local FPSTab      = Window:Tab({Title="FPS", Icon="speedometer"})
local MapTab    = Window:Tab({Title="Map", Icon="map"})
local EventTab    = Window:Tab({Title="Events", Icon="calendar"})

local autoJump = false
local floatingJumpButton
local FloatingGui

-- GUI ปุ่มลอย
FloatingGui = Instance.new("ScreenGui")
FloatingGui.Name = "FloatingJumpGui"
FloatingGui.ResetOnSpawn = false
FloatingGui.Enabled = false
FloatingGui.Parent = player:WaitForChild("PlayerGui")

-- สร้างปุ่มลอย
local function createFloatingJumpButton()

if floatingJumpButton then return end  

local btn = Instance.new("TextButton")  
btn.Size = UDim2.new(0,140,0,52)  
btn.Position = UDim2.new(0.5,-70,0.8,0)  
btn.AnchorPoint = Vector2.new(0.5,0)  
btn.BackgroundColor3 = Color3.fromRGB(180,220,255)  
btn.BackgroundTransparency = 0.35  
btn.TextColor3 = Color3.fromRGB(0,70,150)  
btn.Text = "Auto Jump : OFF"  
btn.Font = Enum.Font.GothamBold  
btn.TextSize = 14  
btn.Parent = FloatingGui  
btn.Active = true  
btn.Draggable = true  

local corner = Instance.new("UICorner", btn)  
corner.CornerRadius = UDim.new(0,18)  

local stroke = Instance.new("UIStroke", btn)  
stroke.Thickness = 2.5  
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border  
stroke.Color = Color3.fromRGB(80,170,255)  

floatingJumpButton = btn  

task.spawn(function()  
	while btn.Parent do  

		TweenService:Create(  
			stroke,  
			TweenInfo.new(0.9, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),  
			{Color = Color3.fromRGB(0,90,255)}  
		):Play()  

		task.wait(0.9)  

		TweenService:Create(  
			stroke,  
			TweenInfo.new(0.9, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),  
			{Color = Color3.fromRGB(150,230,255)}  
		):Play()  

		task.wait(0.9)  

	end  
end)  

btn.MouseButton1Click:Connect(function()  

	autoJump = not autoJump  

	if autoJump then  
		btn.Text = "Auto Jump : ON"  
	else  
		btn.Text = "Auto Jump : OFF"  
	end  

end)

end

createFloatingJumpButton()

-- Toggle Auto Jump
MainTab:Toggle({
Title = "Auto Jump",
Default = false,
Callback = function(state)

autoJump = state  

    if floatingJumpButton then  
        if state then  
            floatingJumpButton.Text = "Auto Jump : ON"  
        else  
            floatingJumpButton.Text = "Auto Jump : OFF"  
        end  
    end  

end

})

-- Toggle Float Button
MainTab:Toggle({
Title = "Float Auto Jump",
Default = false,
Callback = function(state)

FloatingGui.Enabled = state  

    if not state then  
        autoJump = false  

        if floatingJumpButton then  
            floatingJumpButton.Text = "Auto Jump : OFF"  
        end  
    end  

end

})

-- ระบบ Auto Jump
RunService.RenderStepped:Connect(function()

if autoJump then  

	local char = player.Character  
	if char then  

		local hum = char:FindFirstChildOfClass("Humanoid")  

		if hum then  
			if hum.FloorMaterial ~= Enum.Material.Air then  
				hum:ChangeState(Enum.HumanoidStateType.Jumping)  
			end  
		end  

	end  

end

end)

-- =========================
-- Services & Player
-- =========================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

-- =========================
-- Floating GUI
-- =========================
local FloatingGui = Instance.new("ScreenGui")
FloatingGui.Name = "FloatingGui"
FloatingGui.ResetOnSpawn = false
FloatingGui.Parent = player:WaitForChild("PlayerGui")

-- =====================
-- HEAVY FPS LAG SYSTEM
-- =====================
local lagging = false
local floatingLagButton

local function lagFPSHeavy(time)
	if lagging then return end
	lagging = true

	task.spawn(function()
		local start = tick()
		while tick() - start < time do
			local t = {}
			for i = 1, 4e6 do
				t[i] = math.random()
			end
			RunService.Heartbeat:Wait()
		end
		lagging = false
	end)
end

-- =========================
-- ปุ่มลอย Lag Switch
-- =========================
local function createFloatingLagButton()
	if floatingLagButton then return end

	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0,140,0,52)
	btn.Position = UDim2.new(0.5,-70,0.87,0)
	btn.AnchorPoint = Vector2.new(0.5,0)
	btn.BackgroundColor3 = Color3.fromRGB(180,220,255)
	btn.BackgroundTransparency = 0.35
	btn.TextColor3 = Color3.fromRGB(0,70,150)
	btn.Text = "Lag Switch"
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.Parent = FloatingGui
	btn.Active = true
	btn.Draggable = true

	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0,18)

	-- ขอบปุ่มจริง
	local stroke = Instance.new("UIStroke", btn)
	stroke.Thickness = 2.5
	stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
	stroke.Color = Color3.fromRGB(0,120,255)

	-- ไล่สีขอบ น้ำเงิน ↔ ฟ้าอ่อน
	task.spawn(function()
		while btn.Parent do
			TweenService:Create(
				stroke,
				TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
				{Color = Color3.fromRGB(0,80,255)}
			):Play()
			task.wait(0.8)
			TweenService:Create(
				stroke,
				TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
				{Color = Color3.fromRGB(160,230,255)}
			):Play()
			task.wait(0.8)
		end
	end)

	btn.MouseButton1Click:Connect(function()
		lagFPSHeavy(0.5)
	end)

	floatingLagButton = btn
end

local function removeFloatingLagButton()
	if floatingLagButton then
		floatingLagButton:Destroy()
		floatingLagButton = nil
	end
end

-- =========================
-- ปุ่มในเมนูหลัก
-- =========================
MainTab:Button({
	Title = "Lag Switch ",
	Desc = "FPS -1 ",
	Callback = function()
		lagFPSHeavy(0.5)
	end
})

MainTab:Toggle({
	Title = "Lag Switch (float)",
	Desc = "Floating button",
	Value = false,
	Callback = function(v)
		if v then
			createFloatingLagButton()
		else
			removeFloatingLagButton()
		end
	end
})

-- =========================
-- AUTO BOUNCE SYSTEM
-- =========================
local autoBounce = false
local floatingBounceButton
local bouncePower = 150
local groundCheckDistance = 8

task.spawn(function()
	while true do
		RunService.Heartbeat:Wait()
		if not autoBounce then continue end

		local char = player.Character
		if not char then continue end

		local root = char:FindFirstChild("HumanoidRootPart")
		local humanoid = char:FindFirstChildOfClass("Humanoid")
		if not root or not humanoid then continue end

		local rayOrigin = root.Position
		local rayDirection = Vector3.new(0, -groundCheckDistance, 0)
		local params = RaycastParams.new()
		params.FilterDescendantsInstances = {char}
		params.FilterType = Enum.RaycastFilterType.Blacklist

		local ray = workspace:Raycast(rayOrigin, rayDirection, params)

		if ray and root.Velocity.Y < 0 then
			root.Velocity = Vector3.new(root.Velocity.X, bouncePower, root.Velocity.Z)
		end
	end
end)

-- =========================
-- Floating Auto Bounce Button
-- =========================
local function createFloatingBounceButton()
	if floatingBounceButton then return end

	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0,140,0,52)
	btn.Position = UDim2.new(0.5,-70,0.78,0)
	btn.AnchorPoint = Vector2.new(0.5,0)
	btn.BackgroundColor3 = Color3.fromRGB(180,220,255)
	btn.BackgroundTransparency = 0.35
	btn.TextColor3 = Color3.fromRGB(0,70,150)
	btn.Text = "Auto Bounce : OFF"
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.Parent = FloatingGui
	btn.Active = true
	btn.Draggable = true

	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0,18)

	local stroke = Instance.new("UIStroke", btn)
	stroke.Thickness = 2.5
	stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
	stroke.Color = Color3.fromRGB(0,120,255)

	task.spawn(function()
		while btn.Parent do
			TweenService:Create(
				stroke,
				TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
				{Color = Color3.fromRGB(0,80,255)}
			):Play()
			task.wait(0.8)
			TweenService:Create(
				stroke,
				TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
				{Color = Color3.fromRGB(160,230,255)}
			):Play()
			task.wait(0.8)
		end
	end)

	btn.MouseButton1Click:Connect(function()
		autoBounce = not autoBounce
		btn.Text = autoBounce and "Auto Bounce : ON" or "Auto Bounce : OFF"
	end)

	floatingBounceButton = btn
end

local function removeFloatingBounceButton()
	if floatingBounceButton then
		floatingBounceButton:Destroy()
		floatingBounceButton = nil
	end
end

-- =========================
-- Main Menu Buttons
-- =========================
MainTab:Toggle({
	Title = "Auto Bounce (Normal)",
	Desc = "Fall from height → Bounce up 150",
	Value = false,
	Callback = function(v)
		autoBounce = v
	end
})

MainTab:Toggle({
	Title = "Auto Bounce (Floating Button)",
	Desc = "Light blue floating button with animated blue border",
	Value = false,
	Callback = function(v)
		if v then
			createFloatingBounceButton()
		else
			removeFloatingBounceButton()
			autoBounce = false
		end
	end
})

-- =========================
-- Services
-- =========================
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

-- =========================
-- Auto Respawn Settings
-- =========================
getgenv().AutoRespawnEnabled = false
local autoRespawnMethod = "Fake Revive"
local lastSavedPosition
local floatingRespawnButton

-- =========================
-- Setup Auto Respawn Function
-- =========================
local function setupAutoRevive(character)
    task.defer(function()
        character:WaitForChild("HumanoidRootPart",5)
        character:WaitForChild("Humanoid",5)
        
        -- Save last position
        task.spawn(function()
            while character and character.Parent do
                local hrp = character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    character:SetAttribute("LastPosition", hrp.Position)
                end
                task.wait(0.2)
            end
        end)

        -- Detect Downed state
        character:GetAttributeChangedSignal("Downed"):Connect(function()
            if not getgenv().AutoRespawnEnabled then return end
            if character:GetAttribute("Downed") ~= true then return end
            if autoRespawnMethod ~= "Fake Revive" then return end

            local hrp = character:FindFirstChild("HumanoidRootPart")
            if hrp then lastSavedPosition = hrp.Position end

            task.wait(0.5)

            -- Instant respawn
            local start = tick()
            repeat
                pcall(function()
                    game:GetService("ReplicatedStorage"):WaitForChild("Events",9e9)
                        :WaitForChild("Player",9e9)
                        :WaitForChild("ChangePlayerMode",9e9)
                        :FireServer(true)
                end)
                task.wait(0.5)
            until character:GetAttribute("Downed") ~= true or tick() - start > 2

            local newChar
            repeat
                newChar = player.Character
                task.wait()
            until newChar and newChar:FindFirstChild("HumanoidRootPart")

            local newHRP = newChar:FindFirstChild("HumanoidRootPart")
            if lastSavedPosition and newHRP then
                newHRP.CFrame = CFrame.new(lastSavedPosition)
            end
        end)
    end)
end

-- Setup for current and new characters
if player.Character then setupAutoRevive(player.Character) end
player.CharacterAdded:Connect(setupAutoRevive)

-- =========================
-- Floating Button Function
-- =========================
local function createRespawnFloatingButton()
	if floatingRespawnButton then return end

	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0,140,0,50)
	btn.Position = UDim2.new(0.8,0,0.8,0)
	btn.AnchorPoint = Vector2.new(0.5,0)
	btn.BackgroundColor3 = Color3.fromRGB(180,220,255)
	btn.BackgroundTransparency = 0.3
	btn.TextColor3 = Color3.fromRGB(0,0,150)
	btn.Font = Enum.Font.GothamBold
	btn.Text = "Auto Respawn : OFF"
	btn.TextSize = 14
	btn.Parent = FloatingGui
	btn.Active = true
	btn.Draggable = true

	-- Rounded corners
	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0,18)

	-- Animated border
	local stroke = Instance.new("UIStroke", btn)
	stroke.Thickness = 2.5
	stroke.Color = Color3.fromRGB(0,120,255)
	stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

	-- Color tween animation
	task.spawn(function()
		while btn.Parent do
			TweenService:Create(
				stroke,
				TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, true),
				{Color = Color3.fromRGB(0,180,255)}
			):Play()
			task.wait(0.8)
			TweenService:Create(
				stroke,
				TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, true),
				{Color = Color3.fromRGB(0,120,255)}
			):Play()
			task.wait(0.8)
		end
	end)

	-- Button click
	btn.MouseButton1Click:Connect(function()
		getgenv().AutoRespawnEnabled = not getgenv().AutoRespawnEnabled
		btn.Text = getgenv().AutoRespawnEnabled and "Auto Respawn : ON" or "Auto Respawn : OFF"
		btn.BackgroundColor3 = getgenv().AutoRespawnEnabled and Color3.fromRGB(80,255,80) or Color3.fromRGB(180,220,255)
		btn.TextColor3 = getgenv().AutoRespawnEnabled and Color3.fromRGB(0,0,0) or Color3.fromRGB(0,0,150)

		if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
			local root = player.Character.HumanoidRootPart
			root.Velocity = Vector3.new(root.Velocity.X, 50, root.Velocity.Z)
		end
	end)

	floatingRespawnButton = btn
end

local function removeRespawnFloatingButton()
	if floatingRespawnButton then
		floatingRespawnButton:Destroy()
		floatingRespawnButton = nil
	end
end

-- =========================
-- Normal Toggle in MainTab
-- =========================
MainTab:Toggle({
	Title = "Auto Respawn (Normal)",
	Desc = "Automatically respawn until turned off",
	Value = false,
	Callback = function(state)
		getgenv().AutoRespawnEnabled = state
		if state then
			if player.Character then setupAutoRevive(player.Character) end
		end
	end
})

-- =========================
-- Floating Button Toggle
-- =========================
MainTab:Toggle({
	Title = "Auto Respawn (Floating Button)",
	Desc = "Light blue floating button with animated blue border",
	Value = false,
	Callback = function(state)
		if state then
			createRespawnFloatingButton()
		else
			removeRespawnFloatingButton()
			getgenv().AutoRespawnEnabled = false
		end
	end
})

-- =========================
-- Teleport Roof
-- =========================

TeleportTab:Button({
    Title = "Teleport Roof",
    Desc = "Teleport 1300 studs upward",
    Callback = function()

        local player = game.Players.LocalPlayer
        local char = player.Character or player.CharacterAdded:Wait()
        local root = char:FindFirstChild("HumanoidRootPart")

        if root then
            root.CFrame = root.CFrame + Vector3.new(0,1300,0)
        end

    end
})

-- =========================
-- Floating Teleport Roof Button
-- =========================

local floatingRoofButton

local function createFloatingRoofButton()

    if floatingRoofButton then return end

    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0,140,0,52)
    btn.Position = UDim2.new(0.5,-70,0.70,0)
    btn.AnchorPoint = Vector2.new(0.5,0)
    btn.BackgroundColor3 = Color3.fromRGB(180,220,255)
    btn.BackgroundTransparency = 0.35
    btn.TextColor3 = Color3.fromRGB(0,70,150)
    btn.Text = "Teleport Roof"
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.Parent = FloatingGui
    btn.Active = true
    btn.Draggable = true

    local corner = Instance.new("UICorner",btn)
    corner.CornerRadius = UDim.new(0,18)

    local stroke = Instance.new("UIStroke",btn)
    stroke.Thickness = 2.5
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    stroke.Color = Color3.fromRGB(0,120,255)

    btn.MouseButton1Click:Connect(function()

        local player = game.Players.LocalPlayer
        local char = player.Character
        if not char then return end

        local root = char:FindFirstChild("HumanoidRootPart")
        if root then
            root.CFrame = root.CFrame + Vector3.new(0,1300,0)
        end

    end)

    floatingRoofButton = btn

end


local function removeFloatingRoofButton()
    if floatingRoofButton then
        floatingRoofButton:Destroy()
        floatingRoofButton = nil
    end
end


TeleportTab:Toggle({
    Title = "Teleport Roof (Floating Button)",
    Desc = "Floating button to teleport up 1300",
    Value = false,
    Callback = function(v)

        if v then
            createFloatingRoofButton()
        else
            removeFloatingRoofButton()
        end

    end
})
