repeat
	task.wait()
until game:IsLoaded()

-- ============================================================
-- ASTA DUELS PREMIUM - BLACK/PURPLE EDITION
-- FIXED: Player ESP hindi na nawawala kapag nag-aaimbot
-- RED Player ESP + RED Aimbot ESP
-- ADDED: Anti-Lag, Optimizer, Sky Theme sa Mechanics page
-- REMOVED: K7 Speed Bypass
-- REMOVED: Background images from all stack buttons
-- ============================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local HttpService = game:GetService("HttpService")
local Stats = game:GetService("Stats")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local LP = Players.LocalPlayer
local CoreGui = game:GetService("CoreGui")

-- ============================================================
-- SKY THEME SYSTEM (FROM CANDYHUB)
-- ============================================================
local CANDY_SKY_TAG = "AstaSkyTheme"
_G._AstaSkyMode = _G._AstaSkyMode or "Off"
local candyOriginalLighting = nil

local CANDY_SKY_PRESETS = {
	["Off"] = {
		kind = "off"
	},
	["Night"] = {
		clock = 22,
		brightness = 2,
		ambient = {
			110,
			100,
			130
		},
		outAmb = {
			120,
			110,
			140
		},
		sky = {
			stars = 4000,
			moon = 18,
			sun = 0,
			moonTex = true
		},
		atm = {
			dens = 0.45,
			color = {
				120,
				60,
				180
			},
			decay = {
				60,
				20,
				100
			},
			glare = 0.5,
			haze = 1.2
		},
	},
	["Aurora"] = {
		clock = 14,
		brightness = 3,
		ambient = {
			150,
			120,
			150
		},
		outAmb = {
			160,
			130,
			160
		},
		atm = {
			dens = 0.55,
			color = {
				255,
				80,
				200
			},
			decay = {
				255,
				20,
				150
			},
			glare = 2.5,
			haze = 3
		},
		clouds = {
			cover = 0.7,
			dens = 0.7,
			color = {
				255,
				240,
				250
			}
		},
	},
	["Sunset"] = {
		clock = 17.2,
		brightness = 2.5,
		ambient = {
			170,
			120,
			100
		},
		outAmb = {
			180,
			130,
			110
		},
		sky = {
			stars = 0,
			sun = 25,
			moon = 0
		},
		atm = {
			dens = 0.5,
			color = {
				255,
				130,
				60
			},
			decay = {
				255,
				80,
				30
			},
			glare = 2,
			haze = 2.5
		},
		clouds = {
			cover = 0.55,
			dens = 0.55,
			color = {
				255,
				200,
				140
			}
		},
	},
	["Galaxy"] = {
		clock = 0,
		brightness = 1.5,
		ambient = {
			70,
			60,
			100
		},
		outAmb = {
			80,
			70,
			110
		},
		sky = {
			stars = 10000,
			moon = 30,
			sun = 0
		},
		atm = {
			dens = 0.15,
			color = {
				40,
				20,
				80
			},
			decay = {
				20,
				10,
				50
			},
			glare = 0.3,
			haze = 0.5
		},
	},
	["Cyber"] = {
		clock = 21,
		brightness = 2.2,
		ambient = {
			90,
			130,
			170
		},
		outAmb = {
			100,
			140,
			190
		},
		sky = {
			stars = 2000,
			moon = 12
		},
		atm = {
			dens = 0.4,
			color = {
				0,
				200,
				255
			},
			decay = {
				150,
				0,
				255
			},
			glare = 2,
			haze = 2
		},
		clouds = {
			cover = 0.4,
			dens = 0.6,
			color = {
				100,
				200,
				255
			}
		},
	},
	["Sakura"] = {
		clock = 11,
		brightness = 3.5,
		ambient = {
			170,
			150,
			160
		},
		outAmb = {
			180,
			160,
			170
		},
		sky = {
			sun = 8
		},
		atm = {
			dens = 0.3,
			color = {
				255,
				200,
				220
			},
			decay = {
				255,
				170,
				200
			},
			glare = 1,
			haze = 1.5
		},
		clouds = {
			cover = 0.6,
			dens = 0.4,
			color = {
				255,
				250,
				252
			}
		},
	},
	["Pink Night"] = {
		clock = 23,
		brightness = 2.2,
		ambient = {
			120,
			60,
			110
		},
		outAmb = {
			140,
			70,
			120
		},
		sky = {
			stars = 5000,
			moon = 22,
			sun = 0,
			moonTex = true
		},
		atm = {
			dens = 0.5,
			color = {
				255,
				80,
				180
			},
			decay = {
				140,
				30,
				100
			},
			glare = 0.7,
			haze = 1.4
		},
		clouds = {
			cover = 0.3,
			dens = 0.5,
			color = {
				180,
				90,
				150
			}
		},
	},
	["Blood Moon"] = {
		clock = 22.5,
		brightness = 1.6,
		ambient = {
			130,
			40,
			40
		},
		outAmb = {
			150,
			50,
			50
		},
		sky = {
			stars = 1500,
			moon = 28,
			sun = 0,
			moonTex = true
		},
		atm = {
			dens = 0.6,
			color = {
				220,
				30,
				30
			},
			decay = {
				120,
				10,
				10
			},
			glare = 1.4,
			haze = 2
		},
		clouds = {
			cover = 0.5,
			dens = 0.7,
			color = {
				120,
				30,
				30
			}
		},
	},
	["Volcanic"] = {
		clock = 19,
		brightness = 2,
		ambient = {
			180,
			80,
			40
		},
		outAmb = {
			200,
			90,
			50
		},
		sky = {
			stars = 200,
			sun = 12,
			moon = 0
		},
		atm = {
			dens = 0.75,
			color = {
				255,
				60,
				0
			},
			decay = {
				180,
				20,
				0
			},
			glare = 3,
			haze = 3.5
		},
		clouds = {
			cover = 0.8,
			dens = 0.9,
			color = {
				120,
				40,
				20
			}
		},
	}
}

local function candySaveOriginalLighting()
	if candyOriginalLighting then
		return
	end
	candyOriginalLighting = {
		ClockTime = Lighting.ClockTime,
		OutdoorAmbient = Lighting.OutdoorAmbient,
		Ambient = Lighting.Ambient,
		Brightness = Lighting.Brightness,
		FogStart = Lighting.FogStart,
		FogEnd = Lighting.FogEnd,
		FogColor = Lighting.FogColor,
		ColorShift_Top = Lighting.ColorShift_Top,
		ColorShift_Bottom = Lighting.ColorShift_Bottom,
		GeographicLatitude = Lighting.GeographicLatitude,
		GlobalShadows = Lighting.GlobalShadows,
		LightingChildren = {},
		TerrainChildren = {}
	}
	for _, child in ipairs(Lighting:GetChildren()) do
		if child:IsA("Sky") or child:IsA("Atmosphere") then
			table.insert(candyOriginalLighting.LightingChildren, child:Clone())
		end
	end
	local terrain = workspace:FindFirstChildOfClass("Terrain")
	if terrain then
		for _, child in ipairs(terrain:GetChildren()) do
			if child:IsA("Clouds") then
				table.insert(candyOriginalLighting.TerrainChildren, child:Clone())
			end
		end
	end
end

local function candyClearSky(removeAll)
	for _, child in ipairs(Lighting:GetChildren()) do
		if child:GetAttribute(CANDY_SKY_TAG) or (removeAll and (child:IsA("Sky") or child:IsA("Atmosphere"))) then
			pcall(function()
				child:Destroy()
			end)
		end
	end
	local terrain = workspace:FindFirstChildOfClass("Terrain")
	if terrain then
		for _, child in ipairs(terrain:GetChildren()) do
			if child:GetAttribute(CANDY_SKY_TAG) or (removeAll and child:IsA("Clouds")) then
				pcall(function()
					child:Destroy()
				end)
			end
		end
	end
end

local function candyInstance(className, parent, props)
	local inst = Instance.new(className)
	inst:SetAttribute(CANDY_SKY_TAG, true)
	for k, v in pairs(props or {}) do
		pcall(function()
			inst[k] = v
		end)
	end
	inst.Parent = parent
	return inst
end

local function candyColor(rgb)
	return Color3.fromRGB(rgb[1], rgb[2], rgb[3])
end

local function CandyApplyCustomSky(mode)
	candySaveOriginalLighting()
	candyClearSky(true)
	local terrain = workspace:FindFirstChildOfClass("Terrain")
	local preset = CANDY_SKY_PRESETS[mode]
	if not preset or preset.kind == "off" then
		if candyOriginalLighting then
			for k, v in pairs(candyOriginalLighting) do
				if k ~= "LightingChildren" and k ~= "TerrainChildren" then
					pcall(function()
						Lighting[k] = v
					end)
				end
			end
			for _, child in ipairs(candyOriginalLighting.LightingChildren or {}) do
				child:Clone().Parent = Lighting
			end
			local offTerrain = workspace:FindFirstChildOfClass("Terrain")
			if offTerrain then
				for _, child in ipairs(candyOriginalLighting.TerrainChildren or {}) do
					child:Clone().Parent = offTerrain
				end
			end
		end
		_G._AstaSkyMode = "Off"
		return
	end
	Lighting.FogStart = 0
	Lighting.FogEnd = 100000
	Lighting.FogColor = Color3.fromRGB(200, 200, 200)
	Lighting.ColorShift_Top = Color3.fromRGB(0, 0, 0)
	Lighting.ColorShift_Bottom = Color3.fromRGB(0, 0, 0)
	Lighting.GlobalShadows = true
	Lighting.ClockTime = preset.clock or 14
	Lighting.Brightness = preset.brightness or 2
	if preset.outAmb then
		Lighting.OutdoorAmbient = candyColor(preset.outAmb)
	end
	if preset.ambient then
		Lighting.Ambient = candyColor(preset.ambient)
	end
	if preset.sky then
		local skyProps = {}
		if preset.sky.stars then
			skyProps.StarCount = preset.sky.stars
		end
		if preset.sky.moon then
			skyProps.MoonAngularSize = preset.sky.moon
		end
		if preset.sky.sun then
			skyProps.SunAngularSize = preset.sky.sun
		end
		if preset.sky.moonTex then
			skyProps.MoonTextureId = "rbxasset://sky/moon.jpg"
		end
		candyInstance("Sky", Lighting, skyProps)
	end
	if preset.atm then
		candyInstance("Atmosphere", Lighting, {
			Density = preset.atm.dens or 0.3,
			Color = candyColor(preset.atm.color),
			Decay = candyColor(preset.atm.decay),
			Glare = preset.atm.glare or 1,
			Haze = preset.atm.haze or 1
		})
	end
	if preset.clouds and terrain then
		candyInstance("Clouds", terrain, {
			Cover = preset.clouds.cover or 0.5,
			Density = preset.clouds.dens or 0.5,
			Color = candyColor(preset.clouds.color)
		})
	end
	_G._AstaSkyMode = mode
end

local CandySkyOrder = {
	{
		"Off",
		"Off"
	},
	{
		"Night",
		"Night"
	},
	{
		"Aurora",
		"Aurora"
	},
	{
		"Sunset",
		"Sunset"
	},
	{
		"Galaxy",
		"Galaxy"
	},
	{
		"Cyber",
		"Cyber"
	},
	{
		"Sakura",
		"Sakura"
	},
	{
		"Pink Night",
		"Pink Night"
	},
	{
		"Blood Moon",
		"Blood Moon"
	},
	{
		"Volcanic",
		"Volcanic"
	}
}
local currentSkyTheme = _G._AstaSkyMode or "Off"

-- ============================================================
-- NIGHT MODE FUNCTIONS (REMOVED - replaced by Sky Theme)
-- ============================================================
local nightModeEnabled = false
local function setNightMode(enabled)
	nightModeEnabled = enabled
	if enabled then
		CandyApplyCustomSky("Night")
	else
		CandyApplyCustomSky("Off")
	end
end

-- ============================================================
-- ANTI-LAG (Clean)
-- ============================================================
local antiLagEnabled = false
local antiLagConn = nil

local function applyAntiLag()
	pcall(function()
		for _, obj in ipairs(workspace:GetDescendants()) do
			if obj:IsA("ParticleEmitter") or obj:IsA("PointLight") or obj:IsA("SpotLight") or obj:IsA("Beam") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") or obj:IsA("Trail") then
				obj:Destroy()
			end
		end
		for _, obj in ipairs(Lighting:GetDescendants()) do
			if obj:IsA("BloomEffect") or obj:IsA("BlurEffect") or obj:IsA("SunRaysEffect") or obj:IsA("DepthOfFieldEffect") or obj:IsA("ColorCorrectionEffect") then
				obj:Destroy()
			end
		end
		Lighting.GlobalShadows = false
		Lighting.FogEnd = 9e9
	end)
end

local function startAntiLag()
	antiLagEnabled = true
	applyAntiLag()
	if antiLagConn then
		antiLagConn:Disconnect()
	end
	antiLagConn = workspace.DescendantAdded:Connect(function(obj)
		if antiLagEnabled then
			pcall(function()
				if obj:IsA("ParticleEmitter") or obj:IsA("PointLight") or obj:IsA("SpotLight") or obj:IsA("Beam") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") or obj:IsA("Trail") then
					obj:Destroy()
				end
			end)
		end
	end)
end

local function stopAntiLag()
	antiLagEnabled = false
	if antiLagConn then
		antiLagConn:Disconnect()
		antiLagConn = nil
	end
end

local function toggleAntiLag(on)
	if on then
		startAntiLag()
	else
		stopAntiLag()
	end
	pcall(saveConfig)
end

-- ============================================================
-- OPTIMIZER
-- ============================================================
local optimizerEnabled = false

local function applyOptimizer()
	pcall(function()
		settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
		Lighting.GlobalShadows = false
		Lighting.FogEnd = 9e9
		Lighting.Brightness = 2
		for _, obj in ipairs(workspace:GetDescendants()) do
			pcall(function()
				if obj:IsA("ParticleEmitter") or obj:IsA("Smoke") or obj:IsA("Fire") or obj:IsA("Sparkles") or obj:IsA("Trail") then
					obj.Enabled = false
				end
				if obj:IsA("MeshPart") then
					obj.CastShadow = false
					obj.RenderFidelity = Enum.RenderFidelity.Performance
				end
				if obj:IsA("BasePart") then
					obj.CastShadow = false
					obj.Material = Enum.Material.Plastic
				end
			end)
		end
	end)
end

local function startOptimizer()
	optimizerEnabled = true
	applyOptimizer()
end

local function stopOptimizer()
	optimizerEnabled = false
	pcall(function()
		settings().Rendering.QualityLevel = Enum.QualityLevel.Automatic
		Lighting.GlobalShadows = true
		Lighting.FogEnd = 100000
		Lighting.Brightness = 1
	end)
end

local function toggleOptimizer(on)
	if on then
		startOptimizer()
	else
		stopOptimizer()
	end
	pcall(saveConfig)
end

-- ============================================================
-- PLAYER ESP FUNCTIONS (RED) - HINDI NA NAWAWALA
-- ============================================================
local espEnabled = false
local espConns = {}
local espCharAddedConns = {}

local function addESPToCharacter(char)
	if not espEnabled then
		return
	end
	if not char or char.Parent ~= Workspace then
		return
	end
	local existing = char:FindFirstChild("AstaESP_Highlight")
	if existing then
		existing:Destroy()
	end
	local hl = Instance.new("Highlight", char)
	hl.Name = "AstaESP_Highlight"
	hl.FillColor = Color3.fromRGB(255, 0, 0)  -- RED
	hl.OutlineColor = Color3.fromRGB(200, 0, 0)  -- DARK RED
	hl.FillTransparency = 0.5
	hl.OutlineTransparency = 0
	hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
end

local function removeESPFromCharacter(char)
	local hl = char:FindFirstChild("AstaESP_Highlight")
	if hl then
		hl:Destroy()
	end
end

local function stopESP()
	espEnabled = false
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr.Character then
			removeESPFromCharacter(plr.Character)
		end
	end
	for _, conn in ipairs(espConns) do
		pcall(function()
			conn:Disconnect()
		end)
	end
	espConns = {}
	if espCharAddedConns.charAdded then
		espCharAddedConns.charAdded:Disconnect()
		espCharAddedConns.charAdded = nil
	end
end

local function startESP()
	if espEnabled then
		return
	end
	espEnabled = true
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr ~= LP then
			if plr.Character then
				addESPToCharacter(plr.Character)
			end
			local conn = plr.CharacterAdded:Connect(function(char)
				task.wait(0.2)
				if espEnabled then
					addESPToCharacter(char)
				end
			end)
			table.insert(espConns, conn)
		end
	end
	espCharAddedConns.charAdded = Players.PlayerAdded:Connect(function(plr)
		if plr == LP then
			return
		end
		local conn = plr.CharacterAdded:Connect(function(char)
			task.wait(0.2)
			if espEnabled then
				addESPToCharacter(char)
			end
		end)
		table.insert(espConns, conn)
	end)
end

local function toggleESP(on)
	if on then
		startESP()
	else
		stopESP()
	end
end

-- ============================================================
-- IRISH STEAL MODULE (BLACK/PURPLE THEME - ELEGANT STEAL BAR)
-- ============================================================
local AutoSteal = {
	AutoStealEnabled = false,
	StealRadius = 60,
	StealDuration = 1.4,
	isStealing = false,
	StealData = {},
	progressBarBg = nil,
	progressFill = nil,
	percentLabel = nil,
	bannerFrame = nil,
	infoLabel = nil,
	plotCache = {},
	plotCacheTime = {},
	cachedPrompts = {},
	promptCacheTime = 0,
	lastStealTick = 0,
	progressConn = nil,
	autoStealConn = nil,
	container = nil,
	isDragging = false,
	dragStartPos = nil,
	startContainerPos = nil,
	uiLocked = false,
}

local STEAL_COOLDOWN = 0.1
local PLOT_CACHE_DURATION = 2

-- ============================================================
-- ELEGANT STEAL BAR UI - MODERN GLASS DESIGN
-- ============================================================
local function setupIrishUI()
	local sg = LP.PlayerGui:FindFirstChild("AstaAutoStealGui")
	if sg then
		sg:Destroy()
	end
	sg = Instance.new("ScreenGui")
	sg.Name = "AstaAutoStealGui"
	sg.ResetOnSpawn = false
	sg.Parent = LP.PlayerGui
	sg.IgnoreGuiInset = true
	sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    -- Main Container - Sleek, compact design
	local container = Instance.new("Frame")
	container.Size = UDim2.new(0, 280, 0, 48)
	container.Position = UDim2.new(0.5, - 140, 0, 35)
	container.BackgroundTransparency = 1
	container.Parent = sg
	AutoSteal.container = container

    -- Glossy background with gradient
	local bg = Instance.new("Frame")
	bg.Size = UDim2.new(1, 0, 1, 0)
	bg.Position = UDim2.new(0, 0, 0, 0)
	bg.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
	bg.BackgroundTransparency = 0.15
	bg.BorderSizePixel = 0
	bg.Parent = container
	local bgCorner = Instance.new("UICorner", bg)
	bgCorner.CornerRadius = UDim.new(0, 12)

    -- Glass blur effect
	local blur = Instance.new("BlurEffect", sg)
	blur.Size = 8
	blur.Enabled = false

    -- Border with glow
	local border = Instance.new("Frame")
	border.Size = UDim2.new(1, 0, 1, 0)
	border.Position = UDim2.new(0, 0, 0, 0)
	border.BackgroundTransparency = 1
	border.BorderSizePixel = 2
	border.BorderColor3 = Color3.fromRGB(128, 0, 255)
	border.Parent = container
	local borderCorner = Instance.new("UICorner", border)
	borderCorner.CornerRadius = UDim.new(0, 12)

    -- Glow effect (subtle)
	local glow = Instance.new("ImageLabel")
	glow.Size = UDim2.new(1, 20, 1, 20)
	glow.Position = UDim2.new(- 0.5, - 10, - 0.5, - 10)
	glow.BackgroundTransparency = 1
	glow.Image = "rbxassetid://5028857089"
	glow.ImageColor3 = Color3.fromRGB(128, 0, 255)
	glow.ImageTransparency = 0.5
	glow.Parent = container
	glow.ZIndex = 0

    -- Banner / Title Bar
	AutoSteal.bannerFrame = Instance.new("Frame")
	AutoSteal.bannerFrame.Size = UDim2.new(1, 0, 0, 22)
	AutoSteal.bannerFrame.Position = UDim2.new(0, 0, 0, 0)
	AutoSteal.bannerFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	AutoSteal.bannerFrame.BackgroundTransparency = 0.3
	AutoSteal.bannerFrame.BorderSizePixel = 0
	AutoSteal.bannerFrame.Parent = container
	local bannerCorner = Instance.new("UICorner", AutoSteal.bannerFrame)
	bannerCorner.CornerRadius = UDim.new(0, 12)

    -- Asta Duels Logo / Icon
	local icon = Instance.new("Frame")
	icon.Size = UDim2.new(0, 16, 0, 16)
	icon.Position = UDim2.new(0, 6, 0.5, - 8)
	icon.BackgroundColor3 = Color3.fromRGB(128, 0, 255)
	icon.BorderSizePixel = 0
	icon.Parent = AutoSteal.bannerFrame
	local iconCorner = Instance.new("UICorner", icon)
	iconCorner.CornerRadius = UDim.new(0, 4)
	AutoSteal.infoLabel = Instance.new("TextLabel")
	AutoSteal.infoLabel.Size = UDim2.new(0.7, 0, 1, 0)
	AutoSteal.infoLabel.Position = UDim2.new(0, 26, 0, 0)
	AutoSteal.infoLabel.BackgroundTransparency = 1
	AutoSteal.infoLabel.Font = Enum.Font.GothamBold
	AutoSteal.infoLabel.TextSize = 11
	AutoSteal.infoLabel.TextColor3 = Color3.fromRGB(220, 220, 255)
	AutoSteal.infoLabel.Text = "Asta Duels premium  •  Ping: 0ms  •  FPS: 0  •  discord.gg/rdUPtvhnV"
	AutoSteal.infoLabel.TextXAlignment = Enum.TextXAlignment.Left
	AutoSteal.infoLabel.TextYAlignment = Enum.TextYAlignment.Center
	AutoSteal.infoLabel.Parent = AutoSteal.bannerFrame

    -- Status indicator dot
	local statusDot = Instance.new("Frame")
	statusDot.Size = UDim2.new(0, 6, 0, 6)
	statusDot.Position = UDim2.new(0.7, 8, 0.5, - 3)
	statusDot.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
	statusDot.BorderSizePixel = 0
	statusDot.Parent = AutoSteal.bannerFrame
	local dotCorner = Instance.new("UICorner", statusDot)
	dotCorner.CornerRadius = UDim.new(1, 0)
	AutoSteal.statusDot = statusDot

    -- Progress Bar Container (Elegant)
	AutoSteal.progressBarBg = Instance.new("Frame")
	AutoSteal.progressBarBg.Size = UDim2.new(0.9, 0, 0, 12)
	AutoSteal.progressBarBg.Position = UDim2.new(0.05, 0, 0, 28)
	AutoSteal.progressBarBg.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
	AutoSteal.progressBarBg.BackgroundTransparency = 0.5
	AutoSteal.progressBarBg.Visible = true
	AutoSteal.progressBarBg.Parent = container
	local progCorner = Instance.new("UICorner", AutoSteal.progressBarBg)
	progCorner.CornerRadius = UDim.new(0, 6)

    -- Progress Fill (Gradient effect)
	AutoSteal.progressFill = Instance.new("Frame")
	AutoSteal.progressFill.Size = UDim2.new(0, 0, 1, 0)
	AutoSteal.progressFill.BackgroundColor3 = Color3.fromRGB(128, 0, 255)
	AutoSteal.progressFill.BackgroundTransparency = 0
	AutoSteal.progressFill.Parent = AutoSteal.progressBarBg
	local fillCorner = Instance.new("UICorner", AutoSteal.progressFill)
	fillCorner.CornerRadius = UDim.new(0, 6)

    -- Shimmer effect on progress fill
	local shimmer = Instance.new("ImageLabel")
	shimmer.Size = UDim2.new(1, 0, 1, 0)
	shimmer.Position = UDim2.new(0, 0, 0, 0)
	shimmer.BackgroundTransparency = 1
	shimmer.Image = "rbxassetid://5028857089"
	shimmer.ImageColor3 = Color3.fromRGB(200, 150, 255)
	shimmer.ImageTransparency = 0.7
	shimmer.Parent = AutoSteal.progressFill
	shimmer.ZIndex = 2

    -- Percentage Label (Clean, minimal)
	AutoSteal.percentLabel = Instance.new("TextLabel")
	AutoSteal.percentLabel.Size = UDim2.new(0.15, 0, 1, 0)
	AutoSteal.percentLabel.Position = UDim2.new(0.85, - 4, 0, 0)
	AutoSteal.percentLabel.BackgroundTransparency = 1
	AutoSteal.percentLabel.Font = Enum.Font.GothamBold
	AutoSteal.percentLabel.TextSize = 10
	AutoSteal.percentLabel.TextColor3 = Color3.fromRGB(200, 180, 255)
	AutoSteal.percentLabel.Text = "0%"
	AutoSteal.percentLabel.TextXAlignment = Enum.TextXAlignment.Center
	AutoSteal.percentLabel.TextYAlignment = Enum.TextYAlignment.Center
	AutoSteal.percentLabel.Parent = AutoSteal.progressBarBg

    -- Sparkle particles on progress
	local sparkles = Instance.new("ParticleEmitter")
	sparkles.Texture = "rbxassetid://15674905983"
	sparkles.Rate = 0
	sparkles.Lifetime = NumberRange.new(0.2, 0.5)
	sparkles.SpreadAngle = Vector2.new(180, 180)
	sparkles.VelocityInheritance = 0
	sparkles.Speed = NumberRange.new(0.5, 2)
	sparkles.Transparency = NumberSequence.new({
		NumberSequenceKeypoint.new(0, 0),
		NumberSequenceKeypoint.new(1, 0.5)
	})
	sparkles.Size = NumberSequence.new({
		NumberSequenceKeypoint.new(0, 0.5),
		NumberSequenceKeypoint.new(1, 0)
	})
	sparkles.Color = ColorSequence.new(Color3.fromRGB(200, 150, 255))
	sparkles.Parent = AutoSteal.progressFill
	AutoSteal.sparkles = sparkles

    -- FPS/Ping update loop
	task.spawn(function()
		local framesCount = 0
		local last = tick()
		while AutoSteal.infoLabel and AutoSteal.infoLabel.Parent do
			framesCount = framesCount + 1
			if tick() - last >= 1 then
				local fps = framesCount
				framesCount = 0
				last = tick()
				local ping = 0
				local network = Stats:FindFirstChild("Network")
				if network and network:FindFirstChild("ServerStatsItem") then
					local dataPing = network.ServerStatsItem:FindFirstChild("Data Ping")
					if dataPing then
						ping = math.floor(dataPing:GetValue())
					end
				end
				pcall(function()
					AutoSteal.infoLabel.Text = "Asta Duels premium  •  Ping: " .. ping .. "ms  •  FPS: " .. fps .. "  •  discord.gg/rdUPtvhnV"
				end)
			end
			task.wait()
		end
	end)

    -- Drag functionality
	local function onDragStart(input)
		if AutoSteal.uiLocked then
			return
		end
		if GlobalState and GlobalState.uiLocked then
			return
		end
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			AutoSteal.isDragging = true
			AutoSteal.dragStartPos = input.Position
			AutoSteal.startContainerPos = AutoSteal.container.Position
		end
	end
	local function onDragEnd(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			AutoSteal.isDragging = false
		end
	end
	local function onDragMove(input)
		if not AutoSteal.isDragging then
			return
		end
		if AutoSteal.uiLocked then
			return
		end
		if GlobalState and GlobalState.uiLocked then
			return
		end
		if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
			local delta = input.Position - AutoSteal.dragStartPos
			AutoSteal.container.Position = UDim2.new(
                AutoSteal.startContainerPos.X.Scale, AutoSteal.startContainerPos.X.Offset + delta.X, AutoSteal.startContainerPos.Y.Scale, AutoSteal.startContainerPos.Y.Offset + delta.Y)
		end
	end
	AutoSteal.container.InputBegan:Connect(onDragStart)
	UIS.InputEnded:Connect(onDragEnd)
	UIS.InputChanged:Connect(onDragMove)
end

local function syncIrishLockState(locked)
	AutoSteal.uiLocked = locked
	if AutoSteal.bannerFrame then
		AutoSteal.bannerFrame.BackgroundTransparency = locked and 0.1 or 0.3
	end
end

-- ============================================================
-- ELEGANT PROGRESS BAR UPDATE WITH ANIMATION
-- ============================================================
local function updateIrishProgressBar(p)
	if AutoSteal.progressFill then
        -- Smooth fill with easing
		AutoSteal.progressFill:TweenSize(
            UDim2.new(p, 0, 1, 0), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.06, true)
        -- Color shift: Purple to Pink gradient
		local r = 128 + (127 * p)
		local g = 0 + (50 * p)
		local b = 255 - (50 * p)
		AutoSteal.progressFill.BackgroundColor3 = Color3.fromRGB(
            math.floor(r), math.floor(g), math.floor(b))
        -- Sparkle rate based on progress
		if AutoSteal.sparkles then
			AutoSteal.sparkles.Rate = p > 0.1 and math.floor(p * 20) or 0
		end
	end
	if AutoSteal.percentLabel then
		AutoSteal.percentLabel.Text = math.floor(p * 100) .. "%"
        -- Color shift for percentage
		local r = 200 + (55 * p)
		local g = 180 + (75 * p)
		local b = 255 - (30 * p)
		AutoSteal.percentLabel.TextColor3 = Color3.fromRGB(
            math.floor(r), math.floor(g), math.floor(b))
	end
    -- Status dot pulsing
	if AutoSteal.statusDot then
		if p > 0 and p < 1 then
			AutoSteal.statusDot.BackgroundColor3 = Color3.fromRGB(255, 200, 50)
			AutoSteal.statusDot:TweenSize(UDim2.new(0, 8, 0, 8), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.1, true)
		elseif p >= 1 then
			AutoSteal.statusDot.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
			AutoSteal.statusDot:TweenSize(UDim2.new(0, 6, 0, 6), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.1, true)
		else
			AutoSteal.statusDot.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
			AutoSteal.statusDot:TweenSize(UDim2.new(0, 6, 0, 6), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.1, true)
		end
	end
end

local function getHRP()
	local c = LP.Character
	if c then
		return c:FindFirstChild("HumanoidRootPart") or c:FindFirstChild("Torso") or c:FindFirstChild("UpperTorso")
	end
	return nil
end

local function isMyPlotByName(pn)
	local ct = tick()
	if AutoSteal.plotCache[pn] and (ct - (AutoSteal.plotCacheTime[pn] or 0)) < PLOT_CACHE_DURATION then
		return AutoSteal.plotCache[pn]
	end
	local plots = Workspace:FindFirstChild("Plots")
	if not plots then
		AutoSteal.plotCache[pn] = false
		AutoSteal.plotCacheTime[pn] = ct
		return false
	end
	local plot = plots:FindFirstChild(pn)
	if not plot then
		AutoSteal.plotCache[pn] = false
		AutoSteal.plotCacheTime[pn] = ct
		return false
	end
	local sign = plot:FindFirstChild("PlotSign")
	if sign then
		local yb = sign:FindFirstChild("YourBase")
		if yb and yb:IsA("BillboardGui") then
			local r = yb.Enabled == true
			AutoSteal.plotCache[pn] = r
			AutoSteal.plotCacheTime[pn] = ct
			return r
		end
	end
	AutoSteal.plotCache[pn] = false
	AutoSteal.plotCacheTime[pn] = ct
	return false
end

local function findNearestPrompt()
	local hrp = getHRP()
	if not hrp then
		return nil
	end
	local ct = tick()
	if ct - AutoSteal.promptCacheTime < 0.15 and # AutoSteal.cachedPrompts > 0 then
		local np, nd = nil, math.huge
		for _, data in ipairs(AutoSteal.cachedPrompts) do
			if data.spawn then
				local dist = (data.spawn.Position - hrp.Position).Magnitude
				if dist <= AutoSteal.StealRadius and dist < nd then
					np = data.prompt
					nd = dist
				end
			end
		end
		if np then
			return np
		end
	end
	AutoSteal.cachedPrompts = {}
	AutoSteal.promptCacheTime = ct
	local plots = Workspace:FindFirstChild("Plots")
	if not plots then
		return nil
	end
	local nearest, dist = nil, math.huge
	for _, plot in ipairs(plots:GetChildren()) do
		if isMyPlotByName(plot.Name) then
			continue
		end
		local pods = plot:FindFirstChild("AnimalPodiums")
		if not pods then
			continue
		end
		for _, pod in ipairs(pods:GetChildren()) do
			local base = pod:FindFirstChild("Base")
			if not base then
				continue
			end
			local spawn = base:FindFirstChild("Spawn")
			if not spawn then
				continue
			end
			local d = (spawn.Position - hrp.Position).Magnitude
			if d <= AutoSteal.StealRadius and d < dist then
				local att = spawn:FindFirstChild("PromptAttachment")
				if att then
					for _, p in ipairs(att:GetChildren()) do
						if p:IsA("ProximityPrompt") and p.ActionText and p.ActionText:find("Steal") then
							nearest, dist = p, d
							table.insert(AutoSteal.cachedPrompts, {
								prompt = p,
								spawn = spawn
							})
							break
						end
					end
				end
			end
		end
	end
	return nearest
end

local function executeIrishSteal(prompt)
	if AutoSteal.isStealing then
		return
	end
	local ct = tick()
	if ct - AutoSteal.lastStealTick < STEAL_COOLDOWN then
		return
	end
	if not AutoSteal.StealData[prompt] then
		AutoSteal.StealData[prompt] = {
			hold = {},
			trigger = {},
			ready = true
		}
		if getconnections then
			for _, c in ipairs(getconnections(prompt.PromptButtonHoldBegan)) do
				if c.Function then
					table.insert(AutoSteal.StealData[prompt].hold, c.Function)
				end
			end
			for _, c in ipairs(getconnections(prompt.Triggered)) do
				if c.Function then
					table.insert(AutoSteal.StealData[prompt].trigger, c.Function)
				end
			end
		end
	end
	local data = AutoSteal.StealData[prompt]
	if not data.ready then
		return
	end
	data.ready = false
	AutoSteal.isStealing = true
	AutoSteal.lastStealTick = ct
	local startTime = tick()
	if AutoSteal.progressConn then
		AutoSteal.progressConn:Disconnect()
	end
	AutoSteal.progressConn = RunService.Heartbeat:Connect(function()
		if not AutoSteal.isStealing then
			AutoSteal.progressConn:Disconnect()
			updateIrishProgressBar(0)
			return
		end
		local elapsed = tick() - startTime
		local p = math.clamp(elapsed / AutoSteal.StealDuration, 0, 1)
		updateIrishProgressBar(p)
	end)
	task.spawn(function()
		for _, f in ipairs(data.hold) do
			pcall(f)
		end
		while tick() - startTime < AutoSteal.StealDuration do
			task.wait()
		end
		updateIrishProgressBar(1)
		for _, f in ipairs(data.trigger) do
			pcall(f)
		end
		task.wait(0.05)
		updateIrishProgressBar(0)
		data.ready = true
		AutoSteal.isStealing = false
		if AutoSteal.progressConn then
			AutoSteal.progressConn:Disconnect()
		end
	end)
end

local function startIrishAutoSteal()
	setupIrishUI()
	if AutoSteal.autoStealConn then
		return
	end
	AutoSteal.autoStealConn = RunService.Heartbeat:Connect(function()
		if not AutoSteal.AutoStealEnabled or AutoSteal.isStealing then
			return
		end
		local success, prompt = pcall(findNearestPrompt)
		if success and prompt then
			pcall(executeIrishSteal, prompt)
		end
	end)
end

local function stopIrishAutoSteal()
	if AutoSteal.autoStealConn then
		AutoSteal.autoStealConn:Disconnect()
		AutoSteal.autoStealConn = nil
	end
	if AutoSteal.progressConn then
		AutoSteal.progressConn:Disconnect()
		AutoSteal.progressConn = nil
	end
	AutoSteal.isStealing = false
	updateIrishProgressBar(0)
end

local function setAutoStealEnabled(enabled)
	AutoSteal.AutoStealEnabled = enabled
	if enabled then
		startIrishAutoSteal()
	else
		stopIrishAutoSteal()
	end
	pcall(saveConfig)
end

-- ============================================================
-- UPDATED AIMBOT (WITHOUT AFFECTING PLAYER ESP)
-- ============================================================
local aimbotActive = false
local aimbotConnection = nil
local currentTarget = nil

local AUTO_BAT_SPEED = 56.5
local AUTO_BAT_VERT_SPEED = 52
local AUTO_BAT_DIST = 2.5
local AUTO_BAT_HEIGHT = 0
local AUTO_BAT_V_OFF = 0
local AUTO_BAT_TURN_SPEED = 285
local AUTO_BAT_MAX_TURN_RATE = 28
local AUTO_SWING_ENABLED = true

-- AIMBOT ESP - RED (separate from Player ESP)
local aimbotHighlight = Instance.new("Highlight")
aimbotHighlight.Name = "AimbotESP"
aimbotHighlight.FillColor = Color3.fromRGB(255, 0, 0)  -- RED
aimbotHighlight.OutlineColor = Color3.fromRGB(255, 255, 255)
aimbotHighlight.FillTransparency = 0.5
aimbotHighlight.OutlineTransparency = 0
aimbotHighlight.Enabled = false
pcall(function()
	aimbotHighlight.Parent = game:GetService("CoreGui")
end)
if not aimbotHighlight.Parent then
	aimbotHighlight.Parent = LP:WaitForChild("PlayerGui")
end

local function getCharacter()
	return LP.Character
end
local function getHumanoid()
	local char = getCharacter();
	return char and char:FindFirstChildOfClass("Humanoid")
end
local function getRootPart()
	local char = getCharacter();
	return char and char:FindFirstChild("HumanoidRootPart")
end

local function findBat()
	local char = getCharacter()
	if not char then
		return nil
	end
	for _, tool in ipairs(char:GetChildren()) do
		if tool:IsA("Tool") and (tool.Name:lower():find("bat") or tool.Name:lower():find("slap")) then
			return tool
		end
	end
	local bp = LP:FindFirstChildOfClass("Backpack") or LP:FindFirstChild("Backpack")
	if bp then
		for _, tool in ipairs(bp:GetChildren()) do
			if tool:IsA("Tool") and (tool.Name:lower():find("bat") or tool.Name:lower():find("slap")) then
				return tool
			end
		end
	end
	return nil
end

local function ensureBatEquipped()
	local char = getCharacter()
	local hum = getHumanoid()
	if not char or not hum then
		return
	end
	if not char:FindFirstChildOfClass("Tool") then
		local bat = findBat()
		if bat then
			pcall(function()
				hum:EquipTool(bat)
			end)
		end
	end
end

local function swingBat()
	if not AUTO_SWING_ENABLED then
		return
	end
	local bat = findBat()
	if bat and bat.Parent == getCharacter() then
		pcall(function()
			bat:Activate()
		end)
	end
end

local function getClosestTarget()
	local root = getRootPart()
	if not root then
		return nil
	end
	local closest, minDist = nil, math.huge
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr ~= LP and plr.Character then
			local tRoot = plr.Character:FindFirstChild("HumanoidRootPart")
			local hum = plr.Character:FindFirstChildOfClass("Humanoid")
			if tRoot and hum and hum.Health > 0 then
				local dist = (tRoot.Position - root.Position).Magnitude
				if dist < minDist then
					minDist = dist
					closest = tRoot
				end
			end
		end
	end
	return closest
end

local function resetAutoBatMotion()
	local root = getRootPart()
	local hum = getHumanoid()
	if root then
		root.AssemblyLinearVelocity = root.AssemblyLinearVelocity * 0.3
		root.AssemblyAngularVelocity = Vector3.zero
	end
	if hum then
		hum.AutoRotate = true
	end
end

local function startAimbotLoop()
	if aimbotConnection then
		aimbotConnection:Disconnect()
	end
	aimbotConnection = RunService.Heartbeat:Connect(function()
		if not aimbotActive then
			return
		end
		local char = getCharacter()
		local hum = getHumanoid()
		local root = getRootPart()
		if not char or not hum or not root then
			return
		end
		ensureBatEquipped()
		local target = getClosestTarget()
		currentTarget = target
		if not target then
			if AUTO_SWING_ENABLED then
				swingBat()
			end
			aimbotHighlight.Adornee = nil
			aimbotHighlight.Enabled = false
			return
		end
		local targetVel = target.AssemblyLinearVelocity
		local aimTargetPos = target.Position + (targetVel * math.clamp(targetVel.Magnitude / 130, 0.05, 0.15)) + Vector3.new(0, AUTO_BAT_V_OFF, 0)
		hum.AutoRotate = false
		local look = aimTargetPos - root.Position
		local flatLook = Vector3.new(look.X, 0, look.Z)
		if look.Magnitude > 0.01 and flatLook.Magnitude > 0.01 then
			local targetYaw = math.deg(math.atan2(- flatLook.X, - flatLook.Z))
			local yawDelta = (targetYaw - root.Orientation.Y + 180) % 360 - 180
			local targetPitch = math.deg(math.atan2(look.Y, flatLook.Magnitude))
			local pitchDelta = (targetPitch - root.Orientation.X + 180) % 360 - 180
			local yawRate = math.clamp(math.rad(yawDelta) * AUTO_BAT_TURN_SPEED, - AUTO_BAT_MAX_TURN_RATE, AUTO_BAT_MAX_TURN_RATE)
			local pitchRate = math.clamp(math.rad(pitchDelta) * AUTO_BAT_TURN_SPEED, - AUTO_BAT_MAX_TURN_RATE, AUTO_BAT_MAX_TURN_RATE)
			local yawRad = math.rad(root.Orientation.Y)
			local rightAxis = Vector3.new(math.cos(yawRad), 0, - math.sin(yawRad))
			root.AssemblyAngularVelocity = Vector3.new(0, yawRate, 0) + (rightAxis * pitchRate)
		else
			root.AssemblyAngularVelocity = Vector3.zero
		end
		local dir = look.Magnitude > 0.01 and look.Unit or Vector3.zero
		local standPos = aimTargetPos - (dir * AUTO_BAT_DIST) + Vector3.new(0, AUTO_BAT_HEIGHT, 0)
		local moveDir = standPos - root.Position
		local hDir = Vector3.new(moveDir.X, 0, moveDir.Z)
		local hVel = hDir.Magnitude > 0.1 and hDir.Unit * AUTO_BAT_SPEED or Vector3.zero
		local vVel = math.abs(moveDir.Y) > 0.1 and Vector3.new(0, math.sign(moveDir.Y) * AUTO_BAT_VERT_SPEED, 0) or Vector3.new(0, - 2, 0)
		root.AssemblyLinearVelocity = hVel + vVel
		if hDir.Magnitude > 0.5 then
			hum:Move(hDir.Unit, false)
		end
		if AUTO_SWING_ENABLED and (root.Position - target.Position).Magnitude < 6 then
			local bat = findBat()
			if bat and bat:IsA("Tool") then
				pcall(function()
					bat:Activate()
				end)
			end
		end

        -- I-update ang aimbot highlight
		if target then
			aimbotHighlight.Adornee = target.Parent
			aimbotHighlight.Enabled = true
		end
	end)
end

-- FIXED: Hindi na tinatawag ang startESP() at stopESP()
local function enableAimbot()
	if aimbotActive then
		return
	end
	aimbotActive = true
	local hum = getHumanoid()
	if hum then
		hum.AutoRotate = false
	end
	startAimbotLoop()
end

local function disableAimbot()
	aimbotActive = false
	if aimbotConnection then
		aimbotConnection:Disconnect()
		aimbotConnection = nil
	end
	resetAutoBatMotion()
	aimbotHighlight.Adornee = nil
	aimbotHighlight.Enabled = false
end

local function setAimbotState(enabled)
	if enabled then
		enableAimbot()
	else
		disableAimbot()
	end
end

LP.CharacterAdded:Connect(function(char)
	if aimbotActive then
		task.wait(0.3)
		local hum = char:FindFirstChildOfClass("Humanoid")
		if hum then
			hum.AutoRotate = false
		end
		ensureBatEquipped()
	end
end)

-- ============================================================
-- UNWALK
-- ============================================================
local unwalkEnabled = false
local unwalkSavedAnimate = nil

local function startUnwalk()
	local c = LP.Character
	if not c then
		return
	end
	local hum = c:FindFirstChildOfClass("Humanoid")
	if hum then
		for _, t in ipairs(hum:GetPlayingAnimationTracks()) do
			t:Stop()
		end
	end
	local anim = c:FindFirstChild("Animate")
	if anim then
		unwalkSavedAnimate = anim:Clone()
		anim:Destroy()
	end
end

local function stopUnwalk()
	local c = LP.Character
	if c and unwalkSavedAnimate then
		unwalkSavedAnimate:Clone().Parent = c
		unwalkSavedAnimate = nil
	end
end

local function toggleUnwalk(on)
	unwalkEnabled = on
	if on then
		startUnwalk()
	else
		stopUnwalk()
	end
	pcall(saveConfig)
end

-- ============================================================
-- PERMANENT SPEED TEXT
-- ============================================================
local otherSpeedLabels = {}
local speedLabel = nil

local function setupSpeedIndicator(char, player)
	player = player or LP
	local head = char:FindFirstChild("Head") or char:WaitForChild("Head", 5)
	if not head then
		return
	end
	local old = head:FindFirstChild(player == LP and "AstaSpeedBB" or "AstaOtherSpeedBB")
	if old then
		old:Destroy()
	end
	local bb = Instance.new("BillboardGui", head)
	bb.Name = player == LP and "AstaSpeedBB" or "AstaOtherSpeedBB"
	bb.Size = UDim2.new(0, player == LP and 190 or 90, 0, player == LP and 54 or 30)
	bb.StudsOffset = Vector3.new(0, player == LP and 3.35 or 2.85, 0)
	bb.AlwaysOnTop = true
	if player == LP then
		local tag = Instance.new("TextLabel", bb)
		tag.Size = UDim2.new(1, 0, 0, 22)
		tag.Position = UDim2.new(0, 0, 0, 0)
		tag.BackgroundTransparency = 1
		tag.Text = "/Asta.Duels"
		tag.TextColor3 = Color3.fromRGB(255, 255, 255)
		tag.Font = Enum.Font.GothamBlack
		tag.TextSize = 15
		tag.TextXAlignment = Enum.TextXAlignment.Center
		tag.TextStrokeTransparency = 0.32
		tag.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
	end
	local val = Instance.new("TextLabel", bb)
	val.Size = UDim2.new(1, 0, 0, player == LP and 26 or 30)
	val.Position = UDim2.new(0, 0, 0, player == LP and 24 or 0)
	val.BackgroundTransparency = 1
	val.Text = player == LP and "Speed: 0.0" or "0"
	val.TextColor3 = player == LP and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(255, 255, 255)
	val.Font = Enum.Font.GothamBlack
	val.TextSize = player == LP and 17 or 22
	val.TextXAlignment = Enum.TextXAlignment.Center
	val.TextStrokeTransparency = 0.35
	val.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
	if player == LP then
		speedLabel = val
	else
		otherSpeedLabels[player] = val
	end
end

local function enableSpeedTextAlways()
	if LP.Character then
		setupSpeedIndicator(LP.Character)
	end
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr ~= LP and plr.Character then
			setupSpeedIndicator(plr.Character, plr)
		end
	end
end

LP.CharacterAdded:Connect(function(char)
	task.wait(0.5)
	setupSpeedIndicator(char)
end)

RunService.RenderStepped:Connect(function()
	if speedLabel and LP.Character then
		local hrp = LP.Character:FindFirstChild("HumanoidRootPart")
		if hrp then
			local actualSpeed = Vector3.new(hrp.AssemblyLinearVelocity.X, 0, hrp.AssemblyLinearVelocity.Z).Magnitude
			if actualSpeed < 0.05 then
				actualSpeed = 0
			end
			speedLabel.Text = string.format("Speed: %.1f", actualSpeed)
		end
	end
	for plr, lbl in pairs(otherSpeedLabels) do
		if not lbl or not lbl.Parent then
			otherSpeedLabels[plr] = nil
		else
			local c = plr.Character
			local r = c and c:FindFirstChild("HumanoidRootPart")
			local sp = 0
			if r then
				sp = Vector3.new(r.AssemblyLinearVelocity.X, 0, r.AssemblyLinearVelocity.Z).Magnitude
			end
			if sp < 0.05 then
				sp = 0
			end
			lbl.Text = tostring(math.floor(sp + 0.5))
		end
	end
end)

local function hookOtherSpeed(plr)
	if plr == LP then
		return
	end
	if plr.Character then
		task.spawn(function()
			setupSpeedIndicator(plr.Character, plr)
		end)
	end
	plr.CharacterAdded:Connect(function(char)
		task.wait(0.5)
		setupSpeedIndicator(char, plr)
	end)
	plr.CharacterRemoving:Connect(function()
		otherSpeedLabels[plr] = nil
	end)
end

Players.PlayerAdded:Connect(hookOtherSpeed)
Players.PlayerRemoving:Connect(function(plr)
	otherSpeedLabels[plr] = nil
end)
for _, plr in ipairs(Players:GetPlayers()) do
	hookOtherSpeed(plr)
end

-- ============================================================
-- BAT COUNTER
-- ============================================================
local batCounterEnabled = false
local batCounterDebounce = false
local batCounterConn = nil
local BAT_COUNTER_SLAP_LIST = {
	"Bat",
	"Slap",
	"Iron Slap",
	"Gold Slap",
	"Diamond Slap",
	"Emerald Slap",
	"Ruby Slap",
	"Dark Matter Slap",
	"Flame Slap",
	"Nuclear Slap",
	"Galaxy Slap",
	"Glitched Slap"
}

local function findBatForCounter()
	local c = LP.Character
	if not c then
		return nil
	end
	local bp = LP:FindFirstChildOfClass("Backpack")
	for _, name in ipairs(BAT_COUNTER_SLAP_LIST) do
		local t = c:FindFirstChild(name) or (bp and bp:FindFirstChild(name))
		if t then
			return t
		end
	end
	for _, ch in ipairs(c:GetChildren()) do
		if ch:IsA("Tool") and ch.Name:lower():find("bat") then
			return ch
		end
	end
	if bp then
		for _, ch in ipairs(bp:GetChildren()) do
			if ch:IsA("Tool") and ch.Name:lower():find("bat") then
				return ch
			end
		end
	end
	return nil
end

local function swingBatForCounter(bat, char)
	local hum2 = char:FindFirstChildOfClass("Humanoid")
	if bat.Parent ~= char then
		if hum2 then
			pcall(function()
				hum2:EquipTool(bat)
			end)
		end
		task.wait(0.05)
	end
	local remote = bat:FindFirstChildOfClass("RemoteEvent") or bat:FindFirstChildOfClass("RemoteFunction")
	if remote and remote:IsA("RemoteEvent") then
		pcall(function()
			remote:FireServer()
		end)
		task.wait(0.15)
		pcall(function()
			remote:FireServer()
		end)
	else
		pcall(function()
			bat:Activate()
		end)
		task.wait(0.15)
		pcall(function()
			bat:Activate()
		end)
	end
end

local function startBatCounter()
	if batCounterConn then
		return
	end
	batCounterConn = RunService.Heartbeat:Connect(function()
		if not batCounterEnabled then
			return
		end
		if batCounterDebounce then
			return
		end
		local char = LP.Character
		if not char then
			return
		end
		local hum2 = char:FindFirstChildOfClass("Humanoid")
		if not hum2 then
			return
		end
		local st = hum2:GetState()
		if st == Enum.HumanoidStateType.Physics or st == Enum.HumanoidStateType.Ragdoll or st == Enum.HumanoidStateType.FallingDown then
			batCounterDebounce = true
			task.spawn(function()
				local bat = findBatForCounter()
				if bat then
					swingBatForCounter(bat, char)
				end
				task.wait(0.5)
				batCounterDebounce = false
			end)
		end
	end)
end

local function stopBatCounter()
	if batCounterConn then
		batCounterConn:Disconnect()
		batCounterConn = nil
	end
	batCounterDebounce = false
end

local function toggleBatCounter(on)
	batCounterEnabled = on
	if on then
		startBatCounter()
	else
		stopBatCounter()
	end
	pcall(saveConfig)
end

-- ============================================================
-- STRETCH REZ
-- ============================================================
local stretchRezEnabled = false
local stretchRezConn = nil

local function enableStretchRez()
	stretchRezEnabled = true
	workspace.CurrentCamera.FieldOfView = 120
	if stretchRezConn then
		stretchRezConn:Disconnect()
	end
	stretchRezConn = RunService.RenderStepped:Connect(function()
		if not stretchRezEnabled then
			stretchRezConn:Disconnect()
			stretchRezConn = nil
			return
		end
		workspace.CurrentCamera.FieldOfView = 120
	end)
end

local function disableStretchRez()
	stretchRezEnabled = false
	if stretchRezConn then
		stretchRezConn:Disconnect()
		stretchRezConn = nil
	end
	workspace.CurrentCamera.FieldOfView = 70
end

local function toggleStretchRez(on)
	stretchRezEnabled = on
	if on then
		enableStretchRez()
	else
		disableStretchRez()
	end
	pcall(saveConfig)
end

-- ============================================================
-- HUB CREATION
-- ============================================================
function CreateHub()
-- ============================================================
-- BLACK/PURPLE THEME CONSTANTS
-- ============================================================
	local DROP_ASCEND_DURATION = 0.2
	local DROP_ASCEND_SPEED = 150
	local lockInEnabled = false
	local lockInHeartbeatConn = nil
	local originalAnims = nil
	local _isfile = isfile or (syn and syn.isfile) or (getgenv and getgenv().isfile) or function()
		return false
	end
	local _readfile = readfile or (syn and syn.readfile) or (getgenv and getgenv().readfile) or function()
		return nil
	end
	local _writefile = writefile or (syn and syn.writefile) or (getgenv and getgenv().writefile) or function()
	end
	local getconnections = getconnections or get_signal_cons or getconnects or (syn and syn.get_signal_cons)

-- STATE
	local State = {
		normalSpeed = 60,
		carrySpeed = 30,
		laggerSpeed = 10.1,
		laggerCarrySpeed = 20,
		speedToggled = false,
		laggerEnabled = false,
		laggerCarryEnabled = false,
		infJumpEnabled = false,
		jumpPower = 55,
		antiRagdollEnabled = false,
		fpsBoostEnabled = false,
		guiVisible = true,
		uiLocked = false,
		autoLeftEnabled = false,
		autoRightEnabled = false,
		autoLeftPhase = 1,
		autoRightPhase = 1,
		medusaLastUsed = 0,
		medusaDebounce = false,
		medusaCounterEnabled = false,
		batAimbotToggled = false,
		autoSwingEnabled = true,
		hittingCooldown = false,
		batCounterEnabled = false,
		batCounterDebounce = false,
		dropEnabled = false,
		_tpInProgress = false,
		lastMoveDir = Vector3.new(0, 0, 0),
		unwalkEnabled = false,
		stackButtonsHidden = false,
		_prevCarry = 30,
		_prevSpeed = false,
		_prevLaggerCarry = false,
		duelCountdownEnabled = false,
		_duelWaiting = false,
		savedButtonPositions = {},
		prcMode = "Default",
		aimbotSpeed = 58,
		autoTPDownEnabled = false,
		autoTPDownHeight = 20,
		autoTPDownConn = nil,
		autoBatEnabled = false,
		autoBatSpeed = 56.5,
		autoBatVertSpeed = 52,
		autoBatDist = 2.5,
		autoBatHeight = 0,
		autoBatVOff = 0,
		autoBatTurnSpeed = 285,
		autoBatMaxTurnRate = 28,
		autoStealEnabled = true,
		autoStealRadius = 60,
		autoStealDuration = 1.4,
		stretchRezEnabled = false,
		espEnabled = false,
		buttonSize = 1.0,
		infJumpMode = "manual",
		antiLagClean = false,
		optimizerEnabled = false,
		skyTheme = currentSkyTheme or "Off",
	}
	GlobalState = State
	local Keys = {
		speed = Enum.KeyCode.Q,
		guiHide = Enum.KeyCode.LeftControl,
		autoLeft = Enum.KeyCode.L,
		autoRight = Enum.KeyCode.R,
		lagger = Enum.KeyCode.Unknown,
		laggerCarry = Enum.KeyCode.Unknown,
		tpDown = Enum.KeyCode.T,
		drop = Enum.KeyCode.H,
		aimbot = Enum.KeyCode.F,
	}

-- AUTO TP DOWN FUNCTIONS
	local function doTpDown()
		pcall(function()
			local char = LP.Character
			if not char then
				return
			end
			local root = char:FindFirstChild("HumanoidRootPart")
			local hum = char:FindFirstChildOfClass("Humanoid")
			if not root or not hum then
				return
			end
			local ray = RaycastParams.new()
			ray.FilterDescendantsInstances = {
				char
			}
			ray.FilterType = Enum.RaycastFilterType.Exclude
			local res = workspace:Raycast(root.Position, Vector3.new(0, - 600, 0), ray)
			if res then
				root.AssemblyLinearVelocity = Vector3.zero
				root.AssemblyAngularVelocity = Vector3.zero
				local newY = res.Position.Y + (hum.HipHeight or 2) + (root.Size.Y / 2) + 0.1
				root.CFrame = CFrame.new(root.Position.X, newY, root.Position.Z)
				root.AssemblyLinearVelocity = Vector3.zero
			end
		end)
	end
	local function startAutoTPDownLoop()
		if State.autoTPDownConn then
			return
		end
		State.autoTPDownConn = RunService.Heartbeat:Connect(function()
			if not State.autoTPDownEnabled then
				return
			end
			local char = LP.Character
			if not char then
				return
			end
			local hrp = char:FindFirstChild("HumanoidRootPart")
			if not hrp or not hrp.Parent then
				return
			end
			local yPos = hrp.Position.Y
			local threshold = State.autoTPDownHeight
			if yPos >= threshold then
				hrp.CFrame = CFrame.new(hrp.Position.X, - 8.80, hrp.Position.Z)
				hrp.AssemblyLinearVelocity = Vector3.zero
			end
		end)
	end
	local function stopAutoTPDownLoop()
		if State.autoTPDownConn then
			State.autoTPDownConn:Disconnect()
			State.autoTPDownConn = nil
		end
	end
	local function setAutoTPDown(enabled)
		State.autoTPDownEnabled = enabled
		if enabled then
			startAutoTPDownLoop()
		else
			stopAutoTPDownLoop()
		end
		pcall(saveConfig)
	end
	local function setAutoTPDownHeight(height)
		if height and height >= 5 and height <= 500 then
			State.autoTPDownHeight = height
			pcall(saveConfig)
		end
	end

-- DEFAULT STACK BUTTON POSITIONS
	local BTN_W = 68
	local BTN_H = 56
	local BTN_GAP = 6
	local COLS = 2
	local stackDefs = {
		{
			key = "autoLeft",
			label = "AUTO\nLEFT"
		},
		{
			key = "autoRight",
			label = "AUTO\nRIGHT"
		},
		{
			key = "aimbot",
			label = "AIMBOT"
		},
		{
			key = "lagger",
			label = "LAGGER\nMODE"
		},
		{
			key = "laggerCarry",
			label = "LAGGER\nCARRY"
		},
		{
			key = "drop",
			label = "DROP\nBR"
		},
		{
			key = "tpDown",
			label = "TP\nDOWN"
		},
		{
			key = "carrySpeed",
			label = "CARRY\nSPEED"
		},
	}
	local GRID_W = COLS * (BTN_W + BTN_GAP) - BTN_GAP
	local GRID_H = math.ceil(# stackDefs / COLS) * (BTN_H + BTN_GAP) - BTN_GAP
	local function getDefaultStackPos(i)
		local col = (i - 1) % COLS
		local row2 = math.floor((i - 1) / COLS)
		return UDim2.new(1, - (GRID_W + 14) + col * (BTN_W + BTN_GAP), 0.5, - (GRID_H / 2) + row2 * (BTN_H + BTN_GAP))
	end
	local MOVE_KEYS = {
		[Enum.KeyCode.W] = true,
		[Enum.KeyCode.A] = true,
		[Enum.KeyCode.S] = true,
		[Enum.KeyCode.D] = true,
		[Enum.KeyCode.Up] = true,
		[Enum.KeyCode.Down] = true,
		[Enum.KeyCode.Left] = true,
		[Enum.KeyCode.Right] = true
	}
	local MEDUSA_COOLDOWN = 25
	local POS = {
		L1 = Vector3.new(- 476.48, - 6.28, 92.73),
		L2 = Vector3.new(- 483.12, - 4.95, 94.80),
		R1 = Vector3.new(- 476.16, - 6.52, 25.62),
		R2 = Vector3.new(- 483.04, - 5.09, 23.14),
	}
	local Conns = {
		autoSteal = nil,
		antiRag = nil,
		autoLeft = nil,
		autoRight = nil,
		aimbot = nil,
		anchor = {},
		progress = nil,
		batCounter = nil,
		unwalk = nil
	}
	local h, hrp
	local setAutoLeft, setAutoRight, setInfJump, setAntiRag, setFps
	local setMedusaCounter, setUnwalkToggle, setAimbot, setAutoSwing
	local setLagger, setDropBrainrot, setInstaGrabAutoSteal
	local setupMedusaCounter, stopMedusaCounter, startAntiRagdoll, stopAntiRagdoll
	local applyFPSBoost
	local startAutoLeft, stopAutoLeft, startAutoRight, stopAutoRight
	local saveConfig, loadConfig, runDropBrainrot, stopDropBrainrot
	local stackBtnRefs = {}
	local stackWrappers = {}
	local keybindBtnRefs = {}
	local normalBox, carryBox, laggerBox, laggerCarryBox, uiScaleBox, stealRadBoxAutoSteal, lockBtn
	local setHideButtonsToggle
	local autoTPDownHeightBox
	local autoStealRadiusBox, autoStealDurationBox, autoStealToggle
	local toggleSetters = {}
	local autoBatDistBox
	local btnSizeValLbl
	local modeBtns = {}
	local updateInfJumpModeUI = nil
	local jumpPowerBox = nil
	local antiLagToggle, optimizerToggle
	local skyThemeToggle = nil
	local skyThemeLabel = nil
	local skyThemeIndex = 1

-- Mutual exclusivity handler for speed modes
	local function enforceSpeedExclusivity(activeMode)
		if activeMode == "carry" then
			if State.laggerEnabled then
				State.laggerEnabled = false
				if stackBtnRefs.lagger then
					stackBtnRefs.lagger.setOn(false)
				end
			end
			if State.laggerCarryEnabled then
				State.laggerCarryEnabled = false
				if stackBtnRefs.laggerCarry then
					stackBtnRefs.laggerCarry.setOn(false)
				end
			end
			State.speedToggled = true
		elseif activeMode == "lagger" then
			if State.speedToggled then
				State.speedToggled = false
				if stackBtnRefs.carrySpeed then
					stackBtnRefs.carrySpeed.setOn(false)
				end
			end
			if State.laggerCarryEnabled then
				State.laggerCarryEnabled = false
				if stackBtnRefs.laggerCarry then
					stackBtnRefs.laggerCarry.setOn(false)
				end
			end
			State.laggerEnabled = true
		elseif activeMode == "laggerCarry" then
			if State.speedToggled then
				State.speedToggled = false
				if stackBtnRefs.carrySpeed then
					stackBtnRefs.carrySpeed.setOn(false)
				end
			end
			if State.laggerEnabled then
				State.laggerEnabled = false
				if stackBtnRefs.lagger then
					stackBtnRefs.lagger.setOn(false)
				end
			end
			State.laggerCarryEnabled = true
		else
			State.speedToggled = false
			State.laggerEnabled = false
			State.laggerCarryEnabled = false
			if stackBtnRefs.carrySpeed then
				stackBtnRefs.carrySpeed.setOn(false)
			end
			if stackBtnRefs.lagger then
				stackBtnRefs.lagger.setOn(false)
			end
			if stackBtnRefs.laggerCarry then
				stackBtnRefs.laggerCarry.setOn(false)
			end
		end
	end
	local function toggleSkyTheme()
		skyThemeIndex = skyThemeIndex % # CandySkyOrder + 1
		local label = CandySkyOrder[skyThemeIndex][2]
		currentSkyTheme = label
		State.skyTheme = label
		CandyApplyCustomSky(label)
		if skyThemeLabel then
			skyThemeLabel.Text = "Sky Theme: " .. label
		end
		pcall(saveConfig)
	end
	local function toggleNightMode(enabled)
		nightModeEnabled = enabled
		if enabled then
			CandyApplyCustomSky("Night")
			State.skyTheme = "Night"
		else
			CandyApplyCustomSky("Off")
			State.skyTheme = "Off"
		end
		pcall(saveConfig)
	end
	local function toggleESPFeature(enabled)
		State.espEnabled = enabled
		toggleESP(enabled)
	end
	local setStretchRezVisual = nil
	local function toggleStretchRezFeature(enabled)
		State.stretchRezEnabled = enabled
		toggleStretchRez(enabled)
		pcall(saveConfig)
	end

-- BLACK/PURPLE THEME COLORS
	local C = {
		winBg = Color3.fromRGB(0, 0, 0),
		winBorder = Color3.fromRGB(128, 0, 255),
		topBg = Color3.fromRGB(0, 0, 0),
		topTitle = Color3.fromRGB(255, 255, 255),
		topSub = Color3.fromRGB(180, 180, 180),
		topBtn = Color3.fromRGB(255, 255, 255),
		topBtnHov = Color3.fromRGB(50, 50, 50),
		topDivider = Color3.fromRGB(128, 0, 255),
		tabBarBg = Color3.fromRGB(0, 0, 0),
		tabBarDiv = Color3.fromRGB(128, 0, 255),
		tabIdle = Color3.fromRGB(150, 150, 150),
		tabActive = Color3.fromRGB(128, 0, 255),
		tabActiveBg = Color3.fromRGB(20, 20, 20),
		tabUnderline = Color3.fromRGB(128, 0, 255),
		sectionTxt = Color3.fromRGB(255, 255, 255),
		sectionDiv = Color3.fromRGB(128, 0, 255),
		rowBg = Color3.fromRGB(0, 0, 0),
		rowBorder = Color3.fromRGB(128, 0, 255),
		rowLabel = Color3.fromRGB(255, 255, 255),
		rowSub = Color3.fromRGB(180, 180, 180),
		rowValue = Color3.fromRGB(128, 0, 255),
		rowHov = Color3.fromRGB(20, 20, 20),
		inputBg = Color3.fromRGB(15, 15, 15),
		inputBorder = Color3.fromRGB(128, 0, 255),
		inputFocus = Color3.fromRGB(150, 150, 150),
		inputTxt = Color3.fromRGB(255, 255, 255),
		pillOff = Color3.fromRGB(40, 40, 40),
		pillOn = Color3.fromRGB(128, 0, 255),
		dotOff = Color3.fromRGB(80, 80, 80),
		dotOn = Color3.fromRGB(0, 0, 0),
		pillBorder = Color3.fromRGB(128, 0, 255),
		modeBtnBg = Color3.fromRGB(15, 15, 15),
		modeBtnBrd = Color3.fromRGB(128, 0, 255),
		modeBtnTxt = Color3.fromRGB(180, 180, 180),
		modeBtnActBg = Color3.fromRGB(128, 0, 255),
		modeBtnActTx = Color3.fromRGB(0, 0, 0),
		chipBg = Color3.fromRGB(20, 20, 20),
		chipBorder = Color3.fromRGB(128, 0, 255),
		chipTxt = Color3.fromRGB(255, 255, 255),
		btnBg = Color3.fromRGB(15, 15, 15),
		btnBorder = Color3.fromRGB(128, 0, 255),
		btnTxt = Color3.fromRGB(255, 255, 255),
		btnHov = Color3.fromRGB(80, 80, 80),
		stackBg = Color3.fromRGB(0, 0, 0),
		stackBrd = Color3.fromRGB(128, 0, 255),
		stackTxt = Color3.fromRGB(255, 255, 255),
		stackActBg = Color3.fromRGB(128, 0, 255),
		stackActBrd = Color3.fromRGB(128, 0, 255),
		stackActTxt = Color3.fromRGB(0, 0, 0),
		stackDot = Color3.fromRGB(80, 80, 80),
		stackDotOn = Color3.fromRGB(128, 0, 255),
		infoBg = Color3.fromRGB(0, 0, 0),
		infoBrd = Color3.fromRGB(50, 50, 50),
		infoTxt = Color3.fromRGB(180, 180, 180),
		infoVal = Color3.fromRGB(128, 0, 255),
		infoFill = Color3.fromRGB(128, 0, 255),
		accent = Color3.fromRGB(128, 0, 255),
		accentDim = Color3.fromRGB(80, 80, 80),
		lockOn = Color3.fromRGB(128, 0, 255),
		divider = Color3.fromRGB(50, 50, 50),
	}

-- PRC PERFORMANCE MODES
	local function applyPRCMode(mode)
		State.prcMode = mode
		if mode == "Default" then
			pcall(function()
				setfpscap(60)
			end)
			pcall(function()
				local L = game:GetService("Lighting")
				L.GlobalShadows = true
				L.FogEnd = 100000
				L.Brightness = 1
			end)
		elseif mode == "Normal" then
			pcall(function()
				setfpscap(120)
			end)
			pcall(function()
				local L = game:GetService("Lighting")
				L.GlobalShadows = false
				L.FogEnd = 9e9
			end)
		elseif mode == "High" then
			pcall(function()
				setfpscap(999999999)
			end)
			applyFPSBoost()
		end
	end

-- CLEANUP
	for _, name in pairs({
		"VyseSlottedGUI",
		"VyseAsireGUI",
		"VyseAsireHubV4",
		"VyseAsireHubV5",
		"VyseAsireHubV5_1",
		"VoidHubV5_1",
		"AsireHubV5_2",
		"SOURCEHUBV5_2",
		"J hub ",
		"AstaAutoStealGui",
		"SEVENTYHUBV7",
		"SeventyAutoStealGui"
	}) do
		pcall(function()
			local o = game:GetService("CoreGui"):FindFirstChild(name);
			if o then
				o:Destroy()
			end
		end)
		pcall(function()
			local o = LP:WaitForChild("PlayerGui"):FindFirstChild(name);
			if o then
				o:Destroy()
			end
		end)
	end

-- ROOT GUI
	local gui = Instance.new("ScreenGui")
	gui.Name = "AstaDuelsV7"
	gui.ResetOnSpawn = false
	gui.DisplayOrder = 10
	gui.IgnoreGuiInset = true
	gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	gui.Parent = LP:WaitForChild("PlayerGui")
	local uiScaleObj = Instance.new("UIScale", gui)
	uiScaleObj.Scale = 0.9
	local function applyUIScale(scale)
		if uiScaleObj then
			uiScaleObj.Scale = scale
		end
		local stealGui = LP.PlayerGui:FindFirstChild("AstaAutoStealGui")
		if stealGui then
			local existingScale = stealGui:FindFirstChild("UIScale")
			if existingScale then
				existingScale.Scale = scale
			else
				local newScale = Instance.new("UIScale", stealGui)
				newScale.Scale = scale
			end
		end
	end
	local function mkCorner(p, r)
		local c = Instance.new("UICorner", p)
		c.CornerRadius = UDim.new(0, r or 6)
		return c
	end
	local function mkStroke(p, col, th)
		local s = Instance.new("UIStroke", p)
		s.Color = col
		s.Thickness = th or 1
		s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
		return s
	end

-- DRAG FUNCTIONS
	local function makeDraggable(frame, handle)
		local src = handle or frame
		local dragging, dragInput, dragStart, startPos = false, nil, nil, nil
		src.InputBegan:Connect(function(inp)
			if State.uiLocked then
				return
			end
			if inp.UserInputType == Enum.UserInputType.MouseButton1 or inp.UserInputType == Enum.UserInputType.Touch then
				dragging = true
				dragStart = inp.Position
				startPos = frame.Position
				inp.Changed:Connect(function()
					if inp.UserInputState == Enum.UserInputState.End then
						dragging = false
					end
				end)
			end
		end)
		src.InputChanged:Connect(function(inp)
			if inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch then
				dragInput = inp
			end
		end)
		UIS.InputChanged:Connect(function(inp)
			if inp == dragInput and dragging and not State.uiLocked then
				local dx = inp.Position.X - dragStart.X
				local dy = inp.Position.Y - dragStart.Y
				frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + dx, startPos.Y.Scale, startPos.Y.Offset + dy)
			end
		end)
	end

-- STACK DRAGGABLE
	local function makeStackDraggable(frame, onTap, buttonKey)
		local dragging = false
		local dragStart = nil
		local startPos = nil
		local moved = false
		local MOVE_THRESHOLD = 10
		frame.InputBegan:Connect(function(inp)
			if inp.UserInputType ~= Enum.UserInputType.MouseButton1 and inp.UserInputType ~= Enum.UserInputType.Touch then
				return
			end
			dragging = true
			moved = false
			dragStart = inp.Position
			startPos = frame.Position
		end)
		frame.InputChanged:Connect(function(inp)
			if not dragging then
				return
			end
			if inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch then
				local dx = inp.Position.X - dragStart.X
				local dy = inp.Position.Y - dragStart.Y
				if math.abs(dx) > MOVE_THRESHOLD or math.abs(dy) > MOVE_THRESHOLD then
					moved = true
					if not State.uiLocked then
						frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + dx, startPos.Y.Scale, startPos.Y.Offset + dy)
					end
				end
			end
		end)
		frame.InputEnded:Connect(function(inp)
			if inp.UserInputType ~= Enum.UserInputType.MouseButton1 and inp.UserInputType ~= Enum.UserInputType.Touch then
				return
			end
			if dragging then
				if not moved and onTap then
					onTap()
				elseif moved and buttonKey then
					State.savedButtonPositions[buttonKey] = {
						X = frame.Position.X.Offset,
						Y = frame.Position.Y.Offset
					}
					pcall(saveButtonPositions)
				end
				dragging = false
				moved = false
			end
		end)
	end

-- SAVE/LOAD BUTTON POSITIONS
	local BUTTON_POS_FILE = "AstaDuelsButtonPos.json"
	local function saveButtonPositions()
		local ok, encoded = pcall(function()
			return HttpService:JSONEncode(State.savedButtonPositions)
		end)
		if ok then
			pcall(function()
				_writefile(BUTTON_POS_FILE, encoded)
			end)
		end
	end
	local function loadButtonPositions()
		local hasFile = false
		pcall(function()
			hasFile = _isfile(BUTTON_POS_FILE)
		end)
		if not hasFile then
			return
		end
		local raw
		pcall(function()
			raw = _readfile(BUTTON_POS_FILE)
		end)
		if not raw then
			return
		end
		local ok, decoded = pcall(function()
			return HttpService:JSONDecode(raw)
		end)
		if ok and decoded then
			State.savedButtonPositions = decoded
			for i, def in ipairs(stackDefs) do
				local wrapper = stackWrappers[def.key]
				if wrapper and State.savedButtonPositions[def.key] then
					local pos = State.savedButtonPositions[def.key]
					wrapper.Position = UDim2.new(1, pos.X, 0.5, pos.Y)
				end
			end
		end
	end
	local function applyStackButtonsVisible(visible)
		State.stackButtonsHidden = not visible
		for _, wrapper in pairs(stackWrappers) do
			if wrapper then
				wrapper.Visible = visible
			end
		end
	end

-- MAIN WINDOW
	local WIN_W = 290
	local WIN_H = 440
	local TITLE_H = 32
	local mainOuter = Instance.new("Frame", gui)
	mainOuter.Name = "MainOuter"
	mainOuter.Size = UDim2.new(0, WIN_W, 0, WIN_H)
	local centeredPos = UDim2.new(0.5, - WIN_W / 2, 0.5, - WIN_H / 2)
	mainOuter.Position = centeredPos
	mainOuter.BackgroundTransparency = 1
	mainOuter.BorderSizePixel = 0
	mainOuter.ClipsDescendants = true
	mainOuter.Visible = false
	mkCorner(mainOuter, 14)
	mkStroke(mainOuter, C.winBorder, 1)
	makeDraggable(mainOuter)

-- BACKGROUND IMAGE - ITO LANG ANG NATITIRA NA BACKGROUND IMAGE
	local bgImage = Instance.new("ImageLabel", mainOuter)
	bgImage.Size = UDim2.new(1, 0, 1, 0)
	bgImage.BackgroundTransparency = 1
	bgImage.Image = "rbxassetid://94542092135077"
	bgImage.ScaleType = Enum.ScaleType.Crop
	bgImage.ZIndex = 0
	local bgCorner = Instance.new("UICorner", bgImage)
	bgCorner.CornerRadius = UDim.new(0, 14)

-- TITLE BAR
	local titleBar = Instance.new("Frame", mainOuter)
	titleBar.Size = UDim2.new(1, 0, 0, TITLE_H)
	titleBar.Position = UDim2.new(0, 0, 0, 0)
	titleBar.BackgroundColor3 = C.topBg
	titleBar.BackgroundTransparency = 0.3
	titleBar.BorderSizePixel = 0
	titleBar.ZIndex = 5
	local titleCorner = Instance.new("UICorner", titleBar)
	titleCorner.CornerRadius = UDim.new(0, 14)
	local logoWhite = Instance.new("Frame", titleBar)
	logoWhite.Size = UDim2.new(0, 22, 0, 22)
	logoWhite.Position = UDim2.new(0, 10, 0.5, - 11)
	logoWhite.BackgroundColor3 = C.accent
	logoWhite.BorderSizePixel = 0
	local logoCorner = Instance.new("UICorner", logoWhite)
	logoCorner.CornerRadius = UDim.new(0, 6)
	local titleLbl = Instance.new("TextLabel", titleBar)
	titleLbl.Size = UDim2.new(0, 135, 1, 0)
	titleLbl.Position = UDim2.new(0, 38, 0, 0)
	titleLbl.BackgroundTransparency = 1
	titleLbl.Text = "Asta Duels premium"
	titleLbl.TextColor3 = Color3.fromRGB(255, 255, 255)
	titleLbl.Font = Enum.Font.GothamBlack
	titleLbl.TextSize = 13
	titleLbl.TextXAlignment = Enum.TextXAlignment.Left
	titleLbl.TextStrokeTransparency = 1
	titleLbl.ZIndex = 6
	local closeBtn = Instance.new("TextButton", titleBar)
	closeBtn.Size = UDim2.new(0, 22, 0, 22)
	closeBtn.Position = UDim2.new(1, - 30, 0.5, - 11)
	closeBtn.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
	closeBtn.BackgroundTransparency = 0.3
	closeBtn.BorderSizePixel = 0
	closeBtn.Text = "X"
	closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	closeBtn.Font = Enum.Font.GothamBlack
	closeBtn.TextSize = 15
	closeBtn.ZIndex = 7
	mkCorner(closeBtn, 5)
	mkStroke(closeBtn, C.chipBorder, 1)
	closeBtn.AutoButtonColor = false
	closeBtn.MouseEnter:Connect(function()
		TweenService:Create(closeBtn, TweenInfo.new(0.1), {
			TextColor3 = Color3.fromRGB(0, 0, 0),
			BackgroundColor3 = C.btnHov
		}):Play()
	end)
	closeBtn.MouseLeave:Connect(function()
		TweenService:Create(closeBtn, TweenInfo.new(0.1), {
			TextColor3 = Color3.fromRGB(255, 255, 255),
			BackgroundColor3 = Color3.fromRGB(15, 15, 15)
		}):Play()
	end)
	closeBtn.MouseButton1Click:Connect(function()
		if mainOuter.Visible then
			mainOuter.Visible = false
			State.guiVisible = false
		end
	end)
	lockBtn = Instance.new("TextButton", titleBar)
	lockBtn.Size = UDim2.new(0, 22, 0, 22)
	lockBtn.Position = UDim2.new(1, - 56, 0.5, - 11)
	lockBtn.BackgroundColor3 = C.modeBtnBg
	lockBtn.BackgroundTransparency = 0.3
	lockBtn.BorderSizePixel = 0
	lockBtn.Text = "ðŸ”“"
	lockBtn.TextColor3 = C.topBtn
	lockBtn.Font = Enum.Font.GothamBold
	lockBtn.TextSize = 11
	lockBtn.ZIndex = 7
	lockBtn.AutoButtonColor = false
	mkCorner(lockBtn, 5)
	mkStroke(lockBtn, C.chipBorder, 1)
	lockBtn.MouseButton1Click:Connect(function()
		State.uiLocked = not State.uiLocked
		lockBtn.Text = State.uiLocked and "ðŸ”’" or "ðŸ”“"
		pcall(function()
			if lockUISync then
				lockUISync(State.uiLocked)
			end
		end)
		pcall(function()
			syncIrishLockState(State.uiLocked)
		end)
		pcall(saveConfig)
	end)
	local titleDiv = Instance.new("Frame", mainOuter)
	titleDiv.Size = UDim2.new(1, 0, 0, 1)
	titleDiv.Position = UDim2.new(0, 0, 0, TITLE_H)
	titleDiv.BackgroundColor3 = C.topDivider
	titleDiv.BorderSizePixel = 0
	titleDiv.ZIndex = 5

-- SIDEBAR
	local SIDEBAR_W = 95
	local tabBar = Instance.new("Frame", mainOuter)
	tabBar.Size = UDim2.new(0, SIDEBAR_W, 1, - TITLE_H)
	tabBar.Position = UDim2.new(0, 0, 0, TITLE_H)
	tabBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
	tabBar.BackgroundTransparency = 0.4
	tabBar.BorderSizePixel = 0
	tabBar.ZIndex = 5
	local tabCorner = Instance.new("UICorner", tabBar)
	tabCorner.CornerRadius = UDim.new(0, 14)
	local tabBarLL = Instance.new("UIListLayout", tabBar)
	tabBarLL.FillDirection = Enum.FillDirection.Vertical
	tabBarLL.SortOrder = Enum.SortOrder.LayoutOrder
	tabBarLL.Padding = UDim.new(0, 4)
	local sideLine = Instance.new("Frame", mainOuter)
	sideLine.Size = UDim2.new(0, 1, 1, - TITLE_H)
	sideLine.Position = UDim2.new(0, SIDEBAR_W, 0, TITLE_H)
	sideLine.BackgroundColor3 = C.tabBarDiv
	sideLine.BorderSizePixel = 0
	sideLine.ZIndex = 5

-- Content background
	local contentBg = Instance.new("Frame", mainOuter)
	contentBg.Size = UDim2.new(1, - SIDEBAR_W, 1, - TITLE_H)
	contentBg.Position = UDim2.new(0, SIDEBAR_W, 0, TITLE_H)
	contentBg.BackgroundColor3 = C.winBg
	contentBg.BackgroundTransparency = 0.5
	contentBg.BorderSizePixel = 0
	contentBg.ClipsDescendants = true
	contentBg.ZIndex = 2
	local contentCorner = Instance.new("UICorner", contentBg)
	contentCorner.CornerRadius = UDim.new(0, 14)

-- TABS
	local TABS = {
		"Speed",
		"Aimbot",
		"Mechanics",
		"Key Page",
		"Movement",
		"Settings"
	}
	local currentTab = "Speed"
	local tabBtns = {}
	local tabPages = {}
	for i, name in ipairs(TABS) do
		local btn = Instance.new("TextButton", tabBar)
		btn.Size = UDim2.new(1, - 8, 0, 40)
		btn.BackgroundColor3 = (name == currentTab) and C.tabActiveBg or C.tabBarBg
		btn.BackgroundTransparency = (name == currentTab) and 0.4 or 0.5
		btn.BorderSizePixel = 0
		btn.Text = name
		btn.TextColor3 = (name == currentTab) and C.tabActive or C.tabIdle
		btn.Font = Enum.Font.GothamBold
		btn.TextSize = 11
		btn.ZIndex = 6
		btn.LayoutOrder = i
		btn.AutoButtonColor = false
		mkCorner(btn, 8)
		local underline = Instance.new("Frame", btn)
		underline.Size = UDim2.new(0.7, 0, 0, 2)
		underline.Position = UDim2.new(0.15, 0, 1, - 2)
		underline.BackgroundColor3 = C.tabUnderline
		underline.BorderSizePixel = 0
		underline.Visible = (name == currentTab)
		underline.ZIndex = 7
		tabBtns[name] = {
			btn = btn,
			underline = underline
		}
		btn.MouseEnter:Connect(function()
			if name ~= currentTab then
				TweenService:Create(btn, TweenInfo.new(0.1), {
					TextColor3 = Color3.fromRGB(255, 255, 255)
				}):Play()
			end
		end)
		btn.MouseLeave:Connect(function()
			if name ~= currentTab then
				TweenService:Create(btn, TweenInfo.new(0.1), {
					TextColor3 = C.tabIdle
				}):Play()
			end
		end)
		btn.MouseButton1Click:Connect(function()
			currentTab = name
			for _, n in ipairs(TABS) do
				local t = tabBtns[n]
				local active = (n == name)
				TweenService:Create(t.btn, TweenInfo.new(0.14), {
					TextColor3 = active and C.tabActive or C.tabIdle,
					BackgroundColor3 = active and C.tabActiveBg or C.tabBarBg,
					BackgroundTransparency = active and 0.4 or 0.5
				}):Play()
				t.underline.Visible = active
				if tabPages[n] then
					tabPages[n].Visible = active
				end
			end
		end)
	end

-- ROW BUILDERS
	local currentPage = nil
	local lo = 0
	local function LO()
		lo = lo + 1
		return lo
	end
	local function makeGap(px)
		local f = Instance.new("Frame", currentPage)
		f.Size = UDim2.new(1, 0, 0, px or 6)
		f.BackgroundTransparency = 1
		f.BorderSizePixel = 0
		f.LayoutOrder = LO()
	end
	local function makeSectionHeader(label)
		local wrap = Instance.new("Frame", currentPage)
		wrap.Size = UDim2.new(1, 0, 0, 28)
		wrap.BackgroundTransparency = 1
		wrap.BorderSizePixel = 0
		wrap.LayoutOrder = LO()
		local lbl = Instance.new("TextLabel", wrap)
		lbl.Size = UDim2.new(1, - 24, 1, 0)
		lbl.Position = UDim2.new(0, 12, 0, 0)
		lbl.BackgroundTransparency = 1
		lbl.Text = label and label:upper() or ""
		lbl.TextColor3 = C.sectionTxt
		lbl.Font = Enum.Font.GothamBold
		lbl.TextSize = 10
		lbl.TextXAlignment = Enum.TextXAlignment.Left
	end
	local function makeInputRow(label, default, onChange)
		local row = Instance.new("Frame", currentPage)
		row.Size = UDim2.new(1, 0, 0, 44)
		row.BackgroundColor3 = C.rowBg
		row.BackgroundTransparency = 0.5
		row.BorderSizePixel = 0
		row.LayoutOrder = LO()
		mkCorner(row, 8)
		local div = Instance.new("Frame", row)
		div.Size = UDim2.new(1, - 24, 0, 1)
		div.Position = UDim2.new(0, 12, 1, - 1)
		div.BackgroundColor3 = C.rowBorder
		div.BorderSizePixel = 0
		local lbl = Instance.new("TextLabel", row)
		lbl.Size = UDim2.new(1, - 90, 1, 0)
		lbl.Position = UDim2.new(0, 12, 0, 0)
		lbl.BackgroundTransparency = 1
		lbl.Text = label
		lbl.TextColor3 = C.rowLabel
		lbl.Font = Enum.Font.GothamBold
		lbl.TextSize = 13
		lbl.TextXAlignment = Enum.TextXAlignment.Left
		local boxWrap = Instance.new("Frame", row)
		boxWrap.Size = UDim2.new(0, 70, 0, 28)
		boxWrap.Position = UDim2.new(1, - 82, 0.5, - 14)
		boxWrap.BackgroundColor3 = C.inputBg
		boxWrap.BackgroundTransparency = 0.4
		boxWrap.BorderSizePixel = 0
		mkCorner(boxWrap, 5)
		local bs = mkStroke(boxWrap, C.inputBorder, 1)
		local box = Instance.new("TextBox", boxWrap)
		box.Size = UDim2.new(1, - 8, 1, 0)
		box.Position = UDim2.new(0, 4, 0, 0)
		box.BackgroundTransparency = 1
		box.Text = tostring(default)
		box.TextColor3 = C.inputTxt
		box.Font = Enum.Font.GothamBold
		box.TextSize = 13
		box.ClearTextOnFocus = false
		box.ZIndex = 8
		box.TextXAlignment = Enum.TextXAlignment.Center
		box.Focused:Connect(function()
			TweenService:Create(bs, TweenInfo.new(0.15), {
				Color = C.inputFocus
			}):Play()
		end)
		box.FocusLost:Connect(function()
			TweenService:Create(bs, TweenInfo.new(0.15), {
				Color = C.inputBorder
			}):Play()
			if onChange then
				local n = tonumber(box.Text)
				if n then
					onChange(n)
				else
					box.Text = tostring(default)
				end
			end
		end)
		return box, row
	end
	local function makeToggleRow(label, defaultOn, onToggle)
		local row = Instance.new("Frame", currentPage)
		row.Size = UDim2.new(1, 0, 0, 44)
		row.BackgroundTransparency = 0.5
		row.BorderSizePixel = 0
		row.LayoutOrder = LO()
		row.BackgroundColor3 = C.rowBg
		mkCorner(row, 8)
		local div = Instance.new("Frame", row)
		div.Size = UDim2.new(1, - 24, 0, 1)
		div.Position = UDim2.new(0, 12, 1, - 1)
		div.BackgroundColor3 = C.rowBorder
		div.BorderSizePixel = 0
		local lbl = Instance.new("TextLabel", row)
		lbl.Size = UDim2.new(1, - 70, 1, 0)
		lbl.Position = UDim2.new(0, 12, 0, 0)
		lbl.BackgroundTransparency = 1
		lbl.Text = label
		lbl.TextColor3 = C.rowLabel
		lbl.Font = Enum.Font.GothamBold
		lbl.TextSize = 13
		lbl.TextXAlignment = Enum.TextXAlignment.Left
		local pillBg = Instance.new("Frame", row)
		pillBg.Size = UDim2.new(0, 42, 0, 20)
		pillBg.Position = UDim2.new(1, - 54, 0.5, - 10)
		pillBg.BackgroundColor3 = defaultOn and C.pillOn or C.pillOff
		pillBg.BackgroundTransparency = 0.3
		pillBg.BorderSizePixel = 0
		pillBg.ZIndex = 7
		mkCorner(pillBg, 10)
		mkStroke(pillBg, C.pillBorder, 1)
		local dot = Instance.new("Frame", pillBg)
		dot.Size = UDim2.new(0, 14, 0, 14)
		dot.Position = defaultOn and UDim2.new(1, - 17, 0.5, - 7) or UDim2.new(0, 3, 0.5, - 7)
		dot.BackgroundColor3 = defaultOn and C.dotOn or C.dotOff
		dot.BorderSizePixel = 0
		dot.ZIndex = 8
		mkCorner(dot, 7)
		local isOn = defaultOn or false
		local function setV(on)
			isOn = on
			TweenService:Create(pillBg, TweenInfo.new(0.18, Enum.EasingStyle.Quad), {
				BackgroundColor3 = on and C.pillOn or C.pillOff
			}):Play()
			TweenService:Create(dot, TweenInfo.new(0.18, Enum.EasingStyle.Back), {
				Position = on and UDim2.new(1, - 17, 0.5, - 7) or UDim2.new(0, 3, 0.5, - 7),
				BackgroundColor3 = on and C.dotOn or C.dotOff
			}):Play()
		end
		local function toggle()
			isOn = not isOn
			setV(isOn)
			if onToggle then
				pcall(onToggle, isOn)
			end
		end
		local clk = Instance.new("TextButton", row)
		clk.Size = UDim2.new(1, - 60, 1, 0)
		clk.BackgroundTransparency = 1
		clk.Text = ""
		clk.ZIndex = 5
		clk.BorderSizePixel = 0
		clk.AutoButtonColor = false
		clk.MouseButton1Click:Connect(toggle)
		local pClk = Instance.new("TextButton", pillBg)
		pClk.Size = UDim2.new(1, 0, 1, 0)
		pClk.BackgroundTransparency = 1
		pClk.Text = ""
		pClk.ZIndex = 9
		pClk.BorderSizePixel = 0
		pClk.AutoButtonColor = false
		pClk.MouseButton1Click:Connect(toggle)
		return setV
	end

-- KEYBIND ROW
	local function getKeyDisplayName(kc)
		local n = kc.Name
		local gpNames = {
			ButtonA = "A",
			ButtonB = "B",
			ButtonX = "X",
			ButtonY = "Y",
			ButtonL1 = "LB",
			ButtonL2 = "LT",
			ButtonL3 = "LS",
			ButtonR1 = "RB",
			ButtonR2 = "RT",
			ButtonR3 = "RS",
			ButtonSelect = "SEL",
			ButtonStart = "STA",
			DPadUp = "Dâ†‘",
			DPadDown = "Dâ†“",
			DPadLeft = "Dâ†",
			DPadRight = "Dâ†’",
			Thumbstick1 = "LS",
			Thumbstick2 = "RS",
		}
		if gpNames[n] then
			return gpNames[n]
		end
		return n:sub(1, 5)
	end
	local function makeKeybindRow(label, currentKey, onChanged, keyName)
		local row = Instance.new("Frame", currentPage)
		row.Size = UDim2.new(1, 0, 0, 44)
		row.BackgroundTransparency = 0.5
		row.BorderSizePixel = 0
		row.LayoutOrder = LO()
		row.BackgroundColor3 = C.rowBg
		mkCorner(row, 8)
		local div = Instance.new("Frame", row)
		div.Size = UDim2.new(1, - 24, 0, 1)
		div.Position = UDim2.new(0, 12, 1, - 1)
		div.BackgroundColor3 = C.rowBorder
		div.BorderSizePixel = 0
		local lbl = Instance.new("TextLabel", row)
		lbl.Size = UDim2.new(1, - 75, 1, 0)
		lbl.Position = UDim2.new(0, 12, 0, 0)
		lbl.BackgroundTransparency = 1
		lbl.Text = label
		lbl.TextColor3 = C.rowLabel
		lbl.Font = Enum.Font.GothamBold
		lbl.TextSize = 13
		lbl.TextXAlignment = Enum.TextXAlignment.Left
		local kbtn = Instance.new("TextButton", row)
		kbtn.Size = UDim2.new(0, 52, 0, 28)
		kbtn.Position = UDim2.new(1, - 64, 0.5, - 14)
		kbtn.BackgroundColor3 = C.chipBg
		kbtn.BackgroundTransparency = 0.4
		kbtn.BorderSizePixel = 0
		kbtn.Text = getKeyDisplayName(currentKey)
		kbtn.TextColor3 = C.chipTxt
		kbtn.Font = Enum.Font.GothamBold
		kbtn.TextSize = 11
		kbtn.ZIndex = 8
		kbtn.AutoButtonColor = false
		mkCorner(kbtn, 5)
		local ks = mkStroke(kbtn, C.chipBorder, 1)
		local listening = false
		local lconnKeyboard = nil
		local lconnGamepad = nil
		local function stopL(key)
			listening = false
			if lconnKeyboard then
				lconnKeyboard:Disconnect()
				lconnKeyboard = nil
			end
			if lconnGamepad then
				lconnGamepad:Disconnect()
				lconnGamepad = nil
			end
			TweenService:Create(ks, TweenInfo.new(0.12), {
				Color = C.chipBorder
			}):Play()
			kbtn.TextColor3 = C.chipTxt
			if key then
				kbtn.Text = getKeyDisplayName(key)
				if onChanged then
					onChanged(key)
				end
				task.spawn(function()
					if saveConfig then
						pcall(saveConfig)
					end
				end)
			end
		end
		kbtn.MouseButton1Click:Connect(function()
			if listening then
				stopL(nil)
				return
			end
			listening = true
			kbtn.Text = "..."
			kbtn.TextColor3 = C.inputTxt
			TweenService:Create(ks, TweenInfo.new(0.12), {
				Color = C.inputFocus
			}):Play()
			lconnKeyboard = UIS.InputBegan:Connect(function(inp)
				if not listening then
					return
				end
				if inp.UserInputType ~= Enum.UserInputType.Keyboard then
					return
				end
				if inp.KeyCode == Enum.KeyCode.Escape then
					stopL(nil)
					return
				end
				stopL(inp.KeyCode)
			end)
			lconnGamepad = UIS.InputBegan:Connect(function(inp)
				if not listening then
					return
				end
				if inp.UserInputType ~= Enum.UserInputType.Gamepad1 and inp.UserInputType ~= Enum.UserInputType.Gamepad2 and inp.UserInputType ~= Enum.UserInputType.Gamepad3 and inp.UserInputType ~= Enum.UserInputType.Gamepad4 then
					return
				end
				local kc = inp.KeyCode
				if kc == Enum.KeyCode.Unknown then
					return
				end
				stopL(kc)
			end)
		end)
		kbtn.MouseEnter:Connect(function()
			if not listening then
				TweenService:Create(kbtn, TweenInfo.new(0.1), {
					BackgroundColor3 = Color3.fromRGB(40, 40, 40)
				}):Play()
			end
		end)
		kbtn.MouseLeave:Connect(function()
			if not listening then
				TweenService:Create(kbtn, TweenInfo.new(0.1), {
					BackgroundColor3 = C.chipBg
				}):Play()
			end
		end)
		if keyName then
			keybindBtnRefs[keyName] = kbtn
		end
		return kbtn
	end

-- BUILD PAGES
	local function buildPage(tabName, buildFn)
		local page = Instance.new("ScrollingFrame", contentBg)
		page.Name = tabName
		page.Visible = (tabName == "Speed")
		page.Size = UDim2.new(1, 0, 1, 0)
		page.Position = UDim2.new(0, 0, 0, 0)
		page.BackgroundTransparency = 1
		page.BorderSizePixel = 0
		page.ScrollBarThickness = 3
		page.ScrollBarImageColor3 = C.accent
		page.ScrollBarImageTransparency = 0.4
		page.AutomaticCanvasSize = Enum.AutomaticSize.Y
		page.CanvasSize = UDim2.new(0, 0, 0, 0)
		local ll = Instance.new("UIListLayout", page)
		ll.SortOrder = Enum.SortOrder.LayoutOrder
		ll.Padding = UDim.new(0, 0)
		tabPages[tabName] = page
		currentPage = page
		lo = 0
		buildFn()
		currentPage = nil
	end

-- SPEED PAGE
	buildPage("Speed", function()
		makeGap(2)
		makeSectionHeader("Speed Settings")
		makeGap(2)
		normalBox = makeInputRow("Normal Speed", State.normalSpeed, function(n)
			if n > 0 and n <= 500 then
				State.normalSpeed = n
			end
		end)
		carryBox = makeInputRow("Carry Speed", State.carrySpeed, function(n)
			if n > 0 and n <= 500 then
				State.carrySpeed = n
			end
		end)
		laggerBox = makeInputRow("Lagger Speed", State.laggerSpeed, function(n)
			if n > 0 and n <= 500 then
				State.laggerSpeed = n
			end
		end)
		laggerCarryBox = makeInputRow("Lagger Carry Speed", State.laggerCarrySpeed, function(n)
			if n > 0 and n <= 500 then
				State.laggerCarrySpeed = n
			end
		end)
		makeGap(4)
		makeSectionHeader("PRC Performance")
		makeGap(2)
		local prcRow = Instance.new("Frame", currentPage)
		prcRow.Size = UDim2.new(1, 0, 0, 44)
		prcRow.BackgroundTransparency = 0.5
		prcRow.BorderSizePixel = 0
		prcRow.LayoutOrder = LO()
		prcRow.BackgroundColor3 = C.rowBg
		mkCorner(prcRow, 8)
		local prcWrap = Instance.new("Frame", prcRow)
		prcWrap.Size = UDim2.new(1, - 24, 0, 34)
		prcWrap.Position = UDim2.new(0, 12, 0, 5)
		prcWrap.BackgroundColor3 = C.modeBtnBg
		prcWrap.BackgroundTransparency = 0.4
		prcWrap.BorderSizePixel = 0
		mkCorner(prcWrap, 7)
		mkStroke(prcWrap, C.modeBtnBrd, 1)
		local prcLL = Instance.new("UIListLayout", prcWrap)
		prcLL.FillDirection = Enum.FillDirection.Horizontal
		prcLL.SortOrder = Enum.SortOrder.LayoutOrder
		prcLL.Padding = UDim.new(0, 0)
		local prcModes = {
			"Default",
			"Normal",
			"High"
		}
		local prcBtns = {}
		for i, mode in ipairs(prcModes) do
			local b = Instance.new("TextButton", prcWrap)
			b.Size = UDim2.new(1 / 3, 0, 1, 0)
			b.BackgroundColor3 = (mode == State.prcMode) and C.modeBtnActBg or C.modeBtnBg
			b.BackgroundTransparency = (mode == State.prcMode) and 0.3 or 0.5
			b.BorderSizePixel = 0
			b.Text = mode
			b.TextColor3 = (mode == State.prcMode) and C.modeBtnActTx or C.modeBtnTxt
			b.Font = Enum.Font.GothamBold
			b.TextSize = 12
			b.ZIndex = 8
			b.LayoutOrder = i
			mkCorner(b, 5)
			b.AutoButtonColor = false
			b.MouseEnter:Connect(function()
				if mode ~= State.prcMode then
					TweenService:Create(b, TweenInfo.new(0.1), {
						BackgroundColor3 = Color3.fromRGB(60, 60, 60),
						TextColor3 = Color3.fromRGB(255, 255, 255)
					}):Play()
				end
			end)
			b.MouseLeave:Connect(function()
				if mode ~= State.prcMode then
					TweenService:Create(b, TweenInfo.new(0.1), {
						BackgroundColor3 = C.modeBtnBg,
						TextColor3 = C.modeBtnTxt
					}):Play()
				end
			end)
			b.MouseButton1Click:Connect(function()
				for _, m in ipairs(prcModes) do
					local btn = prcBtns[m]
					if btn then
						TweenService:Create(btn, TweenInfo.new(0.15), {
							BackgroundColor3 = (m == mode) and C.modeBtnActBg or C.modeBtnBg,
							BackgroundTransparency = (m == mode) and 0.3 or 0.5,
							TextColor3 = (m == mode) and C.modeBtnActTx or C.modeBtnTxt
						}):Play()
					end
				end
				applyPRCMode(mode)
			end)
			prcBtns[mode] = b
		end
		local prcInfo = Instance.new("TextLabel", currentPage)
		prcInfo.Size = UDim2.new(1, - 24, 0, 20)
		prcInfo.Position = UDim2.new(0, 12, 0, 0)
		prcInfo.BackgroundTransparency = 1
		prcInfo.Text = "Default: 60 FPS | Normal: 120 FPS | High: Unlimited + Boost"
		prcInfo.TextColor3 = C.rowSub
		prcInfo.Font = Enum.Font.Gotham
		prcInfo.TextSize = 9
		prcInfo.TextXAlignment = Enum.TextXAlignment.Left
		prcInfo.LayoutOrder = LO()
		makeGap(4)
		local modeRow = Instance.new("Frame", currentPage)
		modeRow.Size = UDim2.new(1, 0, 0, 48)
		modeRow.BackgroundTransparency = 0.5
		modeRow.BorderSizePixel = 0
		modeRow.LayoutOrder = LO()
		modeRow.BackgroundColor3 = C.rowBg
		mkCorner(modeRow, 8)
		local modeWrap = Instance.new("Frame", modeRow)
		modeWrap.Size = UDim2.new(1, - 24, 0, 34)
		modeWrap.Position = UDim2.new(0, 12, 0, 7)
		modeWrap.BackgroundColor3 = C.modeBtnBg
		modeWrap.BackgroundTransparency = 0.4
		modeWrap.BorderSizePixel = 0
		mkCorner(modeWrap, 7)
		mkStroke(modeWrap, C.modeBtnBrd, 1)
		local modeLL = Instance.new("UIListLayout", modeWrap)
		modeLL.FillDirection = Enum.FillDirection.Horizontal
		modeLL.SortOrder = Enum.SortOrder.LayoutOrder
		modeLL.Padding = UDim.new(0, 0)
		local modeStatusRow = Instance.new("Frame", currentPage)
		modeStatusRow.Size = UDim2.new(1, 0, 0, 22)
		modeStatusRow.BackgroundTransparency = 1
		modeStatusRow.BorderSizePixel = 0
		modeStatusRow.LayoutOrder = LO()
		local modeStatusLbl = Instance.new("TextLabel", modeStatusRow)
		modeStatusLbl.Size = UDim2.new(1, - 24, 1, 0)
		modeStatusLbl.Position = UDim2.new(0, 12, 0, 0)
		modeStatusLbl.BackgroundTransparency = 1
		modeStatusLbl.Text = "Mode: Normal"
		modeStatusLbl.TextColor3 = C.rowSub
		modeStatusLbl.Font = Enum.Font.Gotham
		modeStatusLbl.TextSize = 11
		modeStatusLbl.TextXAlignment = Enum.TextXAlignment.Left
		local modeNames = {
			"Normal",
			"Carry",
			"Lagger",
			"Lagger Carry"
		}
		local function setModeActive(active)
			for _, m in ipairs(modeNames) do
				local b = modeBtns[m]
				if not b then
					continue
				end
				local isActive = (m == active)
				TweenService:Create(b, TweenInfo.new(0.15), {
					BackgroundColor3 = isActive and C.modeBtnActBg or C.modeBtnBg,
					BackgroundTransparency = isActive and 0.3 or 0.5,
					TextColor3 = isActive and C.modeBtnActTx or C.modeBtnTxt
				}):Play()
			end
			modeStatusLbl.Text = "Mode: " .. active
			if active == "Normal" then
				enforceSpeedExclusivity("normal")
			elseif active == "Carry" then
				enforceSpeedExclusivity("carry")
			elseif active == "Lagger" then
				enforceSpeedExclusivity("lagger")
			elseif active == "Lagger Carry" then
				enforceSpeedExclusivity("laggerCarry")
			end
		end
		for i, mname in ipairs(modeNames) do
			local b = Instance.new("TextButton", modeWrap)
			b.Size = UDim2.new(1 / 4, 0, 1, 0)
			b.BackgroundColor3 = (i == 1) and C.modeBtnActBg or C.modeBtnBg
			b.BackgroundTransparency = (i == 1) and 0.3 or 0.5
			b.BorderSizePixel = 0
			b.Text = mname
			b.TextColor3 = (i == 1) and C.modeBtnActTx or C.modeBtnTxt
			b.Font = Enum.Font.GothamBold
			b.TextSize = 10
			b.ZIndex = 8
			b.LayoutOrder = i
			mkCorner(b, 5)
			b.AutoButtonColor = false
			b.MouseEnter:Connect(function()
				local currentActive = (State.speedToggled and "Carry" or State.laggerEnabled and "Lagger" or State.laggerCarryEnabled and "Lagger Carry" or "Normal")
				if mname ~= currentActive then
					TweenService:Create(b, TweenInfo.new(0.1), {
						BackgroundColor3 = Color3.fromRGB(60, 60, 60),
						TextColor3 = Color3.fromRGB(255, 255, 255)
					}):Play()
				end
			end)
			b.MouseLeave:Connect(function()
				local currentActive = (State.speedToggled and "Carry" or State.laggerEnabled and "Lagger" or State.laggerCarryEnabled and "Lagger Carry" or "Normal")
				if mname ~= currentActive then
					TweenService:Create(b, TweenInfo.new(0.1), {
						BackgroundColor3 = C.modeBtnBg,
						TextColor3 = C.modeBtnTxt
					}):Play()
				end
			end)
			b.MouseButton1Click:Connect(function()
				setModeActive(mname)
			end)
			modeBtns[mname] = b
		end
	end)

-- AIMBOT PAGE
	buildPage("Aimbot", function()
		makeGap(4)
		makeSectionHeader("Aimbot Settings")
		makeGap(2)
		local aimbotSpeedBox = makeInputRow("Movement Speed", State.autoBatSpeed, function(val)
			if val >= 10 and val <= 200 then
				State.autoBatSpeed = val
				AUTO_BAT_SPEED = val
				pcall(saveConfig)
			end
		end)
		local infoAim = Instance.new("TextLabel", currentPage)
		infoAim.Size = UDim2.new(1, - 24, 0, 24)
		infoAim.Position = UDim2.new(0, 12, 0, 0)
		infoAim.BackgroundTransparency = 1
		infoAim.Text = "Controls how fast you move towards the target"
		infoAim.TextColor3 = C.rowSub
		infoAim.Font = Enum.Font.Gotham
		infoAim.TextSize = 10
		infoAim.TextXAlignment = Enum.TextXAlignment.Left
		infoAim.LayoutOrder = LO()
		makeGap(2)
		makeSectionHeader("FACING")
		makeGap(2)
		autoBatDistBox = makeInputRow("FACING", State.autoBatDist or 2.5, function(val)
			if val >= 0.5 and val <= 10 then
				State.autoBatDist = val
				AUTO_BAT_DIST = val
				pcall(saveConfig)
			end
		end)
		local batDistInfo = Instance.new("TextLabel", currentPage)
		batDistInfo.Size = UDim2.new(1, - 24, 0, 24)
		batDistInfo.Position = UDim2.new(0, 12, 0, 0)
		batDistInfo.BackgroundTransparency = 1
		batDistInfo.Text = "Distance from target when auto-bat is active (default: 2.5)"
		batDistInfo.TextColor3 = C.rowSub
		batDistInfo.Font = Enum.Font.Gotham
		batDistInfo.TextSize = 10
		batDistInfo.TextXAlignment = Enum.TextXAlignment.Left
		batDistInfo.LayoutOrder = LO()
		makeGap(2)
		makeSectionHeader("Bat Counter (Auto Swing on Ragdoll)")
		makeGap(2)
		toggleSetters.batCounter = makeToggleRow("Bat Counter", State.batCounterEnabled, function(on)
			State.batCounterEnabled = on
			toggleBatCounter(on)
		end)
		makeGap(4)
		makeSectionHeader("Medusa Counter")
		makeGap(2)
		setMedusaCounter = makeToggleRow("Medusa Counter", State.medusaCounterEnabled, function(on)
			State.medusaCounterEnabled = on
			if on then
				setupMedusaCounter(LP.Character)
			else
				stopMedusaCounter()
			end
		end)
		toggleSetters.medusaCounter = setMedusaCounter
		makeGap(4)
		makeSectionHeader("Status")
		makeGap(2)
		local statusText = Instance.new("TextLabel", currentPage)
		statusText.Size = UDim2.new(1, - 24, 0, 30)
		statusText.Position = UDim2.new(0, 12, 0, 0)
		statusText.BackgroundTransparency = 1
		statusText.Text = "Press AIMBOT button (default: F) to toggle\nAuto aims, moves, and swings bat at enemies\nRED ESP automatically enabled when aimbot is ON"
		statusText.TextColor3 = C.rowValue
		statusText.Font = Enum.Font.GothamBold
		statusText.TextSize = 11
		statusText.TextXAlignment = Enum.TextXAlignment.Left
		statusText.LayoutOrder = LO()
	end)

-- MECHANICS PAGE (with Anti-Lag, Optimizer, Sky Theme)
	buildPage("Mechanics", function()
		makeGap(2)
		makeSectionHeader("Asta Auto Steal (Elegant Progress Bar)")
		makeGap(2)
		autoStealToggle = makeToggleRow("Asta Auto Steal", State.autoStealEnabled, function(on)
			State.autoStealEnabled = on
			setAutoStealEnabled(on)
		end)
		autoStealRadiusBox = makeInputRow("Steal Radius", State.autoStealRadius, function(n)
			if n >= 5 and n <= 300 then
				State.autoStealRadius = math.floor(n)
