-- GG z

if getgenv().Library then 
	getgenv().Library:Unload()
end

if not isfolder("Astroxel") then
	makefolder("Astroxel")
end

if not isfolder("Astroxel/Assets") then
	makefolder("Astroxel/Assets")
end

if not isfolder("Astroxel/Configs") then
	makefolder("Astroxel/Configs")
end

if not isfolder("Astroxel/Themes") then
	makefolder("Astroxel/Themes")
end

local Library do
	local Workspace = game:GetService("Workspace")
	local UserInputService = game:GetService("UserInputService")
	local Players = game:GetService("Players")
	local HttpService = game:GetService("HttpService")
	local RunService = game:GetService("RunService")
	local CoreGui = cloneref and cloneref(game:GetService("CoreGui")) or game:GetService("CoreGui")
	local TweenService = game:GetService("TweenService")
	local Stats = game:GetService("Stats")

	gethui = gethui or function()
		return CoreGui
	end

	local LocalPlayer = Players.LocalPlayer
	local Camera = Workspace.CurrentCamera
	local Mouse = LocalPlayer:GetMouse()

	local FromRGB = Color3.fromRGB
	local FromHSV = Color3.fromHSV
	local FromHex = Color3.fromHex

	local RGBSequence = ColorSequence.new
	local RGBSequenceKeypoint = ColorSequenceKeypoint.new
	local NumSequence = NumberSequence.new
	local NumSequenceKeypoint = NumberSequenceKeypoint.new

	local UDim2New = UDim2.new
	local UDimNew = UDim.new
	local Vector2New = Vector2.new

	local MathClamp = math.clamp
	local MathFloor = math.floor
	local MathAbs = math.abs
	local MathSin = math.sin

	local TableInsert = table.insert
	local TableFind = table.find
	local TableRemove = table.remove
	local TableConcat = table.concat
	local TableClone = table.clone
	local TableUnpack = table.unpack

	local StringFormat = string.format
	local StringFind = string.find
	local StringGSub = string.gsub
	local StringLower = string.lower

	local InstanceNew = Instance.new

	local MathMax = math.max
	local MathRad = math.rad

	local Vector3New = Vector3.new
	local CFrameNew = CFrame.new
	local CFrameAngles = CFrame.Angles

	local RectNew = Rect.new

	local IsMobile = false

	if UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled and not UserInputService.MouseEnabled then
		IsMobile = true
	elseif not UserInputService.TouchEnabled and UserInputService.KeyboardEnabled and UserInputService.MouseEnabled then
		IsMobile = false
	elseif UserInputService.TouchEnabled and UserInputService.KeyboardEnabled and UserInputService.MouseEnabled then 
		IsMobile = false
	end

	Library = {
		Theme =  { },
		Themes = { },

		MenuKeybind = tostring(Enum.KeyCode.RightControl), 
		Flags = { },

		Tween = {
			Time = 0.25,
			Style = Enum.EasingStyle.Quart,
			Direction = Enum.EasingDirection.Out
		},

		Folders = {
			Directory = "Astroxel",
			Configs = "Astroxel/Configs",
			Assets = "Astroxel/Assets",
			Themes = "Astroxel/Themes"
		},

		Images = {
			["Saturation"] = {"Saturation.png", "https://github.com/sametexe001/images/blob/main/saturation.png?raw=true" },
			["Value"] = { "Value.png", "https://github.com/sametexe001/images/blob/main/value.png?raw=true" },
			["Hue"] = { "Hue.png", "https://github.com/sametexe001/images/blob/main/horizontalhue.png?raw=true" },
			["Checkers"] = { "Checkers.png", "https://github.com/sametexe001/images/blob/main/checkers.png?raw=true" },
		},

		-- Ignore below
		Pages = { },
		Sections = { },

		Connections = { },
		Threads = { },

		ThemeMap = { },
		ThemeItems = { },

		OpenFrames = { },

		CurrentPage = nil,

		SearchItems = { },

		ThemeColorpickers = { },

		SetFlags = { },

		UnnamedConnections = 0,
		UnnamedFlags = 0,

		Holder = nil,
		NotifHolder = nil,
		UnusedHolder = nil,
		Font = nil,
		KeyList = nil
	}

	Library.__index = Library

	Library.Sections.__index = Library.Sections
	Library.Pages.__index = Library.Pages

	local Keys = {
		["Unknown"]           = "Unknown",
		["Backspace"]         = "Back",
		["Tab"]               = "Tab",
		["Clear"]             = "Clear",
		["Return"]            = "Return",
		["Pause"]             = "Pause",
		["Escape"]            = "Escape",
		["Space"]             = "Space",
		["QuotedDouble"]      = '"',
		["Hash"]              = "#",
		["Dollar"]            = "$",
		["Percent"]           = "%",
		["Ampersand"]         = "&",
		["Quote"]             = "'",
		["LeftParenthesis"]   = "(",
		["RightParenthesis"]  = " )",
		["Asterisk"]          = "*",
		["Plus"]              = "+",
		["Comma"]             = ",",
		["Minus"]             = "-",
		["Period"]            = ".",
		["Slash"]             = "`",
		["Three"]             = "3",
		["Seven"]             = "7",
		["Eight"]             = "8",
		["Colon"]             = ":",
		["Semicolon"]         = ";",
		["LessThan"]          = "<",
		["GreaterThan"]       = ">",
		["Question"]          = "?",
		["Equals"]            = "=",
		["At"]                = "@",
		["LeftBracket"]       = "LeftBracket",
		["RightBracket"]      = "RightBracked",
		["BackSlash"]         = "BackSlash",
		["Caret"]             = "^",
		["Underscore"]        = "_",
		["Backquote"]         = "`",
		["LeftCurly"]         = "{",
		["Pipe"]              = "|",
		["RightCurly"]        = "}",
		["Tilde"]             = "~",
		["Delete"]            = "Delete",
		["End"]               = "End",
		["KeypadZero"]        = "Keypad0",
		["KeypadOne"]         = "Keypad1",
		["KeypadTwo"]         = "Keypad2",
		["KeypadThree"]       = "Keypad3",
		["KeypadFour"]        = "Keypad4",
		["KeypadFive"]        = "Keypad5",
		["KeypadSix"]         = "Keypad6",
		["KeypadSeven"]       = "Keypad7",
		["KeypadEight"]       = "Keypad8",
		["KeypadNine"]        = "Keypad9",
		["KeypadPeriod"]      = "KeypadP",
		["KeypadDivide"]      = "KeypadD",
		["KeypadMultiply"]    = "KeypadM",
		["KeypadMinus"]       = "KeypadM",
		["KeypadPlus"]        = "KeypadP",
		["KeypadEnter"]       = "KeypadE",
		["KeypadEquals"]      = "KeypadE",
		["Insert"]            = "Insert",
		["Home"]              = "Home",
		["PageUp"]            = "PageUp",
		["PageDown"]          = "PageDown",
		["RightShift"]        = "RightShift",
		["LeftShift"]         = "LeftShift",
		["RightControl"]      = "RightControl",
		["LeftControl"]       = "LeftControl",
		["LeftAlt"]           = "LeftAlt",
		["RightAlt"]          = "RightAlt"
	}

	local Themes = {
		["Default"] = {
			["Background"] = FromRGB(15, 18, 19),
			["Inline"] = FromRGB(20, 21, 24),
			["Border"] = FromRGB(37, 37, 37),
			["Shadow"] = FromRGB(0, 0, 0),
			["Text"] = FromRGB(255, 255, 255),
			["Inactive Text"] = FromRGB(185, 185, 185),
			["Accent"] = FromRGB(28, 115, 202),
			["Element"] = FromRGB(37, 39, 43),
			["Gradient"] = FromRGB(216, 216, 216)
		}
	}
	
	local Il = loadstring(game:HttpGet("https://raw.githubusercontent.com/Dummyrme/Library/refs/heads/main/Icon.lua"))()
	function getIconData(i)
		local id = Il.Icons[i]
		if id then
			local imG = Il.Spritesheets[tostring(id.Image)]
			if imG then
				return {
					Image = Il.Spritesheets[tostring(id.Image)],
					ImageRectSize = id.ImageRectSize,
					ImageRectPosition = id.ImageRectPosition,
				}
			end
		end
		if type(i) == 'string' and not i:find('rbxassetid://') then
			return {
				Image = "rbxassetid://".. i,
				ImageRectSize = Vector2.new(0, 0),
				ImageRectPosition = Vector2.new(0, 0),
			}
		elseif type(i) == 'number' then
			return {
				Image = "rbxassetid://".. i,
				ImageRectSize = Vector2.new(0, 0),
				ImageRectPosition = Vector2.new(0, 0),
			}
		else
			return i
		end
	end
	function gl(obj, id)
		local dt = getIconData(id)
		if typeof(obj) == "Instance" and obj:IsA("GuiObject") and dt then
			obj.Image = dt.Image
			obj.ImageRectSize = dt.ImageRectSize
			obj.ImageRectOffset = dt.ImageRectPosition
		end
		return dt
	end

	Library.Theme = TableClone(Themes["Default"])
	Library.Themes = Themes

	-- Folders
	for Index, Value in Library.Folders do 
		if not isfolder(Value) then
			makefolder(Value)
		end
	end

	if not isfile(Library.Folders.Directory .. "/autoload.json") then 
		writefile(Library.Folders.Directory .. "/autoload.json", "")
	end

	-- Images
	for Index, Value in Library.Images do 
		local ImageData = Value

		local ImageName = ImageData[1]
		local ImageLink = ImageData[2]

		if not isfile(Library.Folders.Assets .. "/" .. ImageName) then
			writefile(Library.Folders.Assets .. "/" .. ImageName, game:HttpGet(ImageLink))
		end
	end

	-- Tweening
	local Tween = { } do
		Tween.__index = Tween

		Tween.Create = function(self, Item, Info, Goal, IsRawItem)
			Item = IsRawItem and Item or Item.Instance
			Info = Info or TweenInfo.new(Library.Tween.Time, Library.Tween.Style, Library.Tween.Direction)

			local NewTween = {
				Tween = TweenService:Create(Item, Info, Goal),
				Info = Info,
				Goal = Goal,
				Item = Item
			}

			NewTween.Tween:Play()

			setmetatable(NewTween, Tween)

			return NewTween
		end

		Tween.GetProperty = function(self, Item)
			Item = Item or self.Item 

			if Item:IsA("Frame") then
				return { "BackgroundTransparency" }
			elseif Item:IsA("TextLabel") or Item:IsA("TextButton") then
				return { "TextTransparency", "BackgroundTransparency" }
			elseif Item:IsA("ImageLabel") or Item:IsA("ImageButton") then
				return { "BackgroundTransparency", "ImageTransparency" }
			elseif Item:IsA("ScrollingFrame") then
				return { "BackgroundTransparency", "ScrollBarImageTransparency" }
			elseif Item:IsA("TextBox") then
				return { "TextTransparency", "BackgroundTransparency" }
			elseif Item:IsA("UIStroke") then 
				return { "Transparency" }
			end
		end

		Tween.FadeItem = function(self, Item, Property, Visibility, Speed)
			local Item = Item or self.Item 

			local OldTransparency = Item[Property]
			Item[Property] = Visibility and 1 or OldTransparency

			local NewTween = Tween:Create(Item, TweenInfo.new(Speed or Library.Tween.Time, Library.Tween.Style, Library.Tween.Direction), {
				[Property] = Visibility and OldTransparency or 1
			}, true)

			Library:Connect(NewTween.Tween.Completed, function()
				if not Visibility then 
					task.wait()
					Item[Property] = OldTransparency
				end
			end)

			return NewTween
		end

		Tween.Get = function(self)
			if not self.Tween then 
				return
			end

			return self.Tween, self.Info, self.Goal
		end

		Tween.Pause = function(self)
			if not self.Tween then 
				return
			end

			self.Tween:Pause()
		end

		Tween.Play = function(self)
			if not self.Tween then 
				return
			end

			self.Tween:Play()
		end

		Tween.Clean = function(self)
			if not self.Tween then 
				return
			end

			Tween:Pause()
			self = nil
		end
	end

	-- Instances
	local Instances = { } do
		Instances.__index = Instances

		Instances.Create = function(self, Class, Properties)
			local NewItem = {
				Instance = InstanceNew(Class),
				Properties = Properties,
				Class = Class
			}

			setmetatable(NewItem, Instances)

			for Property, Value in NewItem.Properties do
				NewItem.Instance[Property] = Value
			end

			return NewItem
		end

		Instances.Border = function(self)
			if not self.Instance then 
				return
			end

			local Item = self.Instance
			local UIStroke = Instances:Create("UIStroke", {
				Parent = Item,
				Color = Library.Theme.Border,
				Thickness = 1,
				LineJoinMode = Enum.LineJoinMode.Miter
			})

			UIStroke:AddToTheme({Color = "Border"})

			return UIStroke
		end

		Instances.Tooltip = function(self, Text)
			if not self.Instance then 
				return
			end

			if Text == nil then 
				return
			end

			local Gui = self.Instance

			local MouseLocation = UserInputService:GetMouseLocation()
			local RenderStepped

			local Items = { } do
				Items["Tooltip"] = Instances:Create("Frame", {
					Parent = Library.Holder.Instance,
					Name = "\0",
					BackgroundColor3 = FromRGB(15, 12, 16),
					BorderSizePixel = 0,
					Position = UDim2New(0, MouseLocation.X, 0, MouseLocation.Y - 22),
					Size = UDim2New(0, 0, 0, 0),
					BackgroundTransparency = 1,
					Visible = true,
					AutomaticSize = Enum.AutomaticSize.XY,
					ZIndex = 99
				})  Items["Tooltip"]:AddToTheme({BackgroundColor3 = "Background"})

				Items["Text"] = Instances:Create("TextLabel", {
					Parent = Items["Tooltip"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = Text,
					BorderSizePixel = 0,
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 1, 0),
					ClipsDescendants = true,
					ZIndex = 99,
					TextTransparency = 1,
					AutomaticSize = Enum.AutomaticSize.XY,
					TextXAlignment = Enum.TextXAlignment.Left,
					TextSize = 14,
					BackgroundColor3 = FromRGB(15, 12, 16)
				})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

				Instances:Create("UIPadding", {
					Parent = Items["Text"].Instance,
					Name = "\0",
					PaddingBottom = UDimNew(0, 8),
					PaddingLeft = UDimNew(0, 8),
					PaddingRight = UDimNew(0, 8),
					PaddingTop = UDimNew(0, 8),
				})

				Instances:Create("UICorner", {
					Parent = Items["Tooltip"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})
			end

			Library:Connect(Gui.MouseEnter, function()
				Items["Tooltip"].Instance.Position = UDim2New(0, MouseLocation.X, 0, MouseLocation.Y - 42)
				Items["Tooltip"]:Tween(nil, {BackgroundTransparency = 0})
				Items["Text"]:Tween(nil, {TextTransparency = 0})

				RenderStepped = RunService.RenderStepped:Connect(function()
					MouseLocation = UserInputService:GetMouseLocation()
					Items["Tooltip"]:Tween(nil, {Position = UDim2New(0, MouseLocation.X, 0, MouseLocation.Y - 42)})
				end)
			end)

			Library:Connect(Gui.MouseLeave, function()
				Items["Tooltip"]:Tween(nil, {BackgroundTransparency = 1})
				Items["Text"]:Tween(nil, {TextTransparency = 1})

				if RenderStepped then 
					RenderStepped:Disconnect()
					RenderStepped = nil
				end
			end)
		end

		Instances.FadeItem = function(self, Visibility, Speed)
			local Item = self.Instance

			if Visibility == true then 
				Item.Visible = true
			end

			local Descendants = Item:GetDescendants()
			TableInsert(Descendants, Item)

			local NewTween

			for Index, Value in Descendants do 
				local TransparencyProperty = Tween:GetProperty(Value)

				if not TransparencyProperty then 
					continue
				end

				if type(TransparencyProperty) == "table" then 
					for _, Property in TransparencyProperty do 
						NewTween = Tween:FadeItem(Value, Property, not Visibility, Speed)
					end
				else
					NewTween = Tween:FadeItem(Value, TransparencyProperty, not Visibility, Speed)
				end
			end
		end

		Instances.AddToTheme = function(self, Properties)
			if not self.Instance then 
				return
			end

			Library:AddToTheme(self, Properties)
		end

		Instances.ChangeItemTheme = function(self, Properties)
			if not self.Instance then 
				return
			end

			Library:ChangeItemTheme(self, Properties)
		end

		Instances.Connect = function(self, Event, Callback, Name)
			if not self.Instance then 
				return
			end

			if not self.Instance[Event] then 
				return
			end

			if Event == "MouseButton1Down" or Event == "MouseButton1Click" then 
				if IsMobile then 
					Event = "TouchTap"
				end
			elseif Event == "MouseButton2Down" or Event == "MouseButton2Click" then 
				if IsMobile then
					Event = "TouchLongPress"
				end
			end

			return Library:Connect(self.Instance[Event], Callback, Name)
		end

		Instances.Tween = function(self, Info, Goal)
			if not self.Instance then 
				return
			end

			return Tween:Create(self, Info, Goal)
		end

		Instances.Disconnect = function(self, Name)
			if not self.Instance then 
				return
			end

			return Library:Disconnect(Name)
		end

		Instances.Clean = function(self)
			if not self.Instance then 
				return
			end

			self.Instance:Destroy()
			self = nil
		end

		Instances.MakeDraggable = function(self)
			if not self.Instance then 
				return
			end

			local Gui = self.Instance

			local Dragging = false 
			local DragStart
			local StartPosition 

			local Set = function(Input)
				local DragDelta = Input.Position - DragStart
				self:Tween(TweenInfo.new(0.16, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2New(StartPosition.X.Scale, StartPosition.X.Offset + DragDelta.X, StartPosition.Y.Scale, StartPosition.Y.Offset + DragDelta.Y)})
			end

			self:Connect("InputBegan", function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 or Input.UserInputType == Enum.UserInputType.Touch then
					Dragging = true

					DragStart = Input.Position
					StartPosition = Gui.Position

					Input.Changed:Connect(function()
						if Input.UserInputState == Enum.UserInputState.End then
							Dragging = false
						end
					end)
				end
			end)

			Library:Connect(UserInputService.InputChanged, function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseMovement or Input.UserInputType == Enum.UserInputType.Touch then
					if Dragging then
						Set(Input)
					end
				end
			end)

			return Dragging
		end

		Instances.MakeResizeable = function(self, Minimum, Maximum)
			if not self.Instance then 
				return
			end

			local Gui = self.Instance

			local Resizing = false 
			local Start = UDim2New()
			local Delta = UDim2New()
			local ResizeMax = Gui.Parent.AbsoluteSize - Gui.AbsoluteSize

			local ResizeButton = Instances:Create("ImageButton", {
				Parent = Gui,
				Image = "rbxassetid://7368471234",
				AnchorPoint = Vector2New(1, 1),
				BorderColor3 = FromRGB(0, 0, 0),
				Size = UDim2New(0, 6, 0, 6),
				Position = UDim2New(1, -4, 1, -4),
				Name = "\0",
				BorderSizePixel = 0,
				BackgroundTransparency = 1,
				ZIndex = 5,
				AutoButtonColor = false,
				Visible = true,
			})  ResizeButton:AddToTheme({ImageColor3 = "Accent"})

			ResizeButton:Connect("InputBegan", function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 or Input.UserInputType == Enum.UserInputType.Touch then

					Resizing = true

					Start = Gui.Size - UDim2New(0, Input.Position.X, 0, Input.Position.Y)

					Input.Changed:Connect(function()
						if Input.UserInputState == Enum.UserInputState.End then
							Resizing = false
						end
					end)
				end
			end)

			Library:Connect(UserInputService.InputChanged, function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseMovement or Input.UserInputType == Enum.UserInputType.Touch then
					if Resizing then
						ResizeMax = Maximum or Gui.Parent.AbsoluteSize - Gui.AbsoluteSize

						Delta = Start + UDim2New(0, Input.Position.X, 0, Input.Position.Y)
						Delta = UDim2New(0, math.clamp(Delta.X.Offset, Minimum.X, ResizeMax.X), 0, math.clamp(Delta.Y.Offset, Minimum.Y, ResizeMax.Y))

						Tween:Create(Gui, TweenInfo.new(0.17, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = Delta}, true)
					end
				end
			end)

			return Resizing
		end

		Instances.OnHover = function(self, Function)
			if not self.Instance then 
				return
			end

			return Library:Connect(self.Instance.MouseEnter, Function)
		end

		Instances.OnHoverLeave = function(self, Function)
			if not self.Instance then 
				return
			end

			return Library:Connect(self.Instance.MouseLeave, Function)
		end
	end

	-- Custom font
	local CustomFont = { } do
		function CustomFont:New(Name, Weight, Style, Data)
			if isfile(Library.Folders.Assets .. "/" .. Name .. ".json") then
				return Font.new(getcustomasset(Library.Folders.Assets .. "/" .. Name .. ".json"))
			end

			if not isfile(Library.Folders.Assets .. "/" .. Name .. ".ttf") then 
				writefile(Library.Folders.Assets .. "/" .. Name .. ".ttf", game:HttpGet(Data.Url))
			end

			local FontData = {
				name = Name,
				faces = { {
					name = "Regular",
					weight = Weight,
					style = Style,
					assetId = getcustomasset(Library.Folders.Assets .. "/" .. Name .. ".ttf")
				} }
			}

			writefile(Library.Folders.Assets .. "/" .. Name .. ".json", HttpService:JSONEncode(FontData))
			return Font.new(getcustomasset(Library.Folders.Assets .. "/" .. Name .. ".json"))
		end

		function CustomFont:Get(Name)
			if isfile(Library.Folders.Assets .. "/" .. Name .. ".json") then
				return Font.new(getcustomasset(Library.Folders.Assets .. "/" .. Name .. ".json"))
			end
		end

		CustomFont:New("Inter", 200, "Regular", {
			Url = "https://github.com/sametexe001/luas/raw/refs/heads/main/fonts/InterSemibold.ttf"
		})

		Library.Font = CustomFont:Get("Inter")
	end

	Library.Holder = Instances:Create("ScreenGui", {
		Parent = gethui(),
		Name = "\0",
		ZIndexBehavior = Enum.ZIndexBehavior.Global,
		DisplayOrder = 2,
		ResetOnSpawn = false
	})

	Library.UnusedHolder = Instances:Create("ScreenGui", {
		Parent = gethui(),
		Name = "\0",
		ZIndexBehavior = Enum.ZIndexBehavior.Global,
		Enabled = false,
		ResetOnSpawn = false
	})

	Library.NotifHolder = Instances:Create("Frame", {
		Parent = Library.Holder.Instance,
		Name = "\0",
		BorderColor3 = FromRGB(0, 0, 0),
		AnchorPoint = Vector2New(1, 0),
		BackgroundTransparency = 1,
		Position = UDim2New(1, 0, 0, 0),
		Size = UDim2New(0, 0, 1, 0),
		BorderSizePixel = 0,
		AutomaticSize = Enum.AutomaticSize.X,
		BackgroundColor3 = FromRGB(255, 255, 255)
	})

	Instances:Create("UIListLayout", {
		Parent = Library.NotifHolder.Instance,
		Name = "\0",
		Padding = UDimNew(0, 12),
		SortOrder = Enum.SortOrder.LayoutOrder
	})

	Instances:Create("UIPadding", {
		Parent = Library.NotifHolder.Instance,
		Name = "\0",
		PaddingTop = UDimNew(0, 12),
		PaddingBottom = UDimNew(0, 12),
		PaddingRight = UDimNew(0, 12),
		PaddingLeft = UDimNew(0, 12)
	})

	Library.Unload = function(self)
		for Index, Value in self.Connections do 
			Value.Connection:Disconnect()
		end

		for Index, Value in self.Threads do 
			coroutine.close(Value)
		end

		if self.Holder then 
			self.Holder:Clean()
		end

		Library = nil 
		getgenv().Library = nil

		UserInputService.MouseIconEnabled = true
	end

	Library.GetImage = function(self, Image)
		local ImageData = self.Images[Image]

		if not ImageData then 
			return
		end

		return getcustomasset(self.Folders.Assets .. "/" .. ImageData[1])
	end

	Library.Round = function(self, Number, Float)
		local Multiplier = 1 / (Float or 1)
		return MathFloor(Number * Multiplier) / Multiplier
	end

	Library.Thread = function(self, Function)
		local NewThread = coroutine.create(Function)

		coroutine.wrap(function()
			coroutine.resume(NewThread)
		end)()

		TableInsert(self.Threads, NewThread)
		return NewThread
	end

	Library.SafeCall = function(self, Function, ...)
		local Arguements = { ... }
		local Success, Result = pcall(Function, TableUnpack(Arguements))

		if not Success then
			warn(Result)
			return false
		end

		return Success
	end

	Library.Connect = function(self, Event, Callback, Name)
		Name = Name or StringFormat("Connection%s%s", self.UnnamedConnections + 1, HttpService:GenerateGUID(false))

		local NewConnection = {
			Event = Event,
			Callback = Callback,
			Name = Name,
			Connection = nil
		}

		Library:Thread(function()
			NewConnection.Connection = Event:Connect(Callback)
		end)

		TableInsert(self.Connections, NewConnection)
		return NewConnection
	end

	Library.Disconnect = function(self, Name)
		for _, Connection in self.Connections do 
			if Connection.Name == Name then
				Connection.Connection:Disconnect()
				break
			end
		end
	end

	Library.NextFlag = function(self)
		local FlagNumber = self.UnnamedFlags + 1
		return StringFormat("flag_number_%s_%s", FlagNumber, HttpService:GenerateGUID(false))
	end

	Library.AddToTheme = function(self, Item, Properties)
		Item = Item.Instance or Item 

		local ThemeData = {
			Item = Item,
			Properties = Properties,
		}

		for Property, Value in ThemeData.Properties do
			if type(Value) == "string" then
				Item[Property] = self.Theme[Value]
			else
				Item[Property] = Value()
			end
		end

		TableInsert(self.ThemeItems, ThemeData)
		self.ThemeMap[Item] = ThemeData
	end

	Library.RemoveFromTheme = function(self, Item)
		Item = Item.Instance or Item

		if not self.ThemeMap[Item] then 
			return
		end

		self.ThemeMap[Item].Properties = nil
		self.ThemeMap[Item] = nil
	end

	Library.GetConfig = function(self)
		local Config = { } 

		local Success, Result = Library:SafeCall(function()
			for Index, Value in Library.Flags do 
				if type(Value) == "table" and Value.Key then
					Config[Index] = {Key = tostring(Value.Key), Mode = Value.Mode}
				elseif type(Value) == "table" and Value.Color then
					Config[Index] = {Color = "#" .. Value.Color, Alpha = Value.Alpha}
				else
					Config[Index] = Value
				end
			end
		end)

		return HttpService:JSONEncode(Config)
	end

	Library.LoadConfig = function(self, Config)
		local Decoded = HttpService:JSONDecode(Config)

		local Success, Result = Library:SafeCall(function()
			for Index, Value in Decoded do 
				local SetFunction = Library.SetFlags[Index]

				if not SetFunction then
					continue
				end

				if type(Value) == "table" and Value.Key then 
					SetFunction(Value)
				elseif type(Value) == "table" and Value.Color then
					SetFunction(Value.Color, Value.Alpha)
				else
					SetFunction(Value)
				end
			end
		end)

		return Success, Result
	end

	Library.DeleteConfig = function(self, Config)
		if isfile(Library.Folders.Configs .. "/" .. Config) then 
			delfile(Library.Folders.Configs .. "/" .. Config)
		end
	end

	Library.RefreshConfigsList = function(self, Element)
		local CurrentList = {}
		local List = {}


		local ConfigFolderName = string.gsub(self.Folders.Configs, self.Folders.Directory .. "/", "")

		for Index, Value in listfiles(self.Folders.Configs) do

			local v = tostring(Value):gsub("\\", "/")


			local root = (self.Folders.Directory .. "/" .. ConfigFolderName .. "/")
			root = root:gsub("([%^%$%(%)%.%[%]%*%+%-%?])", "%%%1")


			local FileName = v:gsub("^" .. root, "")


			if not string.find(FileName, tostring(game.GameId), 1, true) then
				continue
			end


			local RealName = string.gsub(FileName, tostring(game.GameId), "")

			List[Index] = { Name = RealName, RealName = FileName }
		end

		local IsNew = #List ~= #CurrentList
		if not IsNew then
			for Index = 1, #List do
				if List[Index] ~= CurrentList[Index] then
					IsNew = true
					break
				end
			end
		end

		if IsNew then
			CurrentList = List
			Element:Clear()
			for _, Value in CurrentList do
				Element:Add(Value.Name)
			end
		end
	end


	Library.ChangeItemTheme = function(self, Item, Properties)
		Item = Item.Instance or Item

		if not self.ThemeMap[Item] then 
			return
		end

		self.ThemeMap[Item].Properties = Properties
		self.ThemeMap[Item] = self.ThemeMap[Item]
	end

	Library.ChangeTheme = function(self, Theme, Color)
		self.Theme[Theme] = Color

		for _, Item in self.ThemeItems do
			if not Item.Properties then 
				continue
			end

			for Property, Value in Item.Properties do
				if type(Value) == "string" and Value == Theme then
					Item.Item[Property] = Color
				elseif type(Value) == "function" then
					Item.Item[Property] = Value()
				end
			end
		end
	end

	Library.GetTheme = function(self)
		local Config = { } 

		local Success, Result = Library:SafeCall(function()
			for Index, Value in Library.Flags do 
				if type(Value) == "table" and Value.Color and StringFind(Index, "Theme") then
					Config[Index] = {Color = "#" .. Value.Color, Alpha = Value.Alpha}
				end
			end
		end)

		return HttpService:JSONEncode(Config)
	end

	Library.LoadTheme = function(self, Config)
		local Decoded = HttpService:JSONDecode(Config)

		local Success, Result = Library:SafeCall(function()
			for Index, Value in Decoded do 
				local SetFunction = Library.SetFlags[Index]

				if not SetFunction then
					continue
				end

				if type(Value) == "table" and Value.Color and StringFind(Index, "Theme") then
					SetFunction(Value.Color, Value.Alpha)
				end
			end
		end)

		return Success, Result
	end

	Library.DeleteTheme = function(self, Config)
		if isfile(Library.Folders.Themes .. "/" .. Config) then 
			delfile(Library.Folders.Themes .. "/" .. Config)
		end
	end

	Library.SaveTheme = function(self, Config)
		if isfile(Library.Folders.Themes .. "/" .. Config .. ".json") then
			writefile(Library.Folders.Themes .. "/" .. Config .. ".json", Library:GetTheme())
		end
	end

	Library.RefreshThemesList = function(self, Element)
		local CurrentList = {}
		local List = {}


		local ConfigFolderName = string.gsub(self.Folders.Themes, self.Folders.Directory .. "/", "")

		for Index, Value in listfiles(self.Folders.Themes) do

			local v = tostring(Value):gsub("\\", "/")


			local root = (self.Folders.Directory .. "/" .. ConfigFolderName .. "/")
			root = root:gsub("([%^%$%(%)%.%[%]%*%+%-%?])", "%%%1")


			local FileName = v:gsub("^" .. root, "")

			List[Index] = FileName
		end

		local IsNew = #List ~= #CurrentList
		if not IsNew then
			for Index = 1, #List do
				if List[Index] ~= CurrentList[Index] then
					IsNew = true
					break
				end
			end
		end

		if IsNew then
			CurrentList = List
			Element:Refresh(CurrentList)
		end
	end


	Library.IsMouseOverFrame = function(self, Frame, XOffset, YOffset)
		Frame = Frame.Instance
		XOffset = XOffset or 0 
		YOffset = YOffset or 0

		local MousePosition = Vector2New(Mouse.X + XOffset, Mouse.Y + YOffset)

		return MousePosition.X >= Frame.AbsolutePosition.X and MousePosition.X <= Frame.AbsolutePosition.X + Frame.AbsoluteSize.X 
			and MousePosition.Y >= Frame.AbsolutePosition.Y and MousePosition.Y <= Frame.AbsolutePosition.Y + Frame.AbsoluteSize.Y
	end

	local Components = { } do
		Components.Toggle = function(Data)
			local Toggle = {
				Value = false,
				Flag = Data.Flag,
				Disabled = false,
				OnChanged = nil
			}

			local Items = { } do
				Items["Toggle"] = Instances:Create("TextButton", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					BackgroundTransparency = 1,
					BorderSizePixel = 0,
					Size = UDim2New(1, 0, 0, 20),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Items["Text"] = Instances:Create("TextLabel", {
					Parent = Items["Toggle"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					TextTransparency = 0.4000000059604645,
					Text = Data.Name,
					AutomaticSize = Enum.AutomaticSize.X,
					Size = UDim2New(0, 0, 0, 15),
					AnchorPoint = Vector2New(0, 0.5),
					BorderSizePixel = 0,
					BackgroundTransparency = 1,
					Position = UDim2New(0, 0, 0.5, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

				Items["Indicator"] = Instances:Create("Frame", {
					Parent = Items["Toggle"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(1, 0),
					Position = UDim2New(1, 0, 0, 0),
					Size = UDim2New(0, 40, 0, 20),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(36, 32, 39)
				})  Items["Indicator"]:AddToTheme({BackgroundColor3 = "Element"})

				Instances:Create("UICorner", {
					Parent = Items["Indicator"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Instances:Create("UIGradient", {
					Parent = Items["Indicator"].Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				Items["Circle"] = Instances:Create("Frame", {
					Parent = Items["Indicator"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					BackgroundTransparency = 0.4000000059604645,
					Position = UDim2New(0, 3, 0, 3),
					Size = UDim2New(0, 14, 0, 14),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UICorner", {
					Parent = Items["Circle"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Items["SubElementsHolder"] = Instances:Create("Frame", {
					Parent = Items["Toggle"].Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					AnchorPoint = Vector2New(1, 0),
					Size = UDim2New(0, 0, 1, 0),
					AutomaticSize = Enum.AutomaticSize.X,
					Position = UDim2New(1, -46, 0, 0)
				})

				Instances:Create("UIListLayout", {
					Parent = Items["SubElementsHolder"].Instance,
					Name = "\0",
					Padding = UDimNew(0, 5),
					SortOrder = Enum.SortOrder.LayoutOrder,
					FillDirection = Enum.FillDirection.Horizontal
				})
			end

			function Toggle:Set(Bool)
				if self.Disabled then
					return
				end

				self.Value = Bool
				Library.Flags[self.Flag] = self.Value

				if self.Value then
					Items["Indicator"]:ChangeItemTheme({BackgroundColor3 = "Accent"})

					Items["Indicator"]:Tween(nil, {BackgroundColor3 = Library.Theme.Accent})
					Items["Circle"]:Tween(nil, {BackgroundTransparency = 0, Position = UDim2New(1, -3, 0, 3), AnchorPoint = Vector2New(1, 0)})
					Items["Text"]:Tween(nil, {TextTransparency = 0})
				else
					Items["Indicator"]:ChangeItemTheme({BackgroundColor3 = "Element"})

					Items["Indicator"]:Tween(nil, {BackgroundColor3 = Library.Theme.Element})
					Items["Circle"]:Tween(nil, {BackgroundTransparency = 0.4, Position = UDim2New(0, 3, 0, 3), AnchorPoint = Vector2New(0, 0)})
					Items["Text"]:Tween(nil, {TextTransparency = 0.4})
				end

				if Data.Callback then 
					Library:SafeCall(Data.Callback, self.Value)
				end

				if self.OnChanged then 
					self.OnChanged(self.Value)
				end
			end

			function Toggle:SetText(Text)
				Text = tostring(Text)

				Items["Text"].Instance.Text = Text
			end

			function Toggle:SetVisible(Bool)
				Items["Toggle"].Instance.Visible = Bool
			end

			function Toggle:SetDisabled(Bool)
				self.Disabled = Bool

				if self.Disabled then
					Items["Text"]:Tween(nil, {TextTransparency = 0.6})
					Items["Indicator"]:Tween(nil, {BackgroundTransparency = 0.6})
					Items["Circle"]:Tween(nil, {BackgroundTransparency = 0.6})
				else
					if self.Value then
						Items["Text"]:Tween(nil, {TextTransparency = 0})
						Items["Circle"]:Tween(nil, {BackgroundTransparency = 0})
						Items["Indicator"]:Tween(nil, {BackgroundTransparency = 0})
					else
						Items["Text"]:Tween(nil, {TextTransparency = 0.4})
						Items["Circle"]:Tween(nil, {BackgroundTransparency = 0.4})
						Items["Indicator"]:Tween(nil, {BackgroundTransparency = 0})
					end
				end
			end

			local SearchData = {
				Name = Data.Name,
				Item = Items["Toggle"]
			}

			local PageSearchData = Library.SearchItems[Data.Page]

			if not PageSearchData then 
				return 
			end

			TableInsert(PageSearchData, SearchData)

			Items["Toggle"]:Connect("MouseButton1Down", function()
				Toggle:Set(not Toggle.Value)
			end)

			if Data.Disabled then 
				Toggle:SetDisabled(Data.Disabled)
			end

			Toggle:Set(Data.Default)

			Library.SetFlags[Toggle.Flag] = function(Value)
				Toggle:Set(Value)
			end

			return Toggle, Items 
		end

		Components.Checkbox = function(Data)
			local Checkbox = {
				Value = false,
				Flag = Data.Flag,
				Disabled = false,
				OnChanged = nil
			}

			Library.Flags[Checkbox.Flag] = nil

			local Items = { } do
				Items["Checkbox"] = Instances:Create("TextButton", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					BackgroundTransparency = 1,
					BorderSizePixel = 0,
					Size = UDim2New(1, 0, 0, 20),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Items["Text"] = Instances:Create("TextLabel", {
					Parent = Items["Checkbox"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					TextTransparency = 0.4000000059604645,
					Text = Data.Name,
					AutomaticSize = Enum.AutomaticSize.X,
					Size = UDim2New(0, 0, 0, 15),
					AnchorPoint = Vector2New(0, 0.5),
					BorderSizePixel = 0,
					BackgroundTransparency = 1,
					Position = UDim2New(0, 0, 0.5, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

				Items["Indicator"] = Instances:Create("Frame", {
					Parent = Items["Checkbox"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(1, 0),
					Position = UDim2New(1, 0, 0, 0),
					Size = UDim2New(0, 20, 0, 20),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(36, 32, 39)
				})  Items["Indicator"]:AddToTheme({BackgroundColor3 = "Element"})

				Instances:Create("UICorner", {
					Parent = Items["Indicator"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Instances:Create("UIGradient", {
					Parent = Items["Indicator"].Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				Items["Check"] = Instances:Create("ImageLabel", {
					Parent = Items["Indicator"].Instance,
					Name = "\0",
					ImageColor3 = FromRGB(0, 0, 0),
					ScaleType = Enum.ScaleType.Fit,
					ImageTransparency = 1,
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, -2, 1, -2),
					AnchorPoint = Vector2New(0.5, 0.5),
					Image = "rbxassetid://116339777575852",
					BackgroundTransparency = 1,
					Position = UDim2New(0.5, 0, 0.5, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Items["SubElementsHolder"] = Instances:Create("Frame", {
					Parent = Items["Checkbox"].Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					AnchorPoint = Vector2New(1, 0),
					Size = UDim2New(0, 0, 1, 0),
					AutomaticSize = Enum.AutomaticSize.X,
					Position = UDim2New(1, -24, 0, 0)
				})

				Instances:Create("UIListLayout", {
					Parent = Items["SubElementsHolder"].Instance,
					Name = "\0",
					Padding = UDimNew(0, 5),
					SortOrder = Enum.SortOrder.LayoutOrder,
					FillDirection = Enum.FillDirection.Horizontal
				})
			end

			function Checkbox:Set(Bool)
				if self.Disabled then
					return
				end

				self.Value = Bool
				Library.Flags[self.Flag] = self.Value

				if self.Value then
					Items["Indicator"]:ChangeItemTheme({BackgroundColor3 = "Accent"})

					Items["Indicator"]:Tween(nil, {BackgroundColor3 = Library.Theme.Accent})
					Items["Check"]:Tween(nil, {ImageTransparency = 0})
					Items["Text"]:Tween(nil, {TextTransparency = 0})
				else
					Items["Indicator"]:ChangeItemTheme({BackgroundColor3 = "Element"})

					Items["Indicator"]:Tween(nil, {BackgroundColor3 = Library.Theme.Element})
					Items["Check"]:Tween(nil, {ImageTransparency = 1})
					Items["Text"]:Tween(nil, {TextTransparency = 0.4})
				end

				if Data.Callback then 
					Library:SafeCall(Data.Callback, self.Value)
				end

				if self.OnChanged then 
					self.OnChanged(self.Value)
				end
			end

			function Checkbox:SetText(Text)
				Text = tostring(Text)

				Items["Text"].Instance.Text = Text
			end

			function Checkbox:SetVisible(Bool)
				Items["Toggle"].Instance.Visible = Bool
			end

			function Checkbox:SetDisabled(Bool)
				self.Disabled = Bool

				if self.Disabled then
					Items["Text"]:Tween(nil, {TextTransparency = 0.6})
					Items["Indicator"]:Tween(nil, {BackgroundTransparency = 0.6})
					Items["Check"]:Tween(nil, {ImageTransparency = 1})
				else
					if self.Value then
						Items["Text"]:Tween(nil, {TextTransparency = 0})
						Items["Indicator"]:Tween(nil, {BackgroundTransparency = 0})
						Items["Check"]:Tween(nil, {ImageTransparency = 0})
					else
						Items["Text"]:Tween(nil, {TextTransparency = 0.4})
						Items["Indicator"]:Tween(nil, {BackgroundTransparency = 0})
						Items["Check"]:Tween(nil, {ImageTransparency = 1})
					end
				end
			end

			local SearchData = {
				Name = Data.Name,
				Item = Items["Checkbox"]
			}

			local PageSearchData = Library.SearchItems[Data.Page]

			if not PageSearchData then 
				return 
			end

			TableInsert(PageSearchData, SearchData)

			Items["Checkbox"]:Connect("MouseButton1Down", function()
				Checkbox:Set(not Checkbox.Value)
			end)

			if Data.Disabled then 
				Checkbox:SetDisabled(Data.Disabled)
			end

			Checkbox:Set(Data.Default)

			Library.SetFlags[Checkbox.Flag] = function(Value)
				Checkbox:Set(Value)
			end

			return Checkbox, Items 
		end

		Components.Button = function(Data)
			local Button = { }

			local Items = { } do
				Items["Button"] = Instances:Create("Frame", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 0, 25),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UIListLayout", {
					Parent = Items["Button"].Instance,
					Name = "\0",
					FillDirection = Enum.FillDirection.Horizontal,
					HorizontalFlex = Enum.UIFlexAlignment.Fill,
					Padding = UDimNew(0, 8),
					SortOrder = Enum.SortOrder.LayoutOrder,
					VerticalFlex = Enum.UIFlexAlignment.Fill
				})
			end

			function Button:Add(Text, Callback, Disabled)
				local NewButton = {
					Disabled = false,
					OnPressed = nil
				}

				local SubItems = { } do
					SubItems["NewButton"] = Instances:Create("TextButton", {
						Parent = Items["Button"].Instance,
						Name = "\0",
						FontFace = Library.Font,
						TextColor3 = FromRGB(0, 0, 0),
						BorderColor3 = FromRGB(0, 0, 0),
						Text = "",
						AutoButtonColor = false,
						BorderSizePixel = 0,
						Size = UDim2New(0, 200, 0, 50),
						ZIndex = 2,
						TextSize = 14,
						BackgroundColor3 = FromRGB(36, 32, 39)
					})  SubItems["NewButton"]:AddToTheme({BackgroundColor3 = "Element"})

					Instances:Create("UICorner", {
						Parent = SubItems["NewButton"].Instance,
						Name = "\0",
						CornerRadius = UDimNew(0, 5)
					})

					Instances:Create("UIGradient", {
						Parent = SubItems["NewButton"].Instance,
						Name = "\0",
						Rotation = 90,
						Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
					}):AddToTheme({Color = function()
						return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
					end})

					SubItems["Text"] = Instances:Create("TextLabel", {
						Parent = SubItems["NewButton"].Instance,
						Name = "\0",
						FontFace = Library.Font,
						TextColor3 = FromRGB(255, 255, 255),
						BorderColor3 = FromRGB(0, 0, 0),
						Text = Text,
						BackgroundTransparency = 1,
						BorderSizePixel = 0,
						Size = UDim2New(1, 0, 1, 0),
						ZIndex = 2,
						TextSize = 14,
						BackgroundColor3 = FromRGB(255, 255, 255)
					})  SubItems["Text"]:AddToTheme({TextColor3 = "Text"})
				end

				function NewButton:Press()
					if self.Disabled then 
						return 
					end

					SubItems["NewButton"]:ChangeItemTheme({BackgroundColor3 = "Accent"})
					SubItems["NewButton"]:Tween(nil, {BackgroundColor3 = Library.Theme.Accent})
					task.wait(0.1)
					SubItems["NewButton"]:ChangeItemTheme({BackgroundColor3 = "Element"})
					SubItems["NewButton"]:Tween(nil, {BackgroundColor3 = Library.Theme.Element})

					if Callback then 
						Library:SafeCall(Callback)
					end

					if self.OnPressed then 
						self.OnPressed()
					end
				end

				function NewButton:SetText(Text)
					Text = tostring(Text)

					SubItems["Text"].Instance.Text = Text
				end

				function NewButton:SetVisible(Bool)
					SubItems["NewButton"].Instance.Visible = Bool
				end

				function NewButton:SetDisabled(Bool)
					self.Disabled = Bool

					if self.Disabled then 
						SubItems["NewButton"]:Tween(nil, {BackgroundTransparency = 0.6})
						SubItems["Text"]:Tween(nil, {TextTransparency = 0.6})
					else
						SubItems["NewButton"]:Tween(nil, {BackgroundTransparency = 0})
						SubItems["Text"]:Tween(nil, {TextTransparency = 0})
					end
				end

				local SearchData = {
					Name = Text,
					Item = SubItems["NewButton"]
				}

				local PageSearchData = Library.SearchItems[Data.Page]

				if not PageSearchData then 
					return 
				end

				TableInsert(PageSearchData, SearchData)

				SubItems["NewButton"]:Connect("MouseButton1Down", function()
					NewButton:Press()
				end)

				if Disabled then 
					NewButton:SetDisabled(Disabled)
				end

				return NewButton, SubItems 
			end

			function Button:SetVisible(Bool)
				Items["Button"].Instance.Visible = Bool
			end

			return Button, Items 
		end

		Components.Slider = function(Data)
			local Slider = {
				Value = 0,
				Flag = Data.Flag,
				Sliding = false,
				OnChanged = nil,
				Disabled = false
			}

			local Items = { } do
				Items["Slider"] = Instances:Create("Frame", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 0, 35),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Items["Text"] = Instances:Create("TextLabel", {
					Parent = Items["Slider"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = Data.Name,
					AutomaticSize = Enum.AutomaticSize.X,
					BackgroundTransparency = 1,
					Size = UDim2New(0, 0, 0, 15),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

				Items["RealSlider"] = Instances:Create("TextButton", {
					Parent = Items["Slider"].Instance,
					AutoButtonColor = false,
					Text = "",
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(0, 1),
					Position = UDim2New(0, 0, 1, 0),
					Size = UDim2New(1, 0, 0, 15),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(36, 32, 39)
				})  Items["RealSlider"]:AddToTheme({BackgroundColor3 = "Element"})

				Instances:Create("UICorner", {
					Parent = Items["RealSlider"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Instances:Create("UIGradient", {
					Parent = Items["RealSlider"].Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				Items["Accent"] = Instances:Create("Frame", {
					Parent = Items["RealSlider"].Instance,
					Name = "\0",
					Size = UDim2New(0.5, 0, 1, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(232, 186, 248)
				})  Items["Accent"]:AddToTheme({BackgroundColor3 = "Accent"})

				Instances:Create("UIGradient", {
					Parent = Items["Accent"].Instance,
					Name = "\0",
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(163, 163, 163))}
				})

				Instances:Create("UICorner", {
					Parent = Items["Accent"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Items["Drag"] = Instances:Create("Frame", {
					Parent = Items["Accent"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(1, 0.5),
					Position = UDim2New(1, 0, 0.5, 0),
					Size = UDim2New(0, 7, 1, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UICorner", {
					Parent = Items["Drag"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Items["Value"] = Instances:Create("TextLabel", {
					Parent = Items["Slider"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "50s",
					AutomaticSize = Enum.AutomaticSize.X,
					AnchorPoint = Vector2New(1, 0),
					Size = UDim2New(0, 0, 0, 15),
					BackgroundTransparency = 1,
					Position = UDim2New(1, 0, 0, 0),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Value"]:AddToTheme({TextColor3 = "Text"})
			end

			function Slider:Set(Value)
				if self.Disabled then 
					return 
				end

				self.Value = MathClamp(Library:Round(Value, Data.Decimals), Data.Min, Data.Max)

				Library.Flags[self.Flag] = self.Value

				Items["Accent"]:Tween(TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = UDim2New((self.Value - Data.Min) / (Data.Max - Data.Min), 0, 1, 0)})
				Items["Value"].Instance.Text = StringFormat("%s%s", tostring(self.Value), Data.Suffix)

				if Data.Callback then 
					Library:SafeCall(Data.Callback, self.Value)
				end

				if self.OnChanged then 
					self.OnChanged(self.Value)
				end
			end

			function Slider:SetText(Text)
				Text = tostring(Text)

				Items["Text"].Instance.Text = Text
			end

			function Slider:SetMin(Value)
				Data.Min = Value
			end

			function Slider:SetMax(Value)
				Data.Max = Value
			end

			function Slider:SetDisabled(Bool)
				self.Disabled = Bool

				if self.Disabled then
					for Index, Value in Items do 
						if Value.Instance:IsA("Frame") and Value.Instance.BackgroundColor3 ~= FromRGB(255, 255, 255) then
							Value:Tween(nil, {BackgroundTransparency = 0.6})
						elseif Value.Instance:IsA("TextLabel") then 
							Value:Tween(nil, {TextTransparency = 0.6})
						end
					end
				else
					for Index, Value in Items do 
						if Value.Instance:IsA("Frame") then
							Value:Tween(nil, {BackgroundTransparency = 0})
						elseif Value.Instance:IsA("TextLabel") then 
							Value:Tween(nil, {TextTransparency = 0})
						end
					end
				end
			end

			function Slider:SetVisible(Bool)
				Items["Slider"].Instance.Visible = Bool
			end

			function Slider:SetSuffix(Suffix)
				Suffix = tostring(Suffix)

				Data.Suffix = Suffix
			end

			local SearchData = {
				Name = Data.Name,
				Item = Items["Slider"]
			}

			local PageSearchData = Library.SearchItems[Data.Page]

			if not PageSearchData then 
				return 
			end

			TableInsert(PageSearchData, SearchData)

			Items["RealSlider"]:Connect("InputBegan", function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 or Input.UserInputType == Enum.UserInputType.Touch then
					Slider.Sliding = true 

					local SizeX = (Input.Position.X - Items["RealSlider"].Instance.AbsolutePosition.X) / Items["RealSlider"].Instance.AbsoluteSize.X
					local Value = ((Data.Max - Data.Min) * SizeX) + Data.Min

					Slider:Set(Value)

					Input.Changed:Connect(function()
						if Input.UserInputState == Enum.UserInputState.End then
							Slider.Sliding = false
						end
					end)
				end
			end)

			Library:Connect(UserInputService.InputChanged, function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseMovement or Input.UserInputType == Enum.UserInputType.Touch then
					if Slider.Sliding then
						local SizeX = (Input.Position.X - Items["RealSlider"].Instance.AbsolutePosition.X) / Items["RealSlider"].Instance.AbsoluteSize.X
						local Value = ((Data.Max - Data.Min) * SizeX) + Data.Min

						Slider:Set(Value)
					end
				end
			end)

			if Data.Disabled then 
				Slider:SetDisabled(Data.Disabled)
			end

			if Data.Default then 
				Slider:Set(Data.Default)
			end

			Library.SetFlags[Slider.Flag] = function(Value)
				Slider:Set(Value)
			end

			return Slider, Items 
		end

		Components.Dropdown = function(Data)
			local Dropdown = {
				Value = { },
				Flag = Data.Flag,
				IsOpen = false,
				Disabled = false,
				OnChanged = nil,
				Options = { }
			}

			local Items = { } do
				Items["Dropdown"] = Instances:Create("Frame", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 0, 25),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Items["Text"] = Instances:Create("TextLabel", {
					Parent = Items["Dropdown"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = Data.Name,
					AutomaticSize = Enum.AutomaticSize.X,
					AnchorPoint = Vector2New(0, 0.5),
					Size = UDim2New(0, 0, 0, 15),
					BackgroundTransparency = 1,
					Position = UDim2New(0, 0, 0.5, 0),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

				Items["RealDropdown"] = Instances:Create("TextButton", {
					Parent = Items["Dropdown"].Instance,
					Text = "",
					AutoButtonColor = false,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(1, 0),
					Position = UDim2New(1, 0, 0, 0),
					Size = UDim2New(0, not IsMobile and 135 or 75, 0, 25),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(36, 32, 39)
				})  Items["RealDropdown"]:AddToTheme({BackgroundColor3 = "Element"})

				Instances:Create("UICorner", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Instances:Create("UIGradient", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				Items["Value"] = Instances:Create("TextLabel", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "--",
					TextTruncate = Enum.TextTruncate.AtEnd,
					Size = UDim2New(1, -25, 0, 15),
					AnchorPoint = Vector2New(0, 0.5),
					Position = UDim2New(0, 8, 0.5, 0),
					BackgroundTransparency = 1,
					TextXAlignment = Enum.TextXAlignment.Left,
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Value"]:AddToTheme({TextColor3 = "Text"})

				Items["Icon"] = Instances:Create("ImageLabel", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					ImageColor3 = FromRGB(232, 186, 248),
					ScaleType = Enum.ScaleType.Fit,
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(0, 25, 0, 25),
					AnchorPoint = Vector2New(1, 0.5),
					Image = "rbxassetid://96215562143920",
					BackgroundTransparency = 1,
					Position = UDim2New(1, -1, 0.5, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Icon"]:AddToTheme({ImageColor3 = "Accent"})

				Items["OptionHolder"] = Instances:Create("ScrollingFrame", {
					Parent = Library.Holder.Instance,
					Name = "\0",
					Visible = false,
					Active = true,
					AutomaticCanvasSize = Enum.AutomaticSize.Y,
					AnchorPoint = Vector2New(0, 0),
					ZIndex = 12,
					BorderSizePixel = 0,
					CanvasSize = UDim2New(0, 0, 0, 0),
					ScrollBarImageColor3 = FromRGB(232, 186, 248),
					MidImage = "rbxassetid://128693616966482",
					BorderColor3 = FromRGB(0, 0, 0),
					ScrollBarThickness = 1,
					TopImage = "rbxassetid://128693616966482",
					Size = UDim2New(0, 135, 0, 125),
					BottomImage = "rbxassetid://128693616966482",
					Position = UDim2New(0, Items["RealDropdown"].Instance.AbsolutePosition.X, 0, Items["RealDropdown"].Instance.AbsolutePosition.Y + 30),
					BackgroundColor3 = FromRGB(15, 12, 16)
				})  Items["OptionHolder"]:AddToTheme({ScrollBarImageColor3 = "Accent", BackgroundColor3 = "Background"})

				Instances:Create("UICorner", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Instances:Create("UIPadding", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					PaddingTop = UDimNew(0, 5),
					PaddingBottom = UDimNew(0, 8),
					PaddingRight = UDimNew(0, 5),
					PaddingLeft = UDimNew(0, 5)
				})

				Instances:Create("UIListLayout", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					Padding = UDimNew(0, 5),
					SortOrder = Enum.SortOrder.LayoutOrder
				})
			end

			function Dropdown:Set(Option)
				if Data.Multi then
					if type(Option) ~= "table" then
						return
					end

					self.Value = Option
					Library.Flags[self.Flag] = Option

					for Index, Value in Option do
						local OptionData = self.Options[Value]

						if not OptionData then
							continue
						end

						OptionData.Selected = true
						OptionData:Toggle("Active")
					end

					local TextToDisplay = #self.Value == 0 and "--" or TableConcat(self.Value, ", ")
					Items["Value"].Instance.Text = TextToDisplay
				else
					local OptionData = self.Options[Option]

					if not OptionData then
						return
					end

					for Index, Value in self.Options do 
						if Value ~= OptionData then 
							Value.Selected = false
							Value:Toggle("Inactive")
						else
							Value.Selected = true
							Value:Toggle("Active")
						end
					end

					self.Value = OptionData.Name
					Library.Flags[self.Flag] = OptionData.Name

					Items["Value"].Instance.Text = OptionData.Name 
				end

				if Data.Callback then
					Library:SafeCall(Data.Callback, self.Value)
				end

				if self.OnChanged then 
					self.OnChanged(self.Value)
				end
			end

			function Dropdown:Get()
				return self.Value
			end

			function Dropdown:Add(Option, Icon)
				local OptionButton = Instances:Create(Data.IsLabelDropdown and "TextLabel" or "TextButton", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					BackgroundTransparency = 1,
					BorderSizePixel = 0,
					Size = UDim2New(1, 0, 0, 25),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(22, 20, 24)
				})  OptionButton:AddToTheme({BackgroundColor3 = "Inline"})

				if not Data.IsLabelDropdown then
					OptionButton.Instance.AutoButtonColor = false
				end

				Instances:Create("UICorner", {
					Parent = OptionButton.Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				local OptionText = Instances:Create("TextLabel", {
					Parent = OptionButton.Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					TextTransparency = 0.4,
					Text = Option,
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, -15, 1, 0),
					Position = UDim2New(0, Icon and 32 or 4, 0, 0),
					BackgroundTransparency = 1,
					TextXAlignment = Enum.TextXAlignment.Left,
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  OptionText:AddToTheme({TextColor3 = "Text"})

				if Icon then
					local OptionIcon = Instances:Create("ImageLabel", {
						Parent = OptionButton.Instance,
						Name = "\0",
						BorderColor3 = FromRGB(0, 0, 0),
						Size = UDim2New(0, 16, 0, 16),
						AnchorPoint = Vector2New(0, 0.5),
						Image = "rbxassetid://"..Icon,
						BackgroundTransparency = 1,
						Position = UDim2New(0, 8, 0.5, 0),
						ZIndex = 2,
						BorderSizePixel = 0,
						BackgroundColor3 = FromRGB(255, 255, 255)
					})
				end

				local OptionData = {
					Selected = false,
					Name = Option,
					Text = OptionText,
					Button = OptionButton,
				}

				function OptionData:Toggle(State)
					if State == "Active" then
						OptionData.Button:Tween(nil, {BackgroundTransparency = 0})
						OptionData.Text:Tween(nil, {TextTransparency = 0, Position = UDim2New(0, not Icon and 8 or 32, 0, 0)})
					else
						OptionData.Button:Tween(nil, {BackgroundTransparency = 1})
						OptionData.Text:Tween(nil, {TextTransparency = 0.4, Position = UDim2New(0, not Icon and 4 or 32, 0, 0)})
					end
				end

				function OptionData:Set()
					if Dropdown.Disabled then
						return
					end

					self.Selected = not self.Selected

					if Data.Multi then 
						local Index = TableFind(Dropdown.Value, self.Name)

						if Index then 
							TableRemove(Dropdown.Value, Index)
						else
							TableInsert(Dropdown.Value, self.Name)
						end

						Library.Flags[Dropdown.Flag] = Dropdown.Value

						self:Toggle(Index and "Inactive" or "Active")

						local TextToDisplay = #Dropdown.Value == 0 and "--" or TableConcat(Dropdown.Value, ", ")
						Items["Value"].Instance.Text = TextToDisplay
					else
						if self.Selected then 
							for Index, Value in Dropdown.Options do 
								if Value ~= self then 
									Value.Selected = false
									Value:Toggle("Inactive")
								end
							end

							self:Toggle("Active")

							Library.Flags[Dropdown.Flag] = self.Name
							Dropdown.Value = self.Name

							Items["Value"].Instance.Text = self.Name
						else
							self:Toggle("Inactive")

							Library.Flags[Dropdown.Flag] = nil
							Dropdown.Value = nil

							Items["Value"].Instance.Text = "--"
						end
					end

					if Data.Callback then 
						Library:SafeCall(Data.Callback, Dropdown.Value)
					end

					if Dropdown.OnChanged then
						Dropdown.OnChanged(Dropdown.Value)
					end
				end

				if not Data.IsLabelDropdown then
					OptionData.Button:Connect("MouseButton1Down", function()
						OptionData:Set()
					end)
				end

				self.Options[Option] = OptionData
				return OptionData
			end

			function Dropdown:Remove(Option)
				if self.Options[Option] then
					self.Options[Option].Button:Clean()
					self.Options[Option] = nil
				end
			end

			function Dropdown:Clear()
				for Index, Value in self.Options do
					self:Remove(Value.Name)
				end
			end

			function Dropdown:Refresh(List)
				Dropdown:Clear()

				for Index, Value in List do 
					Dropdown:Add(Value)
				end
			end

			function Dropdown:SetMulti(Bool)
				Data.Multi = Bool
			end

			function Dropdown:SetText(Text)
				Items["Text"].Instance.Text = Text
			end

			function Dropdown:SetDisabled(Bool)
				self.Disabled = Bool

				if self.Disabled then 
					Items["Text"]:Tween(nil, {TextTransparency = 0.6})
					Items["RealDropdown"]:Tween(nil, {BackgroundTransparency = 0.6})
					Items["Value"]:Tween(nil, {TextTransparency = 0.6})
					Items["Icon"]:Tween(nil, {ImageTransparency = 0.6})

					self:SetOpen(false)
				else
					Items["Text"]:Tween(nil, {TextTransparency = 0})
					Items["RealDropdown"]:Tween(nil, {BackgroundTransparency = 0})
					Items["Value"]:Tween(nil, {TextTransparency = 0})
					Items["Icon"]:Tween(nil, {ImageTransparency = 0})
				end
			end



			local Debounce = false
			local RenderStepped

			function Dropdown:SetOpen(Bool)

				if not self or not Items or not Items["OptionHolder"] or not Items["OptionHolder"].Instance then
					self.IsOpen = false
					return
				end
				if Debounce then return end

				self.IsOpen = not not Bool
				Debounce = true

				local holder = Items["OptionHolder"].Instance
				local icon   = Items["Icon"]
				local btn    = Items["RealDropdown"] and Items["RealDropdown"].Instance

				if self.IsOpen then
					holder.Visible = true
					if icon and icon.Tween then icon:Tween(nil, {Rotation = -90}) end

					RenderStepped = RunService.RenderStepped:Connect(function()

						if holder and holder.Parent and btn then
							holder.Position = UDim2New(0, btn.AbsolutePosition.X, 0, btn.AbsolutePosition.Y + 30)
						end
					end)


					for _, v in pairs(Library.OpenFrames) do
						if v ~= self and v.Options and type(v.SetOpen) == "function" then
							pcall(v.SetOpen, v, false)
						end
					end
					Library.OpenFrames[self] = self
				else
					if icon and icon.Tween then icon:Tween(nil, {Rotation = 0}) end
					if Library.OpenFrames[self] then Library.OpenFrames[self] = nil end
					if RenderStepped then RenderStepped:Disconnect(); RenderStepped = nil end
				end


				if not holder or not holder.Parent then
					Debounce = false
					return
				end

				local Descendants = holder:GetDescendants()
				table.insert(Descendants, holder)

				local NewTween
				for _, Object in ipairs(Descendants) do
					local TransparencyProperty = Tween:GetProperty(Object)
					if TransparencyProperty and not string.find(Object.ClassName, "UI") then
						Object.ZIndex = self.IsOpen and 1000 or 0
						if type(TransparencyProperty) == "table" then
							for _, Property in ipairs(TransparencyProperty) do
								NewTween = Tween:FadeItem(Object, Property, self.IsOpen, Data.Window.FadeSpeed)
							end
						else
							NewTween = Tween:FadeItem(Object, TransparencyProperty, self.IsOpen, Data.Window.FadeSpeed)
						end
					end
				end


				if NewTween and NewTween.Tween then
					Library:Connect(NewTween.Tween.Completed, function()
						Debounce = false
						if holder and holder.Parent then
							holder.Visible = self.IsOpen
						end
					end)
				else
					Debounce = false
					if holder and holder.Parent then
						holder.Visible = self.IsOpen
					end
				end
			end


			function Dropdown:SetVisible(Bool)
				Items["Dropdown"].Instance.Visible = Bool
			end

			local SearchData = {
				Name = Data.Name,
				Item = Items["Dropdown"]
			}

			local PageSearchData = Library.SearchItems[Data.Page]

			if not PageSearchData then 
				return 
			end

			TableInsert(PageSearchData, SearchData)

			Items["RealDropdown"]:Connect("MouseButton1Down", function()
				if not Dropdown.Disabled then
					Dropdown:SetOpen(not Dropdown.IsOpen)
				end
			end)

			Library:Connect(UserInputService.InputBegan, function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 then
					if Library:IsMouseOverFrame(Items["OptionHolder"]) then
						return
					end

					if Debounce then 
						return
					end

					Dropdown:SetOpen(false)
				end
			end)

			if Data.Disabled then 
				Dropdown:SetDisabled(Data.Disabled)
			end

			for Index, Value in Data.Items do 
				Dropdown:Add(Value)
			end

			if Data.Default then 
				Dropdown:Set(Data.Default)
			end

			Library.SetFlags[Dropdown.Flag] = function(Value)
				Dropdown:Set(Value)
			end

			return Dropdown, Items 
		end

		Components.ToggleDropdown = function(Data)
			local Dropdown = {
				Value = { },
				Flag = Data.Flag,
				IsOpen = false,
				Disabled = false,
				OnChanged = nil,
				Options = { }
			}

			local Items = { } do
				Items["Dropdown"] = Instances:Create("Frame", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 0, 25),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Items["Text"] = Instances:Create("TextLabel", {
					Parent = Items["Dropdown"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = Data.Name,
					AutomaticSize = Enum.AutomaticSize.X,
					AnchorPoint = Vector2New(0, 0.5),
					Size = UDim2New(0, 0, 0, 15),
					BackgroundTransparency = 1,
					Position = UDim2New(0, 0, 0.5, 0),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

				Items["RealDropdown"] = Instances:Create("TextButton", {
					Parent = Items["Dropdown"].Instance,
					Text = "",
					AutoButtonColor = false,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(1, 0),
					Position = UDim2New(1, 0, 0, 0),
					Size = UDim2New(0, not IsMobile and 135 or 75, 0, 25),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(36, 32, 39)
				})  Items["RealDropdown"]:AddToTheme({BackgroundColor3 = "Element"})

				Instances:Create("UICorner", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Instances:Create("UIGradient", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				Items["Value"] = Instances:Create("TextLabel", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "--",
					TextTruncate = Enum.TextTruncate.AtEnd,
					Size = UDim2New(1, -25, 0, 15),
					AnchorPoint = Vector2New(0, 0.5),
					Position = UDim2New(0, 8, 0.5, 0),
					BackgroundTransparency = 1,
					TextXAlignment = Enum.TextXAlignment.Left,
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Value"]:AddToTheme({TextColor3 = "Text"})

				Items["Icon"] = Instances:Create("ImageLabel", {
					Parent = Items["RealDropdown"].Instance,
					Name = "\0",
					ImageColor3 = FromRGB(232, 186, 248),
					ScaleType = Enum.ScaleType.Fit,
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(0, 25, 0, 25),
					AnchorPoint = Vector2New(1, 0.5),
					Image = "rbxassetid://96215562143920",
					BackgroundTransparency = 1,
					Position = UDim2New(1, -1, 0.5, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Icon"]:AddToTheme({ImageColor3 = "Accent"})

				Items["OptionHolder"] = Instances:Create("ScrollingFrame", {
					Parent = Library.Holder.Instance,
					Name = "\0",
					Visible = false,
					Active = true,
					AutomaticCanvasSize = Enum.AutomaticSize.Y,
					AnchorPoint = Vector2New(0, 0),
					ZIndex = 12,
					BorderSizePixel = 0,
					CanvasSize = UDim2New(0, 0, 0, 0),
					ScrollBarImageColor3 = FromRGB(232, 186, 248),
					MidImage = "rbxassetid://128693616966482",
					BorderColor3 = FromRGB(0, 0, 0),
					ScrollBarThickness = 1,
					TopImage = "rbxassetid://128693616966482",
					Size = UDim2New(0, 135, 0, 125),
					BottomImage = "rbxassetid://128693616966482",
					Position = UDim2New(0, Items["RealDropdown"].Instance.AbsolutePosition.X, 0, Items["RealDropdown"].Instance.AbsolutePosition.Y + 30),
					BackgroundColor3 = FromRGB(15, 12, 16)
				})  Items["OptionHolder"]:AddToTheme({ScrollBarImageColor3 = "Accent", BackgroundColor3 = "Background"})

				Instances:Create("UICorner", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Instances:Create("UIPadding", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					PaddingTop = UDimNew(0, 5),
					PaddingBottom = UDimNew(0, 8),
					PaddingRight = UDimNew(0, 5),
					PaddingLeft = UDimNew(0, 5)
				})

				Instances:Create("UIListLayout", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					Padding = UDimNew(0, 0),
					SortOrder = Enum.SortOrder.LayoutOrder
				})
			end

			function Dropdown:Set(Option)
				if Data.Multi then
					if type(Option) ~= "table" then
						return
					end

					self.Value = Option
					Library.Flags[self.Flag] = Option

					for Index, Value in Option do
						local OptionData = self.Options[Value]

						if not OptionData then
							continue
						end

						OptionData.Selected = true
						OptionData:Toggle("Active")
					end

					local TextToDisplay = #self.Value == 0 and "--" or TableConcat(self.Value, ", ")
					Items["Value"].Instance.Text = TextToDisplay
				else
					local OptionData = self.Options[Option]

					if not OptionData then
						return
					end

					for Index, Value in self.Options do 
						if Value ~= OptionData then 
							Value.Selected = false
							Value:Toggle("Inactive")
						else
							Value.Selected = true
							Value:Toggle("Active")
						end
					end

					self.Value = OptionData.Name
					Library.Flags[self.Flag] = OptionData.Name

					Items["Value"].Instance.Text = OptionData.Name 
				end

				if Data.Callback then
					Library:SafeCall(Data.Callback, self.Value)
				end

				if self.OnChanged then 
					self.OnChanged(self.Value)
				end
			end

			function Dropdown:Get()
				return self.Value
			end

			function Dropdown:Add(Option, Icon)
				local OptionButton = Instances:Create("TextButton", {
					Parent = Items["OptionHolder"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					BackgroundTransparency = 1,
					BorderSizePixel = 0,
					Size = UDim2New(1, 0, 0, 25),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(22, 20, 24)
				})  OptionButton:AddToTheme({BackgroundColor3 = "Inline"})

				Instances:Create("UICorner", {
					Parent = OptionButton.Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				local OptionText = Instances:Create("TextLabel", {
					Parent = OptionButton.Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					TextTransparency = 0.4,
					Text = Option,
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, -15, 1, 0),
					Position = UDim2New(0, not Icon and 4 or 32, 0, 0),
					BackgroundTransparency = 1,
					TextXAlignment = Enum.TextXAlignment.Left,
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  OptionText:AddToTheme({TextColor3 = "Text"})

				local OptionIndicator = Instances:Create("Frame", {
					Parent = OptionButton.Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(1, 0.5),
					Position = UDim2New(1, 0, 0.5, 0),
					Size = UDim2New(0, 18, 0, 18),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(36, 32, 39)
				})  OptionIndicator:AddToTheme({BackgroundColor3 = "Element"})

				Instances:Create("UICorner", {
					Parent = OptionIndicator.Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Instances:Create("UIGradient", {
					Parent = OptionIndicator.Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				local OptionCheck = Instances:Create("ImageLabel", {
					Parent = OptionIndicator.Instance,
					Name = "\0",
					ImageColor3 = FromRGB(0, 0, 0),
					ScaleType = Enum.ScaleType.Fit,
					ImageTransparency = 1,
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, -2, 1, -2),
					AnchorPoint = Vector2New(0.5, 0.5),
					Image = "rbxassetid://116339777575852",
					BackgroundTransparency = 1,
					Position = UDim2New(0.5, 0, 0.5, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				if Icon then
					local OptionIcon = Instances:Create("ImageLabel", {
						Parent = OptionButton.Instance,
						Name = "\0",
						BorderColor3 = FromRGB(0, 0, 0),
						Size = UDim2New(0, 16, 0, 16),
						AnchorPoint = Vector2New(0, 0.5),
						Image = "rbxassetid://"..Icon,
						BackgroundTransparency = 1,
						Position = UDim2New(0, 8, 0.5, 0),
						ZIndex = 2,
						BorderSizePixel = 0,
						BackgroundColor3 = FromRGB(255, 255, 255)
					})
				end

				local OptionData = {
					Selected = false,
					Name = Option,
					Text = OptionText,
					Button = OptionButton,

					Indicator = OptionIndicator,
					CheckImage = OptionCheck
				}

				function OptionData:Toggle(State)
					if State == "Active" then
						OptionData.Text:Tween(nil, {TextTransparency = 0})

						OptionData.Indicator:ChangeItemTheme({BackgroundColor3 = "Accent"})
						OptionData.Indicator:Tween(nil, {BackgroundColor3 = Library.Theme.Accent})
						OptionData.CheckImage:Tween(nil, {ImageTransparency = 0})
					else
						OptionData.Text:Tween(nil, {TextTransparency = 0.4})

						OptionData.Indicator:ChangeItemTheme({BackgroundColor3 = "Element"})
						OptionData.Indicator:Tween(nil, {BackgroundColor3 = Library.Theme.Element})
						OptionData.CheckImage:Tween(nil, {ImageTransparency = 1})
					end
				end

				function OptionData:Set()
					if Dropdown.Disabled then
						return
					end

					self.Selected = not self.Selected

					if Data.Multi then 
						local Index = TableFind(Dropdown.Value, self.Name)

						if Index then 
							TableRemove(Dropdown.Value, Index)
						else
							TableInsert(Dropdown.Value, self.Name)
						end

						Library.Flags[Dropdown.Flag] = Dropdown.Value

						self:Toggle(Index and "Inactive" or "Active")

						local TextToDisplay = #Dropdown.Value == 0 and "--" or TableConcat(Dropdown.Value, ", ")
						Items["Value"].Instance.Text = TextToDisplay
					else
						if self.Selected then 
							for Index, Value in Dropdown.Options do 
								if Value ~= self then 
									Value.Selected = false
									Value:Toggle("Inactive")
								end
							end

							self:Toggle("Active")

							Library.Flags[Dropdown.Flag] = self.Name
							Dropdown.Value = self.Name

							Items["Value"].Instance.Text = self.Name
						else
							self:Toggle("Inactive")

							Library.Flags[Dropdown.Flag] = nil
							Dropdown.Value = nil

							Items["Value"].Instance.Text = "--"
						end
					end

					if Data.Callback then 
						Library:SafeCall(Data.Callback, Dropdown.Value)
					end

					if Dropdown.OnChanged then
						Dropdown.OnChanged(Dropdown.Value)
					end
				end

				OptionData.Button:Connect("MouseButton1Down", function()
					OptionData:Set()
				end)

				self.Options[Option] = OptionData
				return OptionData
			end

			function Dropdown:Remove(Option)
				if self.Options[Option] then
					self.Options[Option].Button:Clean()
					self.Options[Option] = nil
				end
			end

			function Dropdown:Clear()
				for Index, Value in self.Options do
					self:Remove(Value.Name)
				end
			end

			function Dropdown:Refresh(List)
				Dropdown:Clear()

				for Index, Value in List do 
					Dropdown:Add(Value)
				end
			end

			function Dropdown:SetMulti(Bool)
				Data.Multi = Bool
			end

			function Dropdown:SetText(Text)
				Items["Text"].Instance.Text = Text
			end

			function Dropdown:SetDisabled(Bool)
				self.Disabled = Bool

				if self.Disabled then 
					Items["Text"]:Tween(nil, {TextTransparency = 0.6})
					Items["RealDropdown"]:Tween(nil, {BackgroundTransparency = 0.6})
					Items["Value"]:Tween(nil, {TextTransparency = 0.6})
					Items["Icon"]:Tween(nil, {ImageTransparency = 0.6})

					self:SetOpen(false)
				else
					Items["Text"]:Tween(nil, {TextTransparency = 0})
					Items["RealDropdown"]:Tween(nil, {BackgroundTransparency = 0})
					Items["Value"]:Tween(nil, {TextTransparency = 0})
					Items["Icon"]:Tween(nil, {ImageTransparency = 0})
				end
			end

			local Debounce = false 
			local RenderStepped

			function Dropdown:SetOpen(Bool)
				if Debounce then 
					return 
				end

				self.IsOpen = Bool

				Debounce = true

				if self.IsOpen then 
					Items["OptionHolder"].Instance.Visible = true 
					Items["Icon"]:Tween(nil, {Rotation = -90})

					RenderStepped = RunService.RenderStepped:Connect(function()
						Items["OptionHolder"].Instance.Position = UDim2New(0, Items["RealDropdown"].Instance.AbsolutePosition.X, 0, Items["RealDropdown"].Instance.AbsolutePosition.Y + 30)
					end)

					for Index, Value in Library.OpenFrames do 
						if Value ~= self and Value.Options then
							Value:SetOpen(false)
						end
					end

					Library.OpenFrames[self] = self
				else
					Items["Icon"]:Tween(nil, {Rotation = 0})

					if Library.OpenFrames[self] then 
						Library.OpenFrames[self] = nil
					end

					if RenderStepped then 
						RenderStepped:Disconnect()
						RenderStepped = nil
					end
				end

				local Descendants = Items["OptionHolder"].Instance:GetDescendants()
				TableInsert(Descendants, Items["OptionHolder"].Instance)

				local NewTween

				for _, Object in Descendants do 
					local TransparencyProperty = Tween:GetProperty(Object)

					if not TransparencyProperty then 
						continue
					end

					if StringFind(Object.ClassName, "UI") then
						continue
					end

					Object.ZIndex = self.IsOpen and 16 or 0

					if type(TransparencyProperty) == "table" then 
						for _, Property in TransparencyProperty do 
							NewTween = Tween:FadeItem(Object, Property, self.IsOpen, Data.Window.FadeSpeed)
						end
					else
						NewTween = Tween:FadeItem(Object, TransparencyProperty, self.IsOpen, Data.Window.FadeSpeed)
					end
				end

				Library:Connect(NewTween.Tween.Completed, function()
					Debounce = false
					Items["OptionHolder"].Instance.Visible = self.IsOpen
				end)
			end

			function Dropdown:SetVisible(Bool)
				Items["Dropdown"].Instance.Visible = Bool
			end

			local SearchData = {
				Name = Data.Name,
				Item = Items["Dropdown"]
			}

			local PageSearchData = Library.SearchItems[Data.Page]

			if not PageSearchData then 
				return 
			end

			TableInsert(PageSearchData, SearchData)

			Items["RealDropdown"]:Connect("MouseButton1Down", function()
				if not Dropdown.Disabled then
					Dropdown:SetOpen(not Dropdown.IsOpen)
				end
			end)

			Library:Connect(UserInputService.InputBegan, function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 then
					if Library:IsMouseOverFrame(Items["OptionHolder"]) then
						return
					end

					if Debounce then 
						return
					end

					Dropdown:SetOpen(false)
				end
			end)

			if Data.Disabled then 
				Dropdown:SetDisabled(Data.Disabled)
			end

			if Data.Default then 
				Dropdown:Set(Data.Default)
			end

			for Index, Value in Data.Items do 
				Dropdown:Add(Value)
			end

			Library.SetFlags[Dropdown.Flag] = function(Value)
				Dropdown:Set(Value)
			end

			return Dropdown, Items 
		end

		Components.Colorpicker = function(Data)
			local Colorpicker = {
				IsOpen = false,

				Color = FromRGB(0, 0, 0),
				HexValue = "000000",
				Alpha = 0,

				Hue = 0,
				Saturation = 0,
				Value = 0,

				Flag = Data.Flag,
				Disabled = false,
				OnChanged = nil
			}

			local AnimationsDropdown 
			local AnimationsDropdownItems

			local HSVTextbox 
			local HSVTextboxItems

			Library.Flags[Colorpicker.Flag] = { }

			local Items = { } do
				Items["ColorpickerButton"] = Instances:Create("TextButton", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					AnchorPoint = Vector2New(1, 0),
					BorderSizePixel = 0,
					Position = UDim2New(1, 0, 0, 0),
					Size = UDim2New(0, 20, 0, 20),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 215, 160)
				})

				Instances:Create("UIGradient", {
					Parent = Items["ColorpickerButton"].Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				Instances:Create("UICorner", {
					Parent = Items["ColorpickerButton"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["ColorpickerWindow"] = Instances:Create("Frame", {
					Parent = Library.Holder.Instance,
					Name = "\0",
					BackgroundTransparency = 0.30000001192092896,
					Position = UDim2New(0, Items["ColorpickerButton"].Instance.AbsolutePosition.X + 50, 0, Items["ColorpickerButton"].Instance.AbsolutePosition.Y + 26),
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(0, 218, 0, 275),
					Visible = false,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(15, 12, 16)
				})  Items["ColorpickerWindow"]:AddToTheme({BackgroundColor3 = "Background"})

				Items["ColorpickerWindow"]:MakeDraggable()
				Items["ColorpickerWindow"]:MakeResizeable(Vector2New(200, 200), Vector2New(9999, 9999))

				Instances:Create("UICorner", {
					Parent = Items["ColorpickerWindow"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["Hue"] = Instances:Create("ImageButton", {
					Parent = Items["ColorpickerWindow"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, -16, 0, 18),
					AutoButtonColor = false,
					AnchorPoint = Vector2New(0, 1),
					Image = Library:GetImage("Hue"),
					Position = UDim2New(0, 8, 1, -75),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UICorner", {
					Parent = Items["Hue"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["HueDragger"] = Instances:Create("Frame", {
					Parent = Items["Hue"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(0, 0.5),
					Position = UDim2New(0, 12, 0.5, 0),
					Size = UDim2New(0, 2, 1, -10),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UIStroke", {
					Parent = Items["HueDragger"].Instance,
					Name = "\0",
					Thickness = 1.2000000476837158,
					ApplyStrokeMode = Enum.ApplyStrokeMode.Border
				})

				Instances:Create("UICorner", {
					Parent = Items["HueDragger"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Items["Alpha"] = Instances:Create("TextButton", {
					Parent = Items["ColorpickerWindow"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					AnchorPoint = Vector2New(1, 0),
					BorderSizePixel = 0,
					Position = UDim2New(1, -8, 0, 8),
					Size = UDim2New(0, 18, 1, -110),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 215, 160)
				})

				Instances:Create("UICorner", {
					Parent = Items["Alpha"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["AlphaDragger"] = Instances:Create("Frame", {
					Parent = Items["Alpha"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(0.5, 0),
					Position = UDim2New(0.5, 0, 0, 3),
					Size = UDim2New(1, -10, 0, 2),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UIStroke", {
					Parent = Items["AlphaDragger"].Instance,
					Name = "\0",
					Thickness = 1.2000000476837158,
					ApplyStrokeMode = Enum.ApplyStrokeMode.Border
				})

				Instances:Create("UICorner", {
					Parent = Items["AlphaDragger"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Items["Checkers"] = Instances:Create("ImageLabel", {
					Parent = Items["Alpha"].Instance,
					Name = "\0",
					ScaleType = Enum.ScaleType.Tile,
					BorderColor3 = FromRGB(0, 0, 0),
					TileSize = UDim2New(0, 6, 0, 6),
					Image = Library:GetImage("Checkers"),
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 1, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UIGradient", {
					Parent = Items["Checkers"].Instance,
					Name = "\0",
					Rotation = 90,
					Transparency = NumSequence{NumSequenceKeypoint(0, 1), NumSequenceKeypoint(0.37, 0.5), NumSequenceKeypoint(1, 0)}
				})

				Instances:Create("UICorner", {
					Parent = Items["Checkers"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["Palette"] = Instances:Create("TextButton", {
					Parent = Items["ColorpickerWindow"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					BorderSizePixel = 0,
					Position = UDim2New(0, 9, 0, 8),
					Size = UDim2New(1, -44, 1, -110),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 215, 160)
				})

				Instances:Create("UICorner", {
					Parent = Items["Palette"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["Saturation"] = Instances:Create("ImageLabel", {
					Parent = Items["Palette"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					Image = Library:GetImage("Saturation"),
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 1, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UICorner", {
					Parent = Items["Saturation"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["Value"] = Instances:Create("ImageLabel", {
					Parent = Items["Palette"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, 2, 1, 0),
					Image = Library:GetImage("Value"),
					BackgroundTransparency = 1,
					Position = UDim2New(0, -1, 0, 0),
					ZIndex = 3,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UICorner", {
					Parent = Items["Value"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["PaletteDragger"] = Instances:Create("Frame", {
					Parent = Items["Palette"].Instance,
					Name = "\0",
					Size = UDim2New(0, 4, 0, 4),
					Position = UDim2New(0, 5, 0, 5),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UICorner", {
					Parent = Items["PaletteDragger"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(1, 0)
				})

				Instances:Create("UIStroke", {
					Parent = Items["PaletteDragger"].Instance,
					Name = "\0",
					Thickness = 1.2000000476837158,
					ApplyStrokeMode = Enum.ApplyStrokeMode.Border
				})

				Items["Shadow"] = Instances:Create("ImageLabel", {
					Parent = Items["ColorpickerWindow"].Instance,
					Name = "\0",
					ImageColor3 = FromRGB(0, 0, 0),
					ImageTransparency = 0.5600000023841858,
					AnchorPoint = Vector2New(0.5, 0.5),
					Image = "rbxassetid://112971167999062",
					ZIndex = -1,
					BorderSizePixel = 0,
					SliceCenter = RectNew(Vector2New(112, 112), Vector2New(147, 147)),
					ScaleType = Enum.ScaleType.Slice,
					BorderColor3 = FromRGB(0, 0, 0),
					BackgroundTransparency = 1,
					Position = UDim2New(0.5, 0, 0.5, 0),
					SliceScale = 0.6000000238418579,
					Size = UDim2New(1, 55, 1, 55),
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Shadow"]:AddToTheme({ImageColor3 = "Shadow"})

				AnimationsDropdown, AnimationsDropdownItems = Components.Dropdown({
					Name = "Animations",
					Parent = Items["ColorpickerWindow"],
					Disabled = false,
					Window = Data.Window,
					Items = {"Rainbow", "Breathing"},
					Multi = true,
					Page = Data.Page,
					Flag = "AnimationsDropdown"..Colorpicker.Flag,
					ZIndex = 2,
				})

				AnimationsDropdownItems["Dropdown"].Instance.AnchorPoint = Vector2New(0, 1)
				AnimationsDropdownItems["Dropdown"].Instance.Position = UDim2New(0, 8, 1, -38)
				AnimationsDropdownItems["Dropdown"].Instance.Size = UDim2New(1, -16, 0, 25)
				AnimationsDropdownItems["OptionHolder"].Instance.Size = UDim2New(0, AnimationsDropdownItems["RealDropdown"].Instance.AbsoluteSize.X, 0, 68)

				HSVTextbox, HSVTextboxItems = Components.Textbox({
					Name = "",
					Flag = Library:NextFlag(),
					Parent = Items["ColorpickerWindow"],
					Page = Data.Page,
					Debounce = true,
					Placeholder = "Enter hex code",
					Disabled = false
				})

				HSVTextboxItems["Text"]:Clean()
				HSVTextboxItems["Background"].Instance.Parent = Items["ColorpickerWindow"].Instance 
				HSVTextboxItems["Textbox"]:Clean()

				HSVTextboxItems["Background"].Instance.AnchorPoint = Vector2New(0, 1)
				HSVTextboxItems["Background"].Instance.Position = UDim2New(0, 8, 1, -8)
				HSVTextboxItems["Background"].Instance.Size = UDim2New(1, -16, 0, 25)
			end

			local OldColor = Colorpicker.Color
			local OldAlpha = Colorpicker.Alpha

			AnimationsDropdown.OnChanged = function(Value)
				if TableFind(Value, "Rainbow") then 
					OldColor = Colorpicker.Color

					Library:Thread(function()
						while task.wait() do 
							local RainbowHue = MathAbs(MathSin(tick() * 0.32))
							local Color = FromHSV(RainbowHue, 1, 1)

							Colorpicker:Set(Color, Colorpicker.Alpha)

							if not TableFind(Value, "Rainbow") then
								Colorpicker:Set(OldColor, Colorpicker.Alpha)
								break
							end
						end
					end)
				end

				if TableFind(Value, "Breathing") then 
					Library:Thread(function()
						OldAlpha = Colorpicker.Alpha
						while task.wait() do 
							local AlphaValue = MathAbs(MathSin(tick() * 0.8))

							Colorpicker:Set(Colorpicker.Color, AlphaValue)

							if not TableFind(Value, "Breathing") then
								Colorpicker:Set(Colorpicker.Color, OldAlpha)
								break
							end
						end
					end)
				end
			end

			HSVTextbox.OnChanged = function(Value)
				Colorpicker:Set(tostring(Value), Colorpicker.Alpha)
			end

			local Debounce = false

			local SlidingPallette = false
			local SlidingHue = false
			local SlidingAlpha = false

			function Colorpicker:SetOpen(Bool)
				if Debounce then 
					return 
				end

				self.IsOpen = Bool 

				Debounce = true

				if self.IsOpen then 
					Items["ColorpickerWindow"].Instance.Visible = true
					Items["ColorpickerWindow"].Instance.Position = UDim2New(0, Items["ColorpickerButton"].Instance.AbsolutePosition.X + 50, 0, Items["ColorpickerButton"].Instance.AbsolutePosition.Y + 26)

					for Index, Value in Library.OpenFrames do 
						if Value ~= self and Value.Hue then
							Value:SetOpen(false)
						end
					end

					Library.OpenFrames[self] = self
				else
					if Library.OpenFrames[self] then 
						Library.OpenFrames[self] = nil
					end
				end

				local Descendants = Items["ColorpickerWindow"].Instance:GetDescendants()
				TableInsert(Descendants, Items["ColorpickerWindow"].Instance)

				local NewTween

				for _, Object in Descendants do 
					local TransparencyProperty = Tween:GetProperty(Object)

					if not TransparencyProperty then 
						continue
					end

					Items["AlphaDragger"].Instance.ZIndex = self.IsOpen and 999 or 1
					Items["PaletteDragger"].Instance.ZIndex = self.IsOpen and 999 or 1

					if not StringFind(Object.ClassName, "UI") and not (Object:IsA("ImageLabel") and Object.Image == "rbxassetid://112971167999062") then
						Object.ZIndex = self.IsOpen and 999 or 1
					end

					if type(TransparencyProperty) == "table" then 
						for _, Property in TransparencyProperty do 
							NewTween = Tween:FadeItem(Object, Property, self.IsOpen, Data.Window.FadeSpeed)
						end
					else
						NewTween = Tween:FadeItem(Object, TransparencyProperty, self.IsOpen, Data.Window.FadeSpeed)
					end
				end

				Library:Connect(NewTween.Tween.Completed, function()
					Debounce = false
					Items["ColorpickerWindow"].Instance.Visible = self.IsOpen
				end)
			end

			function Colorpicker:SlidePalette(Input)
				if not Input or not SlidingPallette then 
					return 
				end

				local ValueX = MathClamp(1 - (Input.Position.X - Items["Palette"].Instance.AbsolutePosition.X) / Items["Palette"].Instance.AbsoluteSize.X, 0, 1)
				local ValueY = MathClamp(1 - (Input.Position.Y - Items["Palette"].Instance.AbsolutePosition.Y) / Items["Palette"].Instance.AbsoluteSize.Y, 0, 1)

				self.Saturation = ValueX
				self.Value = ValueY

				local SlideX = MathClamp((Input.Position.X - Items["Palette"].Instance.AbsolutePosition.X) / Items["Palette"].Instance.AbsoluteSize.X, 0, 0.98)
				local SlideY = MathClamp((Input.Position.Y - Items["Palette"].Instance.AbsolutePosition.Y) / Items["Palette"].Instance.AbsoluteSize.Y, 0, 0.97)

				Items["PaletteDragger"]:Tween(TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2New(SlideX, 0, SlideY, 0)})
				self:Update()
			end

			function Colorpicker:SlideHue(Input)
				if not Input or not SlidingHue then 
					return 
				end

				local ValueX = MathClamp((Input.Position.X - Items["Hue"].Instance.AbsolutePosition.X) / Items["Hue"].Instance.AbsoluteSize.X, 0, 1)

				self.Hue = ValueX

				local SlideX = MathClamp((Input.Position.X - Items["Hue"].Instance.AbsolutePosition.X) / Items["Hue"].Instance.AbsoluteSize.X, 0, 0.99)

				Items["HueDragger"]:Tween(TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2New(SlideX, 0, 0.5, 0)})
				self:Update()
			end

			function Colorpicker:SlideAlpha(Input)
				if not Input or not SlidingAlpha then 
					return 
				end

				local ValueY = MathClamp((Input.Position.Y - Items["Alpha"].Instance.AbsolutePosition.Y) / Items["Alpha"].Instance.AbsoluteSize.Y, 0, 1)

				self.Alpha = ValueY 

				local SlideY = MathClamp((Input.Position.Y - Items["Alpha"].Instance.AbsolutePosition.Y) / Items["Alpha"].Instance.AbsoluteSize.Y, 0, 0.99)

				Items["AlphaDragger"]:Tween(TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2New(0.5, 0, SlideY, 0)})
				self:Update(true)
			end

			function Colorpicker:Update(IsFromAlpha)
				local Hue, Saturation, Value = self.Hue, self.Saturation, self.Value
				local Color = FromHSV(Hue, Saturation, Value)

				self.Color = Color
				self.HexValue = Color:ToHex()

				Library.Flags[self.Flag] = {
					Alpha = self.Alpha,
					Color = self.HexValue
				}

				Items["ColorpickerButton"]:Tween(nil, {BackgroundColor3 = Color})
				Items["Palette"]:Tween(nil, {BackgroundColor3 = FromHSV(Hue, 1, 1)})
				HSVTextboxItems["Input"].Instance.Text = "#" .. self.HexValue

				if not IsFromAlpha then 
					Items["Alpha"]:Tween(nil, {BackgroundColor3 = Color})
				end

				if Data.Callback then 
					Library:SafeCall(Data.Callback, Color, self.Alpha)
				end

				if Colorpicker.OnChanged then 
					Colorpicker.OnChanged(Color, self.Alpha)
				end
			end

			function Colorpicker:Set(Color, Alpha)
				if type(Color) == "table" then
					Color = FromRGB(Color[1], Color[2], Color[3])
					Alpha = Color[4]
				elseif type(Color) == "string" then
					Color = FromHex(Color)
				end 

				self.Hue, self.Saturation, self.Value = Color:ToHSV()
				self.Alpha = Alpha or 0

				local ColorPositionX = MathClamp(1 - self.Saturation, 0, 0.98)
				local ColorPositionY = MathClamp(1 - self.Value, 0, 0.97)

				local AlphaPositionY = MathClamp(self.Alpha, 0, 0.99)

				local HuePositionX = MathClamp(self.Hue, 0, 0.99)

				Items["PaletteDragger"]:Tween(TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2New(ColorPositionX, 0, ColorPositionY, 0)})
				Items["HueDragger"]:Tween(TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2New(HuePositionX, 0, 0.5, 0)})
				Items["AlphaDragger"]:Tween(TweenInfo.new(0.22, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2New(0.5, 0, AlphaPositionY, 0)})
				self:Update()
			end

			Items["ColorpickerButton"]:Connect("MouseButton1Down", function()
				Colorpicker:SetOpen(not Colorpicker.IsOpen)
			end)

			Items["Palette"]:Connect("InputBegan", function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 or Input.UserInputType == Enum.UserInputType.Touch then
					SlidingPallette = true
					Colorpicker:SlidePalette(Input)

					Input.Changed:Connect(function()
						if Input.UserInputState == Enum.UserInputState.End then 
							SlidingPallette = false
						end
					end)
				end
			end)

			Library:Connect(UserInputService.InputBegan, function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 then
					if Library:IsMouseOverFrame(AnimationsDropdownItems["OptionHolder"]) then
						return
					end

					if Library:IsMouseOverFrame(Items["ColorpickerWindow"]) then
						return
					end

					if Debounce then 
						return
					end

					Colorpicker:SetOpen(false)
				end
			end)

			Items["Hue"]:Connect("InputBegan", function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 or Input.UserInputType == Enum.UserInputType.Touch then
					SlidingHue = true
					Colorpicker:SlideHue(Input)

					Input.Changed:Connect(function()
						if Input.UserInputState == Enum.UserInputState.End then 
							SlidingHue = false
						end
					end)
				end
			end)

			Items["Alpha"]:Connect("InputBegan", function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseButton1 or Input.UserInputType == Enum.UserInputType.Touch then
					SlidingAlpha = true
					Colorpicker:SlideAlpha(Input)

					Input.Changed:Connect(function()
						if Input.UserInputState == Enum.UserInputState.End then 
							SlidingAlpha = false
						end
					end)
				end
			end)

			Library:Connect(UserInputService.InputChanged, function(Input)
				if Input.UserInputType == Enum.UserInputType.MouseMovement or Input.UserInputType == Enum.UserInputType.Touch then
					if SlidingPallette then
						Colorpicker:SlidePalette(Input)
					end

					if SlidingHue then
						Colorpicker:SlideHue(Input)
					end

					if SlidingAlpha then
						Colorpicker:SlideAlpha(Input)
					end
				end
			end)

			if Data.Default then 
				Colorpicker:Set(Data.Default, Data.Alpha)
			end

			Library.SetFlags[Colorpicker.Flag] = function(Value, Alpha)
				Colorpicker:Set(Value, Alpha)
			end

			return Colorpicker, Items 
		end

		Components.Keybind = function(Data)
			local Keybind = {
				Flag = Data.Flag,

				IsOpen = false,

				Key = nil,
				Value = "",
				Mode = "",

				Toggled = false,
				Picking = false,
				OnChanged = nil
			}

			Library.Flags[Keybind.Flag] = { }

			local CalculateCount

			local KeylistItem 

			if Library.KeyList then 
				KeylistItem = Library.KeyList:Add("", "", "")
			end

			local Items = { } do
				Items["KeyButton"] = Instances:Create("TextButton", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "None",
					Size = UDim2New(0, 0, 0, 20),
					AutoButtonColor = false,
					AnchorPoint = Vector2New(1, 0),
					AutomaticSize = Enum.AutomaticSize.X,
					Position = UDim2New(1, 0, 0, 0),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(15, 12, 16)
				})  Items["KeyButton"]:AddToTheme({TextColor3 = "Text", BackgroundColor3 = "Background"})

				CalculateCount = function(Index)
					local MaxButtonsAdded = 5

					local Column = Index % MaxButtonsAdded

					local Offset = Data.IsToggle and 44 or Data.IsCheckbox and 24 or 0

					local ButtonSize = Items["KeyButton"].Instance.AbsoluteSize
					local Spacing = -6

					local XPosition = (ButtonSize.X + Spacing) * Column - Spacing - ButtonSize.X

					Items["KeyButton"].Instance.Position = UDim2New(1, -XPosition - Offset, 0, 0)
				end

				CalculateCount(Data.Count)

				Instances:Create("UIPadding", {
					Parent = Items["KeyButton"].Instance,
					Name = "\0",
					PaddingRight = UDimNew(0, 5),
					PaddingLeft = UDimNew(0, 6)
				})

				Instances:Create("UICorner", {
					Parent = Items["KeyButton"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["KeybindWindow"] = Instances:Create("Frame", {
					Parent = Library.Holder.Instance,
					Name = "\0",
					Visible = false,
					Position = UDim2New(0, Items["KeyButton"].Instance.AbsolutePosition.X, 0, Items["KeyButton"].Instance.AbsolutePosition.Y + 25),
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(0, 90, 0, 90),
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(15, 12, 16)
				})  Items["KeybindWindow"]:AddToTheme({BackgroundColor3 = "Background"})

				Instances:Create("UICorner", {
					Parent = Items["KeybindWindow"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["Shadow"] = Instances:Create("ImageLabel", {
					Parent = Items["KeybindWindow"].Instance,
					Name = "\0",
					ImageColor3 = FromRGB(0, 0, 0),
					ImageTransparency = 0.5600000023841858,
					AnchorPoint = Vector2New(0.5, 0.5),
					Image = "rbxassetid://112971167999062",
					ZIndex = -1,
					BorderSizePixel = 0,
					SliceCenter = RectNew(Vector2New(112, 112), Vector2New(147, 147)),
					ScaleType = Enum.ScaleType.Slice,
					BorderColor3 = FromRGB(0, 0, 0),
					BackgroundTransparency = 1,
					Position = UDim2New(0.5, 0, 0.5, 0),
					SliceScale = 0.6000000238418579,
					Size = UDim2New(1, 55, 1, 55),
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Shadow"]:AddToTheme({ImageColor3 = "Shadow"})

				Items["Toggle"] = Instances:Create("TextButton", {
					Parent = Items["KeybindWindow"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					BorderSizePixel = 0,
					Position = UDim2New(0, 8, 0, 8),
					Size = UDim2New(1, -16, 0, 25),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(22, 20, 24)
				})  Items["Toggle"]:AddToTheme({BackgroundColor3 = "Inline"})

				Instances:Create("UICorner", {
					Parent = Items["Toggle"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["ToggleText"] = Instances:Create("TextLabel", {
					Parent = Items["Toggle"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "Toggle",
					BorderSizePixel = 0,
					BackgroundTransparency = 1,
					Position = UDim2New(0, 8, 0, 0),
					Size = UDim2New(1, -15, 1, 0),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["ToggleText"]:AddToTheme({TextColor3 = "Text"})

				Items["Hold"] = Instances:Create("TextButton", {
					Parent = Items["KeybindWindow"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					BorderSizePixel = 0,
					BackgroundTransparency = 1,
					Position = UDim2New(0, 8, 0, 33),
					Size = UDim2New(1, -16, 0, 25),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(22, 20, 24)
				})  Items["Hold"]:AddToTheme({BackgroundColor3 = "Inline"})

				Instances:Create("UICorner", {
					Parent = Items["Hold"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["HoldText"] = Instances:Create("TextLabel", {
					Parent = Items["Hold"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					TextTransparency = 0.4000000059604645,
					Text = "Hold",
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, -15, 1, 0),
					BackgroundTransparency = 1,
					Position = UDim2New(0, 4, 0, 0),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["HoldText"]:AddToTheme({TextColor3 = "Text"})

				Items["Always"] = Instances:Create("TextButton", {
					Parent = Items["KeybindWindow"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(0, 0, 0),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = "",
					AutoButtonColor = false,
					BorderSizePixel = 0,
					BackgroundTransparency = 1,
					Position = UDim2New(0, 8, 0, 58),
					Size = UDim2New(1, -16, 0, 25),
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(22, 20, 24)
				})  Items["Always"]:AddToTheme({BackgroundColor3 = "Inline"})

				Instances:Create("UICorner", {
					Parent = Items["Always"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["AlwaysText"] = Instances:Create("TextLabel", {
					Parent = Items["Always"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					TextTransparency = 0.4000000059604645,
					Text = "Always",
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(1, -15, 1, 0),
					BackgroundTransparency = 1,
					Position = UDim2New(0, 4, 0, 0),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["AlwaysText"]:AddToTheme({TextColor3 = "Text"})
			end

			local Modes = {
				["Toggle"] = {Items["Toggle"], Items["ToggleText"]},
				["Hold"] = {Items["Hold"], Items["HoldText"]},
				["Always"] = {Items["Always"], Items["AlwaysText"]}
			}

			local Update = function()
				if KeylistItem then 
					KeylistItem:Set(Keybind.Value, Data.Name, Keybind.Mode)
					KeylistItem:SetStatus(Keybind.Toggled)
				end
			end

			function Keybind:Set(Key)
				if StringFind(tostring(Key), "Enum") then 
					self.Key = tostring(Key)

					Key = Key.Name == "Backspace" and "None" or Key.Name

					local KeyString = Keys[self.Key] or StringGSub(Key, "Enum.", "") or "None"
					local TextToDisplay = StringGSub(StringGSub(KeyString, "KeyCode.", ""), "UserInputType.", "") or "None"

					self.Value = TextToDisplay
					Items["KeyButton"].Instance.Text = TextToDisplay

					Library.Flags[Data.Flag] = {
						Mode = self.Mode,
						Key = self.Key,
						Toggled = self.Toggled
					}

					if Data.Callback then 
						Library:SafeCall(Data.Callback, self.Toggled)
					end

					if self.OnChanged then 
						self.OnChanged(self.Toggled)
					end

					Update()
				elseif type(Key) == "table" then
					local RealKey = Key.Key == "Backspace" and "None" or Key.Key
					self.Key = tostring(Key.Key)

					if Key.Mode then
						self.Mode = Key.Mode
						self:SetMode(Key.Mode)
					else
						self.Mode = "toggle"
						self:SetMode("toggle")
					end

					local KeyString = Keys[self.Key] or StringGSub(tostring(RealKey), "Enum.", "") or RealKey
					local TextToDisplay = KeyString and StringGSub(StringGSub(KeyString, "KeyCode.", ""), "UserInputType.", "") or "None"

					TextToDisplay = StringGSub(StringGSub(KeyString, "KeyCode.", ""), "UserInputType.", "")

					self.Value = TextToDisplay
					Items["KeyButton"].Instance.Text = TextToDisplay

					if Data.Callback then 
						Library:SafeCall(Data.Callback, self.Toggled)
					end

					if self.OnChanged then 
						self.OnChanged(self.Toggled, self.Value)
					end

					Update()
				elseif TableFind({"toggle", "hold", "always"}, Key) then
					self.Mode = Key
					self:SetMode(Keybind.Mode)

					if Data.Callback then 
						Library:SafeCall(Data.Callback, self.Toggled)
					end

					if self.OnChanged then 
						self.OnChanged(self.Toggled, self.Value)
					end

					Update()
				end

				Items["KeyButton"]:ChangeItemTheme({TextColor3 = "Text", BackgroundColor3 = "Background"})
				Items["KeyButton"]:Tween(nil, {TextColor3 = Library.Theme.Text})
				--CalculateCount(Data.Count)
				self.Picking = false
			end

			function Keybind:SetMode(Mode)
				for Index, Value in Modes do 
					if Index == Mode then 
						Value[1]:Tween(nil, {BackgroundTransparency = 0})
						Value[2]:Tween(nil, {TextTransparency = 0})
					else
						Value[1]:Tween(nil, {BackgroundTransparency = 1})
						Value[2]:Tween(nil, {TextTransparency = 0.4})
					end
				end

				Library.Flags[Data.Flag] = {
					Mode = self.Mode,
					Key = self.Key,
					Toggled = self.Toggled
				}

				if Data.Callback then 
					Library:SafeCall(Data.Callback, self.Toggled)
				end

				if self.OnChanged then 
					self.OnChanged(self.Toggled, self.Value)
				end

				Update()
			end

			local Debounce = false

			function Keybind:SetOpen(Bool)
				if Debounce then 
					return
				end

				self.IsOpen = Bool 

				Debounce = true

				if self.IsOpen then
					Items["KeybindWindow"].Instance.Visible = true
					Items["KeybindWindow"].Instance.Position = UDim2New(0, Items["KeyButton"].Instance.AbsolutePosition.X, 0, Items["KeyButton"].Instance.AbsolutePosition.Y + 25)

					for Index, Value in Library.OpenFrames do
						if Value ~= self and Value.Key then 
							Value:SetOpen(false)
						end
					end

					Library.OpenFrames[self] = self
				else
					if Library.OpenFrames[self] then
						Library.OpenFrames[self] = nil
					end
				end

				local Descendants = Items["KeybindWindow"].Instance:GetDescendants()
				TableInsert(Descendants, Items["KeybindWindow"].Instance)

				local NewTween

				for _, Object in Descendants do 
					local TransparencyProperty = Tween:GetProperty(Object)

					if not TransparencyProperty then
						continue
					end

					if StringFind(Object.ClassName, "UI") then
						continue
					end

					Object.ZIndex = self.IsOpen and 15 or 1

					if type(TransparencyProperty) == "table" then 
						for _, Property in TransparencyProperty do 
							NewTween = Tween:FadeItem(Object, Property, self.IsOpen, Data.Window.FadeSpeed)
						end
					else
						NewTween = Tween:FadeItem(Object, TransparencyProperty, self.IsOpen, Data.Window.FadeSpeed)
					end
				end

				Library:Connect(NewTween.Tween.Completed, function()
					Debounce = false
					Items["KeybindWindow"].Instance.Visible = self.IsOpen
				end)
			end

			function Keybind:Press(Bool)
				if self.Mode == "Toggle" then 
					self.Toggled = not self.Toggled
				elseif self.Mode == "Hold" then 
					self.Toggled = Bool
				elseif self.Mode == "Always" then 
					self.Toggled = true
				end

				Library.Flags[Data.Flag] = {
					Mode = self.Mode,
					Key = self.Key,
					Toggled = self.Toggled
				}

				if Data.Callback then 
					Library:SafeCall(Data.Callback, self.Toggled)
				end

				if self.OnChanged then 
					self.OnChanged(self.Toggled, self.Value)
				end

				Update()
			end

			Items["KeyButton"]:Connect("MouseButton2Down", function()
				Keybind:SetOpen(not Keybind.IsOpen)
			end)

			Items["KeyButton"]:Connect("MouseButton1Click", function()
				if Keybind.Picking then 
					return
				end

				Keybind.Picking = true

				Items["KeyButton"]:ChangeItemTheme({TextColor3 = "Accent", BackgroundColor3 = "Background"})
				Items["KeyButton"]:Tween(nil, {TextColor3 = Library.Theme.Accent})

				local InputBegan 
				InputBegan = UserInputService.InputBegan:Connect(function(Input)
					if Input.UserInputType == Enum.UserInputType.Keyboard then 
						Keybind:Set(Input.KeyCode)
					else
						Keybind:Set(Input.UserInputType)
					end

					InputBegan:Disconnect()
					InputBegan = nil
				end)
			end)

			Items["Hold"]:Connect("MouseButton1Down", function()
				Keybind.Mode = "Hold"
				Keybind:SetMode("Hold")
			end)

			Items["Toggle"]:Connect("MouseButton1Down", function()
				Keybind.Mode = "Toggle"
				Keybind:SetMode("Toggle")
			end)

			Items["Always"]:Connect("MouseButton1Down", function()
				Keybind.Mode = "Always"
				Keybind:SetMode("Always")
			end)

			Library:Connect(UserInputService.InputBegan, function(Input)
				if tostring(Input.KeyCode) == Keybind.Key or tostring(Input.UserInputType) == Keybind.Key and not Keybind.Value == "None" then
					if Keybind.Mode == "Toggle" then 
						Keybind:Press()
					elseif Keybind.Mode == "Hold" then 
						Keybind:Press(true)
					elseif Keybind.Mode == "Always" then 
						Keybind:Press(true)
					end
				end
			end)

			Library:Connect(UserInputService.InputEnded, function(Input)
				if tostring(Input.KeyCode) == Keybind.Key or tostring(Input.UserInputType) == Keybind.Key and not Keybind.Value == "None"  then
					if Keybind.Mode == "Hold" then 
						Keybind:Press(false)
					elseif Keybind.Mode == "Always" then 
						Keybind:Press(true)
					end
				end
			end)

			if Data.Default then
				Keybind.Mode = Data.Mode or "Toggle"
				Keybind:SetMode(Keybind.Mode)
				Keybind:Set({Key = Data.Default, Mode = Data.Mode})
			end

			Library.SetFlags[Data.Flag] = function(Value)
				Keybind:Set(Value)
			end

			return Keybind, Items 
		end

		Components.Textbox = function(Data)
			local Textbox = {
				Value = "",
				Flag = Data.Flag,
				OnChanged = nil,
				Disabled = false
			}

			local Items = { } do
				Items["Textbox"] = Instances:Create("Frame", {
					Parent = Data.Parent.Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					Size = UDim2New(1, 0, 0, 46),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Items["Text"] = Instances:Create("TextLabel", {
					Parent = Items["Textbox"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					Text = Data.Name,
					AutomaticSize = Enum.AutomaticSize.X,
					BackgroundTransparency = 1,
					Size = UDim2New(0, 0, 0, 15),
					BorderSizePixel = 0,
					ZIndex = 2,
					TextSize = 14,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

				Items["Background"] = Instances:Create("Frame", {
					Parent = Items["Textbox"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					AnchorPoint = Vector2New(0, 1),
					Position = UDim2New(0, 0, 1, 0),
					Size = UDim2New(1, 0, 0, 25),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(36, 32, 39)
				})  Items["Background"]:AddToTheme({BackgroundColor3 = "Element"})

				Instances:Create("UIGradient", {
					Parent = Items["Background"].Instance,
					Name = "\0",
					Rotation = 90,
					Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(216, 216, 216))}
				}):AddToTheme({Color = function()
					return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme["Gradient"])}
				end})

				Instances:Create("UICorner", {
					Parent = Items["Background"].Instance,
					Name = "\0",
					CornerRadius = UDimNew(0, 5)
				})

				Items["Input"] = Instances:Create("TextBox", {
					Parent = Items["Background"].Instance,
					Name = "\0",
					FontFace = Library.Font,
					TextStrokeColor3 = FromRGB(255, 255, 255),
					PlaceholderColor3 = FromRGB(185, 185, 185),
					PlaceholderText = Data.Placeholder,
					TextSize = 14,
					Size = UDim2New(1, -16, 1, 0),
					TextColor3 = FromRGB(255, 255, 255),
					BorderColor3 = FromRGB(0, 0, 0),
					ClearTextOnFocus = false,
					Text = "",
					ZIndex = 2,
					BackgroundTransparency = 1,
					TextXAlignment = Enum.TextXAlignment.Left,
					CursorPosition = -1,
					Position = UDim2New(0, 8, 0, 0),
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["Input"]:AddToTheme({TextColor3 = "Text", PlaceholderColor3 = "Inactive Text"})
			end

			function Textbox:Set(Value)
				if self.Disabled then 
					return 
				end

				Items["Input"].Instance.Text = Value
				Library.Flags[self.Flag] = Value

				if Data.Callback then 
					Library:SafeCall(Data.Callback, Value)
				end

				if self.OnChanged then 
					Library:SafeCall(self.OnChanged, Value)
				end
			end

			function Textbox:SetText(Text)
				Items["Input"].Instance.Text = Text
			end

			function Textbox:SetDisabled(Bool)
				self.Disabled = Bool 

				if self.Disabled then 
					Items["Text"]:Tween(nil, {TextTransparency = 0.6})
					Items["Background"]:Tween(nil, {BackgroundTransparency = 0.6})
					Items["Input"]:Tween(nil, {TextTransparency = 0.6})
				else
					Items["Text"]:Tween(nil, {TextTransparency = 0})
					Items["Background"]:Tween(nil, {BackgroundTransparency = 0})
					Items["Input"]:Tween(nil, {TextTransparency = 0})
				end
			end

			function Textbox:SetVisible(Bool)
				Items["Textbox"].Instance.Visible = Bool
			end

			local SearchData = {
				Name = Data.Name,
				Item = Items["Textbox"]
			}

			local PageSearchData = Library.SearchItems[Data.Page]

			if not PageSearchData and not Data.Debounce then 
				return 
			end

			TableInsert(PageSearchData, SearchData)

			Items["Input"]:Connect("Focused", function()
				if Textbox.Disabled then 
					return
				end

				Items["Input"]:ChangeItemTheme({TextColor3 = "Accent", PlaceholderColor3 = "Inactive Text"})
				Items["Input"]:Tween(nil, {TextColor3 = Library.Theme.Accent})
			end)

			Items["Input"]:Connect("FocusLost", function()
				if Textbox.Disabled then 
					return
				end

				Items["Input"]:ChangeItemTheme({TextColor3 = "Text", PlaceholderColor3 = "Inactive Text"})
				Items["Input"]:Tween(nil, {TextColor3 = Library.Theme.Text})

				Textbox:Set(Items["Input"].Instance.Text)
			end)

			if Data.Disabled then 
				Textbox:SetDisabled(Data.Disabled)
			end

			if Data.Default then 
				Textbox:Set(Data.Default)
			end

			Library.SetFlags[Data.Flag] = function(Value)
				Textbox:Set(Value)
			end

			return Textbox, Items 
		end
	end

	Library.Notification = function(self, Text, Description, Duration)
		local Items = { } do
			Items["Notification"] = Instances:Create("Frame", {
				Parent = Library.NotifHolder.Instance,
				Name = "Notification",
				BackgroundTransparency = 0.06,
				AutomaticSize = Enum.AutomaticSize.Y,
				BackgroundColor3 = FromRGB(16, 16, 16),
				BorderSizePixel = 0,
				Size = UDim2New(0, 320, 0, 70)
			}) Items["Notification"]:AddToTheme({BackgroundColor3 = "Background"})

			Instances:Create("UICorner", {
				Parent = Items["Notification"].Instance,
				CornerRadius = UDimNew(0, 8)
			})

			Items["Stroke"] = Instances:Create("UIStroke", {
				Parent = Items["Notification"].Instance,
				Color = FromRGB(158, 114, 158),
				Transparency = 0.8
			})

			Items["Title"] = Instances:Create("TextLabel", {
				Parent = Items["Notification"].Instance,
				FontFace = Library.Font,
				Text = Text,
				TextColor3 = FromRGB(199, 199, 203),
				TextSize = 16,
				TextXAlignment = Enum.TextXAlignment.Left,
				BackgroundTransparency = 1,
				Size = UDim2New(1, -20, 0, 20),
				Position = UDim2New(0, 10, 0, 6)
			}) Items["Title"]:AddToTheme({TextColor3 = "Text"})

			Items["Description"] = Instances:Create("TextLabel", {
				Parent = Items["Notification"].Instance,
				FontFace = Library.Font,
				Text = Description,
				TextColor3 = FromRGB(180, 180, 185),
				TextSize = 14,
				TextXAlignment = Enum.TextXAlignment.Left,
				TextYAlignment = Enum.TextYAlignment.Top,
				BackgroundTransparency = 1,
				AutomaticSize = Enum.AutomaticSize.Y,
				TextWrapped = true,
				Size = UDim2New(1, -20, 0, 0),
				Position = UDim2New(0, 10, 0, 28)
			}) Items["Description"]:AddToTheme({TextColor3 = "Text"})

			Items["Duration"] = Instances:Create("Frame", {
				Parent = Items["Notification"].Instance,
				BackgroundColor3 = FromRGB(44, 38, 44),
				BorderSizePixel = 0,
				Size = UDim2New(1, -20, 0, 6),
				Position = UDim2New(0, 10, 1, -12)
			}) Items["Duration"]:AddToTheme({BackgroundColor3 = "Inline"})

			Items["Accent"] = Instances:Create("Frame", {
				Parent = Items["Duration"].Instance,
				BackgroundColor3 = FromRGB(255, 188, 254),
				BorderSizePixel = 0,
				Size = UDim2New(1, 0, 1, 0)
			}) Items["Accent"]:AddToTheme({BackgroundColor3 = "Accent"})

			Instances:Create("UICorner", {
				Parent = Items["Accent"].Instance
			})
		end

		Items["Accent"]:Tween(
			TweenInfo.new(Duration, Enum.EasingStyle.Linear, Enum.EasingDirection.Out),
			{Size = UDim2New(0, 0, 1, 0)})

		task.delay(Duration, function()
			Tween:Create(Items["Notification"].Instance, TweenInfo.new(0.3), {BackgroundTransparency = 1}, true)
			task.wait(0.3)
			Items["Notification"]:Clean()
		end)
	end

	Library.KeybindList = function(self)
		local KeybindList = { }
		self.KeyList = KeybindList

		local Items = { } do
			Items["KeybindList"] = Instances:Create("Frame", {
				Parent = Library.Holder.Instance,
				Name = "\0",
				BackgroundTransparency = 0.30000001192092896,
				Position = UDim2New(0, 15, 0.5, 0),
				BorderColor3 = FromRGB(0, 0, 0),
				BorderSizePixel = 0,
				AutomaticSize = Enum.AutomaticSize.XY,
				BackgroundColor3 = FromRGB(15, 12, 16)
			})  Items["KeybindList"]:AddToTheme({BackgroundColor3 = "Background"})

			Items["KeybindList"]:MakeDraggable()

			Instances:Create("UICorner", {
				Parent = Items["KeybindList"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Items["Title"] = Instances:Create("TextLabel", {
				Parent = Items["KeybindList"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = "Keybinds",
				BackgroundTransparency = 1,
				Size = UDim2New(0, 0, 0, 15),
				BorderSizePixel = 0,
				AutomaticSize = Enum.AutomaticSize.X,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Title"]:AddToTheme({TextColor3 = "Text"})

			Instances:Create("UIPadding", {
				Parent = Items["KeybindList"].Instance,
				Name = "\0",
				PaddingTop = UDimNew(0, 8),
				PaddingBottom = UDimNew(0, 8),
				PaddingRight = UDimNew(0, 8),
				PaddingLeft = UDimNew(0, 8)
			})

			Items["Content"] = Instances:Create("Frame", {
				Parent = Items["KeybindList"].Instance,
				Name = "\0",
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0, 20),
				BorderColor3 = FromRGB(0, 0, 0),
				BorderSizePixel = 0,
				AutomaticSize = Enum.AutomaticSize.XY,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Instances:Create("UIListLayout", {
				Parent = Items["Content"].Instance,
				Name = "\0",
				Padding = UDimNew(0, 4),
				SortOrder = Enum.SortOrder.LayoutOrder
			})

			Instances:Create("UIPadding", {
				Parent = Items["Content"].Instance,
				Name = "\0",
				PaddingRight = UDimNew(0, 5)
			})
		end

		function KeybindList:Add(Key, Name, Mode)
			local NewKey = Instances:Create("TextLabel", {
				Parent = Items["Content"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = ""..Key.." - ".. Name .. " (".. Mode .. ")",
				BackgroundTransparency = 1,
				Size = UDim2New(0, 0, 0, 20),
				BorderSizePixel = 0,
				AutomaticSize = Enum.AutomaticSize.X,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  NewKey:AddToTheme({TextColor3 = "Text"})

			Instances:Create("UIPadding", {
				Parent = NewKey.Instance,
				Name = "\0",
				PaddingRight = UDimNew(0, 3),
				PaddingLeft = UDimNew(0, 3)
			})

			function NewKey:Set(Key, Name, Mode)
				NewKey.Instance.Text = ""..Key.." - ".. Name .. " (".. Mode .. ")"
			end

			function NewKey:SetStatus(Bool)
				if Bool then 
					NewKey:ChangeItemTheme({TextColor3 = "Accent"})
					NewKey:Tween(nil, {TextColor3 = Library.Theme.Accent})
				else
					NewKey:ChangeItemTheme({TextColor3 = "Text"})
					NewKey:Tween(nil, {TextColor3 = Library.Theme.Text})
				end
			end

			return NewKey
		end

		function KeybindList:SetVisible(Bool)
			Items["KeybindList"].Instance.Visible = Bool
		end

		return KeybindList
	end

	Library.Watermark = function(self, Name)
		local Watermark = { }

		local Items = { } do
			Items["Watermark"] = Instances:Create("Frame", {
				Parent = Library.Holder.Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				AnchorPoint = Vector2New(0.5, 0),
				Position = UDim2New(0.5, 0, 0, 15),
				Size = UDim2New(0, 100, 0, 60),
				BorderSizePixel = 0,
				AutomaticSize = Enum.AutomaticSize.XY,
				BackgroundColor3 = FromRGB(16, 18, 21)
			})  Items["Watermark"]:AddToTheme({BackgroundColor3 = "Background"})

			Instances:Create("UIGradient", {
				Parent = Items["Watermark"].Instance,
				Name = "\0",
				Rotation = 84,
				Color = RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, FromRGB(211, 211, 211))}
			}):AddToTheme({Color = function()
				return RGBSequence{RGBSequenceKeypoint(0, FromRGB(255, 255, 255)), RGBSequenceKeypoint(1, Library.Theme.Gradient)}
			end})

			Instances:Create("UICorner", {
				Parent = Items["Watermark"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Items["Text"] = Instances:Create("TextLabel", {
				Parent = Items["Watermark"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = Name,
				Size = UDim2New(0, 0, 0, 0),
				AnchorPoint = Vector2New(0, 0.5),
				Position = UDim2New(0, 0, 0.5, 0),
				BackgroundTransparency = 1,
				TextXAlignment = Enum.TextXAlignment.Center,
				BorderSizePixel = 0,
				AutomaticSize = Enum.AutomaticSize.XY,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

			Instances:Create("UIPadding", {
				Parent = Items["Watermark"].Instance,
				Name = "\0",
				PaddingTop = UDimNew(0, 7),
				PaddingBottom = UDimNew(0, 7),
				PaddingRight = UDimNew(0, 7),
				PaddingLeft = UDimNew(0, 7)
			})
		end

		function Watermark:SetVisible(Bool)
			Items["Watermark"].Instance.Visible = Bool
		end

		local Frametimer = tick()
		local Framecount = 0

		local FramesPerSecond = 60

		Library:Connect(RunService.RenderStepped, function()
			Framecount += 1

			if tick() - Frametimer >= 1 then 
				FramesPerSecond = Framecount

				Frametimer = tick()
				Framecount = 0
			end

			Items["Text"].Instance.Text = string.format("%s\nFPS: %s | PING: %s ms", Name, tostring(FramesPerSecond), tostring(MathFloor(Stats.Network.ServerStatsItem["Data Ping"]:GetValue())))
		end)

		return Watermark
	end

	Library.Window = function(self, Properties)
		Properties = Properties or { }

		local Window = {
			Name = Properties.Name or Properties.name or "Astroxel",
			Size = Properties.Size or Properties.size or (not IsMobile and UDim2New(0, 770, 0, 526) or UDim2New(0, 526, 0, 350)),
			FadeSpeed = Properties.FadeSpeed or Properties.fadespeed or 0.25,
			BackgroundIcon = Properties.BackgroundIcon or Properties.backgroundicon or "rbxassetid://",

			Pages = { },
			IsOpen = false,

			Items = { }
		}

		local Items = { } do
			Items["MainFrame"] = Instances:Create("Frame", {
				Parent = Library.Holder.Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				AnchorPoint = Vector2New(0, 0),
				BackgroundTransparency = 0.3,
				Position = UDim2New(0, Camera.ViewportSize.X / 3.5, 0, Camera.ViewportSize.Y / 3.5),
				Size = Window.Size,
				ClipsDescendants = true,
				Visible = true,
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(15, 12, 16)
			})  Items["MainFrame"]:AddToTheme({BackgroundColor3 = "Background"})

			Items["MainFrame"]:MakeDraggable()
			Items["MainFrame"]:MakeResizeable(Vector2New(Window.Size.X.Offset, Window.Size.Y.Offset), Vector2New(9999, 9999))

			Items["ImageBackground"] = Instances:Create("ImageLabel", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				Image = Window.BackgroundIcon,
				BackgroundTransparency = 1,
				Size = UDim2New(1, 0, 1, 0),
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Instances:Create("UICorner", {
				Parent = Items["ImageBackground"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Instances:Create("UICorner", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Items["Shadow"] = Instances:Create("ImageLabel", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				ImageColor3 = FromRGB(0, 0, 0),
				ImageTransparency = 0.5600000023841858,
				AnchorPoint = Vector2New(0.5, 0.5),
				Image = "rbxassetid://112971167999062",
				ZIndex = -1,
				BorderSizePixel = 0,
				SliceCenter = RectNew(Vector2New(112, 112), Vector2New(147, 147)),
				ScaleType = Enum.ScaleType.Slice,
				BorderColor3 = FromRGB(0, 0, 0),
				BackgroundTransparency = 1,
				Position = UDim2New(0.5, 0, 0.5, 0),
				SliceScale = 0.6000000238418579,
				Size = UDim2New(1, 55, 1, 55),
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Shadow"]:AddToTheme({ImageColor3 = "Shadow"})

			Items["Title"] = Instances:Create("TextLabel", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = Window.Name,
				AutomaticSize = Enum.AutomaticSize.X,
				Size = UDim2New(0, 0, 0, 15),
				BackgroundTransparency = 1,
				Position = UDim2New(0, 9, 0, 8),
				BorderSizePixel = 0,
				ZIndex = 2,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Title"]:AddToTheme({TextColor3 = "Text"})

			Items["Pages"] = Instances:Create("Frame", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				BackgroundTransparency = 1,
				Position = UDim2New(0, 0, 0, 30),
				Size = not IsMobile and UDim2New(0, 224, 1, -30) or UDim2New(0, 150, 1, -30),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Instances:Create("UIPadding", {
				Parent = Items["Pages"].Instance,
				Name = "\0",
				PaddingRight = UDimNew(0, 8),
				PaddingLeft = UDimNew(0, 8)
			})

			Instances:Create("UIListLayout", {
				Parent = Items["Pages"].Instance,
				Name = "\0",
				Padding = UDimNew(0, 8),
				SortOrder = Enum.SortOrder.LayoutOrder
			})

			Items["Content"] = Instances:Create("Frame", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				BackgroundTransparency = 1,
				Position = not IsMobile and UDim2New(0, 225, 0, 30) or UDim2New(0, 163, 0, 30),
				Size = not IsMobile and UDim2New(1, -233, 1, -38) or UDim2New(1, -171, 1, -38),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Items["MinimizeButton"] = Instances:Create("ImageButton", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				Size = UDim2New(0, 17, 0, 17),
				AutoButtonColor = false,
				AnchorPoint = Vector2New(1, 0),
				Image = "rbxassetid://94817928404736",
				Position = UDim2New(1, -27, 0, 3),
				BackgroundTransparency = 1,
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Instances:Create("Frame", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				Size = UDim2New(0, 1, 1, 0),
				Position = not IsMobile and UDim2New(0, 220, 0, 0) or UDim2New(0, 152, 0, 0),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(41, 37, 45)
			}):AddToTheme({BackgroundColor3 = "Border"})

			Items["CloseButton"] = Instances:Create("ImageButton", {
				Parent = Items["MainFrame"].Instance,
				Name = "\0",
				ScaleType = Enum.ScaleType.Fit,
				BorderColor3 = FromRGB(0, 0, 0),
				Size = UDim2New(0, 17, 0, 17),
				AutoButtonColor = false,
				AnchorPoint = Vector2New(1, 0),
				Image = "rbxassetid://76001605964586",
				BackgroundTransparency = 1,
				Position = UDim2New(1, -8, 0, 8),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Items["Search"] = Instances:Create("Frame", {
				Parent = Items["Content"].Instance,
				Name = "\0",
				Size = UDim2New(1, 0, 0, 35),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(22, 20, 24)
			})  Items["Search"]:AddToTheme({BackgroundColor3 = "Inline"})

			Instances:Create("UICorner", {
				Parent = Items["Search"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Items["Icon"] = Instances:Create("ImageLabel", {
				Parent = Items["Search"].Instance,
				Name = "\0",
				ScaleType = Enum.ScaleType.Fit,
				ImageTransparency = 0.4000000059604645,
				BorderColor3 = FromRGB(0, 0, 0),
				Size = UDim2New(0, 20, 0, 20),
				AnchorPoint = Vector2New(0, 0.5),
				Image = "rbxassetid://71924825350727",
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0.5, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Items["Input"] = Instances:Create("TextBox", {
				Parent = Items["Search"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				AnchorPoint = Vector2New(0, 0.5),
				PlaceholderColor3 = FromRGB(185, 185, 185),
				PlaceholderText = "Search..",
				TextSize = 14,
				Size = UDim2New(1, -43, 0, 15),
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = "",
				BackgroundTransparency = 1,
				TextXAlignment = Enum.TextXAlignment.Left,
				ZIndex = 2,
				Position = UDim2New(0, 35, 0.5, 0),
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Input"]:AddToTheme({TextColor3 = "Text", PlaceholderColor3 = "Inactive Text"})

			-- Items["Cursor"] = Instances:Create("Frame", {
			-- 	Parent = Library.Holder.Instance,
			-- 	Name = "\0",
			-- 	BackgroundTransparency = 1,
			-- 	Position = UDim2New(0, 0, 0, 0),
			-- 	BorderColor3 = FromRGB(0, 0, 0),
			-- 	Size = UDim2New(0, 16, 0, 16),
			-- 	BorderSizePixel = 0,
			-- 	BackgroundColor3 = FromRGB(0, 0, 0)
			-- })

			-- Items["Image"] = Instances:Create("ImageLabel", {
			-- 	Parent = Items["Cursor"].Instance,
			-- 	Name = "\0",
			-- 	ImageColor3 = FromRGB(232, 186, 248),
			-- 	BorderColor3 = FromRGB(0, 0, 0),
			-- 	Image = "rbxassetid://132511743665753",
			-- 	BackgroundTransparency = 1,
			-- 	Size = UDim2New(0, 16, 0, 16),
			-- 	BorderSizePixel = 0,
			-- 	ZIndex = 10001,
			-- 	Rotation = -90,
			-- 	BackgroundColor3 = FromRGB(232, 186, 248)
			-- })  Items["Image"]:AddToTheme({ImageColor3 = "Accent"})

			if IsMobile then 
				Items["FloatingButton"] = Instances:Create("TextButton", {
					Parent = Library.Holder.Instance,
					Text = "",
					AutoButtonColor = false,
					Name = "\0",
					Position = UDim2New(0, 125, 0, 125),
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(0, 50, 0, 50),
					BorderSizePixel = 0,
					ZIndex = 127,
					BackgroundColor3 = Library.Theme.Background
				})  Items["FloatingButton"]:AddToTheme({BackgroundColor3 = "Background"})

				Items["FloatingButton"]:MakeDraggable()

				Items["OpenTitle"] = Instances:Create("TextLabel", {
					Parent = Items["FloatingButton"].Instance,
					BorderColor3 = FromRGB(0, 0, 0),
					Name = "\0",
					Text = "Close",
					FontFace = Library.Font,
					TextColor3 = FromRGB(255, 255, 255),
					TextSize = 14,
					BackgroundTransparency = 1,
					AnchorPoint = Vector2New(0.5, 0.5),
					Position = UDim2New(0.5, 0, 0.5, 0),
					ZIndex = 127,
					Size = UDim2New(1, -10, 1, -10),
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})  Items["OpenTitle"]:AddToTheme({TextColor3 = "Text"})

				Instances:Create("UICorner", {
					Parent = Items["FloatingButton"].Instance,
					CornerRadius = UDimNew(1, 0)
				}) 

				Items["FloatingButton"]:Connect("MouseButton1Down", function(Input)
					IsOpen = not IsOpen
					task.wait()
					Window:SetOpen(IsOpen)
					Items["OpenTitle"].Instance.Text = IsOpen and "Open" or "Close"
				end)
			end

			Window.Items = Items
		end

		UserInputService.MouseIconEnabled = true

		local Debounce = false

		function Window:SetBackgroundTransparency(Value)
			Items["MainFrame"].Instance.BackgroundTransparency = Value
		end

		function Window:SetBackgroundImage(Value)
			Items["BackgroundImage"].Instance.Image = Value
		end

		local RenderStepped

		Items["Input"]:Connect("Focused", function()
			local PageSearchData = Library.SearchItems[Library.CurrentPage]

			if not PageSearchData then
				return 
			end

			RenderStepped = RunService.RenderStepped:Connect(function()
				for Index, Value in PageSearchData do 
					local Name = Value.Name
					local Element = Value.Item

					if StringFind(StringLower(Name), StringLower(Items["Input"].Instance.Text)) then
						if Items["Input"].Instance.Text ~= "" then 
							Element.Instance.Visible  = true 
						else
							Element.Instance.Visible  = true 
						end
					else
						Element.Instance.Visible = false
					end
				end
			end)
		end)

		Items["Input"]:Connect("FocusLost", function()
			if RenderStepped then 
				RenderStepped:Disconnect()
				RenderStepped = nil
			end
		end)

		local IsMinisize = false
		local IsOpen = false
		local OldSize = Items["MainFrame"].Instance.AbsoluteSize

		function Window:Minimize(Bool)
			IsMinisize = Bool

			if IsMinisize then 
				Items["MainFrame"]:Tween(nil, {Size = UDim2New(0, OldSize.X, 0, 35)})
				Items["MainFrame"]:Tween(nil, {Size = UDim2New(0, 275, 0, 35)})
			else
				Items["MainFrame"]:Tween(nil, {Size = UDim2New(0, OldSize.X, 0, OldSize.Y)})
			end
		end

		function Window:SetOpen(Bool)
			IsOpen = Bool

			if IsOpen then
				Items["MainFrame"].Instance.Visible = true
			else
				Items["MainFrame"].Instance.Visible = false
			end
		end

		Library:Connect(UserInputService.InputBegan, function(Input)
			if tostring(Input.KeyCode) == Library.MenuKeybind or tostring(Input.UserInputType) == Library.MenuKeybind then
				Window.IsOpen = not Window.IsOpen
				Items["Image"].Instance.Visible = Window.IsOpen
				UserInputService.MouseIconEnabled = true
				Items["MainFrame"].Instance.Visible = Window.IsOpen
			end
		end)

		-- Library:Connect(RunService.RenderStepped, function()
		-- 	local MouseLocation = UserInputService:GetMouseLocation() 
		-- 	Items["Cursor"].Instance.Position = UDim2New(0, MouseLocation.X - 1, 0, MouseLocation.Y - 56)           
		-- end)

		Items["MinimizeButton"]:Connect("MouseButton1Down", function()
			Window:Minimize(not IsMinisize)
		end)

		Items["CloseButton"]:Connect("MouseButton1Down", function()
			Items["MainFrame"].Instance.Visible = false
			task.wait(0.1)
			Library:Unload()
		end)

		return setmetatable(Window, self)
	end



	Library.Page = function(self, Properties)
		Properties = Properties or { }

		local Page = {
			Window = self,

			Name = Properties.Name or Properties.name or "Page",
			Icon = Properties.Icon or Properties.icon or '',
			Columns = Properties.Columns or Properties.columns or 2,

			Active = false,
			IsKeyPage = Properties.IsKeyPage or Properties.iskeypage or false,

			Items = { },
			ColumnsData = { }
		}

		local Items = { } do
			Items["Inactive"] = Instances:Create("TextButton", {
				Parent = Page.Window.Items["Pages"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(0, 0, 0),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = "",
				AutoButtonColor = false,
				BorderSizePixel = 0,
				BackgroundTransparency = 1,
				Size = UDim2New(1, 0, 0, 35),
				ClipsDescendants = true,
				ZIndex = 2,
				TextSize = 14,
				BackgroundColor3 = FromRGB(22, 20, 24),
			})  Items["Inactive"]:AddToTheme({BackgroundColor3 = "Inline"})
			
			Items["Icon"] = Instances:Create("ImageLabel", {
				Parent = Items["Inactive"].Instance,
				Name = "\0",
				Size = UDim2New(0, 20, 0, 20),
				AnchorPoint = Vector2New(0, 0.5),
				BackgroundTransparency = 1,
				ZIndex = 3,
				Position = UDim2New(0, 8, 0.5, 0),
				ImageTransparency = 0.4,
			})
			
			gl(Items["Icon"].Instance, Page.Icon)

			Instances:Create("UICorner", {
				Parent = Items["Inactive"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Items["Liner"] = Instances:Create("Frame", {
				Parent = Items["Inactive"].Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				AnchorPoint = Vector2New(0, 0.5),
				BackgroundTransparency = 1,
				Position = UDim2New(0, -3, 0.5, 0),
				Size = UDim2New(0, 3, 1, -20),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 174, 254)
			})  Items["Liner"]:AddToTheme({BackgroundColor3 = "Accent"})

			Instances:Create("UICorner", {
				Parent = Items["Liner"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(1, 0)
			})

			Items["Text"] = Instances:Create("TextLabel", {
				Parent = Items["Inactive"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				TextTransparency = 0.4,
				Text = Page.Name,
				AutomaticSize = Enum.AutomaticSize.X,
				Size = UDim2New(0, 0, 0, 15),
				AnchorPoint = Vector2New(0, 0.5),
				BorderSizePixel = 0,
				BackgroundTransparency = 1,
				Position = UDim2New(0, 32, 0.5, 0),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

			Items["Page"] = Instances:Create("Frame", {
				Parent = Page.Window.Items["Content"].Instance,
				Name = "\0",
				Visible = false,
				BackgroundTransparency = 1,
				Size = UDim2New(1, 0, 1, 0),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			if not Page.IsKeyPage then
				Items["Columns"] = Instances:Create("Frame", {
					Parent = Items["Page"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					BackgroundTransparency = 1,
					Position = UDim2New(0, 0, 0, 43),
					Size = UDim2New(1, 0, 1, -43),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				Instances:Create("UIListLayout", {
					Parent = Items["Columns"].Instance,
					Name = "\0",
					FillDirection = Enum.FillDirection.Horizontal,
					HorizontalFlex = Enum.UIFlexAlignment.Fill,
					Padding = UDimNew(0, 8),
					SortOrder = Enum.SortOrder.LayoutOrder,
					VerticalFlex = Enum.UIFlexAlignment.Fill
				})

				for Index = 1, Page.Columns do 
					local NewColumn = Instances:Create("ScrollingFrame", {
						Parent = Items["Columns"].Instance,
						Name = "\0",
						ScrollBarImageColor3 = FromRGB(0, 0, 0),
						Active = true,
						AutomaticCanvasSize = Enum.AutomaticSize.Y,
						ScrollBarThickness = 0,
						BorderColor3 = FromRGB(0, 0, 0),
						BackgroundTransparency = 1,
						Size = UDim2New(0, 100, 0, 100),
						BackgroundColor3 = FromRGB(255, 255, 255),
						ZIndex = 2,
						BorderSizePixel = 0,
						CanvasSize = UDim2New(0, 0, 0, 0)
					})

					Instances:Create("UIPadding", {
						Parent = NewColumn.Instance,
						Name = "\0",
						PaddingBottom = UDimNew(0, 8)
					})

					Instances:Create("UIListLayout", {
						Parent = NewColumn.Instance,
						Name = "\0",
						Padding = UDimNew(0, 8),
						SortOrder = Enum.SortOrder.LayoutOrder
					})

					Page.ColumnsData[Index] = NewColumn
				end
			else
				Items["Page"].Instance.Size = UDim2New(1, 0, 1, -43)
				Items["Page"].Instance.Position = UDim2New(0, 0, 0, 43)

				Instances:Create("UIListLayout", {
					Parent = Items["Page"].Instance,
					Name = "\0",
					VerticalAlignment = Enum.VerticalAlignment.Center,
					SortOrder = Enum.SortOrder.LayoutOrder,
					HorizontalAlignment = Enum.HorizontalAlignment.Center,
					Padding = UDimNew(0, 15)
				})
			end

			Page.Items = Items
		end

		if not Page.IsKeyPage then 
			Library.SearchItems[Page] = { }
		end

		local Debounce = false 

		function Page:Turn(Bool)
			self.Active = Bool
			Items["Page"].Instance.Parent = self.Active and Page.Window.Items["Content"].Instance or Library.UnusedHolder.Instance

			if self.Active then
				Items["Page"].Instance.Visible = true

				Items["Liner"]:Tween(nil, {BackgroundTransparency = 0, Size = UDim2New(0, 6, 1, -20)})
				Items["Inactive"]:Tween(nil, {BackgroundTransparency = 0})
				Items["Text"]:Tween(nil, {TextTransparency = 0, Position = UDim2New(0, 35, 0.5, 0)})
				Items["Icon"]:Tween(nil, {ImageTransparency = 0, Position = UDim2New(0, 12, 0.5, 0)})

				Library.CurrentPage = Page
			else
				Items["Liner"]:Tween(nil, {BackgroundTransparency = 1, Size = UDim2New(0, 3, 1, -20)})
				Items["Inactive"]:Tween(nil, {BackgroundTransparency = 1})
				Items["Text"]:Tween(nil, {TextTransparency = 0.4, Position = UDim2New(0, 32, 0.5, 0)})
				Items["Icon"]:Tween(nil, {ImageTransparency = 0.4, Position = UDim2New(0, 8, 0.5, 0)})
				
				Items["Page"].Instance.Visible = false
			end
		end

		function Page:AddKey(Text, Key, GetKeyLink, Callback)
			if not self.IsKeyPage then
				return 
			end

			local NewKey = {
				Text = Text,
				Key = Key,

				Success = false,
			}

			local InputTextbox, InputTextboxItems
			local Button, CheckKeyButton, GetKeyButton, ButtonItems

			local SubItems = { } do
				SubItems["NewKey"] = Instances:Create("Frame", {
					Parent = Items["Page"].Instance,
					Name = "\0",
					BackgroundTransparency = 1,
					Size = UDim2New(1, -125, 0, 80),
					BorderColor3 = FromRGB(0, 0, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})

				InputTextbox, InputTextboxItems = Components.Textbox({
					Name = "Enter key here..",
					Flag = Library:NextFlag(),
					Parent = SubItems["NewKey"],
					Page = self,
					Placeholder = "..",
					Disabled = false
				})

				Button, ButtonItems = Components.Button({
					Parent = SubItems["NewKey"],
					Page = self,
				})

				ButtonItems["Button"].Instance.AnchorPoint = Vector2New(0, 1)
				ButtonItems["Button"].Instance.Position = UDim2New(0, 0, 1, 0)

				CheckKeyButton = Button:Add("Check key", function()
					NewKey:Check()
				end, false)

				GetKeyButton = Button:Add("Get key", function()
					NewKey:GetKey()
				end, false)
			end

			function NewKey:Check()
				if InputTextboxItems["Input"].Instance.Text == NewKey.Key then
					NewKey.Success = true
					Callback()
					return
				else
					NewKey.Success = false
					InputTextbox:SetText("Key is false.")
					task.wait(2)
					InputTextbox:SetText("Enter key here..")
				end
			end

			function NewKey:GetKey()
				setclipboard(tostring(GetKeyLink))
				InputTextbox:SetText("Copied link to your clipboard!")
				task.wait(2)
				InputTextbox:SetText("Enter key here..")
			end
		end

		Items["Inactive"]:Connect("MouseButton1Down", function()
			for Index, Value in Page.Window.Pages do
				Value:Turn(Value == Page)
			end
		end)

		if #Page.Window.Pages == 0 then 
			Page:Turn(true)
		end

		TableInsert(Page.Window.Pages, Page)
		return setmetatable(Page, Library.Pages)
	end

	Library.Pages.Section = function(self, Properties)
		Properties = Properties or { }

		local Section = {
			Window = self.Window,
			Page = self,

			Name = Properties.Name or Properties.name or "Section",
			Side = Properties.Side or Properties.side or 1,

			Items = { }
		}

		local Items = { } do
			Items["Section"] = Instances:Create("Frame", {
				Parent = Section.Page.ColumnsData[Section.Side].Instance,
				Name = "\0",
				BorderSizePixel = 0,
				Size = UDim2New(1, 0, 0, 45),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				AutomaticSize = Enum.AutomaticSize.Y,
				BackgroundColor3 = FromRGB(22, 20, 24)
			})  Items["Section"]:AddToTheme({BackgroundColor3 = "Inline"})

			Instances:Create("UICorner", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Instances:Create("UIGradient", {
				Parent = Items["Section"].Instance,
				Name = "\0"
			})

			Instances:Create("UIPadding", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				PaddingBottom = UDimNew(0, 8)
			})

			Items["Text"] = Instances:Create("TextLabel", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = Section.Name,
				AutomaticSize = Enum.AutomaticSize.X,
				Size = UDim2New(0, 0, 0, 15),
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0, 8),
				BorderSizePixel = 0,
				ZIndex = 2,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

			Instances:Create("Frame", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				Size = UDim2New(1, -16, 0, 1),
				Position = UDim2New(0, 8, 0, 28),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(41, 37, 45)
			}):AddToTheme({BackgroundColor3 = "Border"})

			Items["Content"] = Instances:Create("Frame", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				BorderSizePixel = 0,
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0, 38),
				Size = UDim2New(1, -16, 0, 0),
				ZIndex = 2,
				AutomaticSize = Enum.AutomaticSize.Y,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Instances:Create("UIListLayout", {
				Parent = Items["Content"].Instance,
				Name = "\0",
				Padding = UDimNew(0, 6),
				SortOrder = Enum.SortOrder.LayoutOrder
			})

			Section.Items = Items
		end

		return setmetatable(Section, Library.Sections)
	end

	Library.Pages.ImageSection = function(self, Properties)
		Properties = Properties or { }

		local Section = {
			Window = self.Window,
			Page = self,

			Name = Properties.Name or Properties.name or "ImageSection",
			Side = Properties.Side or Properties.side or 1,
			Images  = Properties.Images or Properties.images or {
				["Scary Cat"] = "115002736787206",
			},

			Items = { }
		}

		local Items = { } do
			Items["Section"] = Instances:Create("Frame", {
				Parent = Section.Page.ColumnsData[Section.Side].Instance,
				Name = "\0",
				BorderSizePixel = 0,
				Size = UDim2New(1, 0, 0, 45),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				AutomaticSize = Enum.AutomaticSize.Y,
				BackgroundColor3 = FromRGB(22, 20, 24)
			})  Items["Section"]:AddToTheme({BackgroundColor3 = "Inline"})

			Instances:Create("UICorner", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Instances:Create("UIGradient", {
				Parent = Items["Section"].Instance,
				Name = "\0"
			})

			Items["Text"] = Instances:Create("TextLabel", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = Section.Name,
				AutomaticSize = Enum.AutomaticSize.X,
				Size = UDim2New(0, 0, 0, 15),
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0, 8),
				BorderSizePixel = 0,
				ZIndex = 2,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

			Instances:Create("Frame", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				Size = UDim2New(1, -16, 0, 1),
				Position = UDim2New(0, 8, 0, 28),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(41, 37, 45)
			}):AddToTheme({BackgroundColor3 = "Border"})

			Instances:Create("UIPadding", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				PaddingBottom = UDimNew(0, 8)
			})

			Items["Image"] = Instances:Create("ImageLabel", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				ScaleType = Enum.ScaleType.Fit,
				BorderColor3 = FromRGB(0, 0, 0),
				Size = UDim2New(1, -16, 0, IsMobile and 179 or 279),
				Image = "rbxassetid://",
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0, 35),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Section.Items = Items
		end 

		local ImagesDropdown, ImagesDropdownItems = Components.Dropdown({
			Name = "Images",
			Parent = Items["Section"],
			Flag = "Images"..Section.Name,
			Items = { },
			Default = nil,
			Multi = false,
			Page = Section.Page,
			Window = Section.Window,
			IsLabelDropdown = false,
			Disabled = false
		})

		ImagesDropdownItems["Dropdown"].Instance.Position = UDim2New(0, 8, 0, Items["Image"].Instance.AbsoluteSize.Y + 46)
		ImagesDropdownItems["Dropdown"].Instance.Size = UDim2New(1, -16, 0, 25)

		for Index, Value in Section.Images do 
			ImagesDropdown:Add(Index)
		end

		ImagesDropdown.OnChanged = function(Value)
			local ImageData = Section.Images[Value] 

			if not ImageData then
				Items["Image"].Instance.Image = "rbxassetid://"
				return 
			end

			Items["Image"].Instance.Image = "rbxassetid://"..ImageData
		end

		return setmetatable(Section, Library.Sections)
	end

	Library.Pages.ViewportSection = function(self, Properties)
		Properties = Properties or { }

		local Section = {
			Window = self.Window,
			Page = self,

			Name = Properties.Name or Properties.name or "Section",
			Side = Properties.Side or Properties.side or "Left",
			Part = Properties.Part or Properties.part or nil,

			Items = { }
		}

		local Items = { } do
			Items["Section"] = Instances:Create("TextButton", {
				Parent = Section.Page.ColumnsData[Section.Side].Instance,
				Name = "\0",
				AutoButtonColor = false,
				Text = "",
				BorderSizePixel = 0,
				Size = UDim2New(1, 0, 0, 45),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				AutomaticSize = Enum.AutomaticSize.Y,
				BackgroundColor3 = FromRGB(22, 20, 24)
			})  Items["Section"]:AddToTheme({BackgroundColor3 = "Inline"})

			Instances:Create("UICorner", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				CornerRadius = UDimNew(0, 5)
			})

			Instances:Create("UIGradient", {
				Parent = Items["Section"].Instance,
				Name = "\0"
			})

			Items["Text"] = Instances:Create("TextLabel", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = Section.Name,
				AutomaticSize = Enum.AutomaticSize.X,
				Size = UDim2New(0, 0, 0, 15),
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0, 8),
				BorderSizePixel = 0,
				ZIndex = 2,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

			Instances:Create("Frame", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				Size = UDim2New(1, -16, 0, 1),
				Position = UDim2New(0, 8, 0, 28),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(41, 37, 45)
			}):AddToTheme({BackgroundColor3 = "Border"})

			Instances:Create("UIPadding", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				PaddingBottom = UDimNew(0, 8)
			})

			Items["PartViewer"] = Instances:Create("ViewportFrame", {
				Parent = Items["Section"].Instance,
				Name = "\0",
				BorderColor3 = FromRGB(0, 0, 0),
				BackgroundTransparency = 1,
				Position = UDim2New(0, 8, 0, 35),
				Size = UDim2New(1, -16, 0, 225),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})
		end

		local ViewportCamera = InstanceNew("Camera")
		Items["PartViewer"].Instance.CurrentCamera = ViewportCamera

		local ClonedPart = Section.Part:Clone()
		ClonedPart.Anchored = true
		ClonedPart.Parent = Items["PartViewer"].Instance
		ClonedPart.Position = Vector3New(0, 5, 0)

		local Distance = MathMax(Section.Part.Size.X, Section.Part.Size.Y, Section.Part.Size.Z) * 3
		ViewportCamera.CFrame = CFrameNew(0, Distance * 0.5, Distance)

		local LastPosition 
		local IsRotating = false
		local Sensitivity = 0.7

		Items["PartViewer"]:Connect("InputBegan", function(Input)
			if Input.UserInputType == Enum.UserInputType.MouseButton1 or Input.UserInputType == Enum.UserInputType.Touch then
				IsRotating = true
				LastPosition = Input.Position

				Input.Changed:Connect(function()
					if Input.UserInputState == Enum.UserInputState.End then
						IsRotating = false
						LastPosition = nil
					end
				end)
			end
		end)

		Items["PartViewer"]:Connect("InputChanged", function(Input)
			if Input.UserInputType == Enum.UserInputType.MouseMovement or Input.UserInputType == Enum.UserInputType.Touch then
				if not LastPosition then 
					return
				end

				if not IsRotating then
					return
				end

				local Delta = Input.Position - LastPosition
				ClonedPart.CFrame = ClonedPart.CFrame * CFrameAngles(0, MathRad(-Delta.X * Sensitivity), 0) -- yaw
				ClonedPart.CFrame = ClonedPart.CFrame * CFrameAngles(MathRad(Delta.Y * Sensitivity), 0, 0) -- pitch
				LastPosition = Input.Position
			end
		end)

		return setmetatable(Section, Library.Sections)
	end

	Library.Sections.Toggle = function(self, Properties)
		Properties = Properties or { }

		local Toggle = {
			Window = self.Window,
			Page = self.Page,
			Section = self,

			Name = Properties.Name or Properties.name or "Toggle",
			Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
			Default = Properties.Default or Properties.default or false,
			Callback = Properties.Callback or Properties.callback or function() end,
			Disabled = Properties.Disabled or Properties.disabled or false,
			OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			Tooltip = Properties.Tooltip or Properties.tooltip or nil,

			Count = 0
		}

		local NewToggle, ToggleItems = Components.Toggle({
			Name = Toggle.Name,
			Parent = Toggle.Section.Items["Content"],
			Flag = Toggle.Flag,
			Default = Toggle.Default,
			Page = Toggle.Page,
			Section = Toggle.Section,
			OnChanged = Toggle.OnChanged,
			Callback = Toggle.Callback,
			Disabled = Toggle.Disabled
		})

		ToggleItems["Toggle"]:Tooltip(Toggle.Tooltip)

		function Toggle:Set(Bool)
			NewToggle:Set(Bool)
		end

		function Toggle:SetText(Text)
			NewToggle:SetText(Text)
		end

		function Toggle:SetDisabled(Bool)
			NewToggle:SetDisabled(Bool)
		end

		function Toggle:SetVisible(Bool)
			NewToggle:SetVisible(Bool)
		end

		function Toggle:OnChanged(Callback)
			NewToggle.OnChanged = Callback
			Callback(NewToggle.Value) 
		end

		function Toggle:Colorpicker(Properties)
			Properties = Properties or { }

			local Colorpicker = {
				Window = self.Window,
				Page = self.Page,
				Section = self.Section,

				Name = Properties.Name or Properties.name or "Colorpicker",
				Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
				Alpha = Properties.Alpha or Properties.alpha or 0,
				Default = Properties.Default or Properties.default or Color3.fromRGB(255, 255, 255),
				Callback = Properties.Callback or Properties.callback or function() end,
				OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
				Disabled = Properties.Disabled or Properties.disabled or false
			}

			Toggle.Count += 1

			local NewColorpicker, ColorpickerItems = Components.Colorpicker({
				Name = Colorpicker.Name,
				Count = Toggle.Count,
				Parent = ToggleItems["SubElementsHolder"],
				Flag = Colorpicker.Flag,
				Default = Colorpicker.Default,
				Alpha = Colorpicker.Alpha,
				Page = Colorpicker.Page,
				Section = Colorpicker.Section,
				OnChanged = Colorpicker.OnChanged,
				Window = Colorpicker.Window,
				IsToggle = true,
				Callback = Colorpicker.Callback,
				Disabled = Colorpicker.Disabled
			})

			return NewColorpicker
		end

		function Toggle:Keybind(Properties)
			Properties = Properties or { }

			local Keybind = {
				Window = self.Window,
				Page = self.Page,
				Section = self.Section,

				Name = Properties.Name or Properties.name or "Keybind",
				Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
				Default = Properties.Default or Properties.default or nil,
				Mode = Properties.Mode or Properties.mode or "Toggle",
				Callback = Properties.Callback or Properties.callback or function() end,
				OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			}

			Toggle.Count += 1

			local NewKeybind, KeybindItems = Components.Keybind({
				Name = Keybind.Name,
				Count = Toggle.Count,
				Parent = ToggleItems["SubElementsHolder"],
				Flag = Keybind.Flag,
				Default = Keybind.Default,
				Mode = Keybind.Mode,
				IsToggle = true,
				Page = Keybind.Page,
				Section = Keybind.Section,
				OnChanged = Keybind.OnChanged,
				Window = Keybind.Window,
				Callback = Keybind.Callback
			})

			return NewKeybind
		end

		return Toggle
	end

	Library.Sections.Checkbox = function(self, Properties)
		Properties = Properties or { }

		local Checkbox = {
			Window = self.Window,
			Page = self.Page,
			Section = self,

			Name = Properties.Name or Properties.name or "Checkbox",
			Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
			Default = Properties.Default or Properties.default or false,
			Callback = Properties.Callback or Properties.callback or function() end,
			Disabled = Properties.Disabled or Properties.disabled or false,
			OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			Tooltip = Properties.Tooltip or Properties.tooltip or nil,

			Count = 0
		}

		local NewCheckbox, CheckboxItems = Components.Checkbox({
			Name = Checkbox.Name,
			Parent = Checkbox.Section.Items["Content"],
			Flag = Checkbox.Flag,
			Default = Checkbox.Default,
			Page = Checkbox.Page,
			Section = Checkbox.Section,
			OnChanged = Checkbox.OnChanged,
			Callback = Checkbox.Callback,
			Disabled = Checkbox.Disabled
		})

		CheckboxItems["Checkbox"]:Tooltip(Checkbox.Tooltip)

		function Checkbox:Set(Bool)
			NewCheckbox:Set(Bool)
		end

		function Checkbox:SetText(Text)
			NewCheckbox:SetText(Text)
		end

		function Checkbox:SetDisabled(Bool)
			NewCheckbox:SetDisabled(Bool)
		end

		function Checkbox:SetVisible(Bool)
			NewCheckbox:SetVisible(Bool)
		end

		function Checkbox:OnChanged(Callback)
			NewCheckbox.OnChanged = Callback
			Callback(NewCheckbox.Value) 
		end

		function Checkbox:Colorpicker(Properties)
			Properties = Properties or { }

			local Colorpicker = {
				Window = self.Window,
				Page = self.Page,
				Section = self.Section,

				Name = Properties.Name or Properties.name or "Colorpicker",
				Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
				Alpha = Properties.Alpha or Properties.alpha or 0,
				Default = Properties.Default or Properties.default or Color3.fromRGB(255, 255, 255),
				Callback = Properties.Callback or Properties.callback or function() end,
				OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
				Disabled = Properties.Disabled or Properties.disabled or false
			}

			Checkbox.Count += 1

			local NewColorpicker, ColorpickerItems = Components.Colorpicker({
				Name = Colorpicker.Name,
				Count = Checkbox.Count,
				Parent = CheckboxItems["SubElementsHolder"],
				Flag = Colorpicker.Flag,
				Default = Colorpicker.Default,
				Alpha = Colorpicker.Alpha,
				Page = Colorpicker.Page,
				Section = Colorpicker.Section,
				OnChanged = Colorpicker.OnChanged,
				Window = Colorpicker.Window,
				IsCheckbox = true,
				Callback = Colorpicker.Callback,
				Disabled = Colorpicker.Disabled
			})

			return NewColorpicker
		end

		function Checkbox:Keybind(Properties)
			Properties = Properties or { }

			local Keybind = {
				Window = self.Window,
				Page = self.Page,
				Section = self.Section,

				Name = Properties.Name or Properties.name or "Keybind",
				Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
				Default = Properties.Default or Properties.default or nil,
				Mode = Properties.Mode or Properties.mode or "Toggle",
				Callback = Properties.Callback or Properties.callback or function() end,
				OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			}

			Checkbox.Count += 1

			local NewKeybind, KeybindItems = Components.Keybind({
				Name = Keybind.Name,
				Count = Checkbox.Count,
				Parent = CheckboxItems["SubElementsHolder"],
				Flag = Keybind.Flag,
				Default = Keybind.Default,
				Mode = Keybind.Mode,
				IsCheckbox = true,
				Page = Keybind.Page,
				Section = Keybind.Section,
				OnChanged = Keybind.OnChanged,
				Window = Keybind.Window,
				Callback = Keybind.Callback
			})

			return NewKeybind
		end

		return Checkbox, NewCheckbox
	end

	Library.Sections.Button = function(self, Properties)
		Properties = Properties or { }

		local Button = {
			Window = self.Window,
			Page = self.Page,
			Section = self
		}

		local NewButton, ButtonItems = Components.Button({
			Parent = Button.Section.Items["Content"],
			Page = Button.Page,
		})

		function Button:Add(Text, Callback, Confirmation, Disabled, Tooltip)
			local _NewButton = {
				Text = Text,
				Callback = Callback,
				Confirmation = Confirmation,
				Disabled = Disabled
			}

			local NewAddedButton, NewAddedButtonItems = NewButton:Add(Text, Callback, Confirmation, Disabled)
			NewAddedButtonItems["NewButton"]:Tooltip(Tooltip)

			function _NewButton:SetText(Text)
				NewAddedButton:SetText(Text)
			end

			function _NewButton:SetVisible(Bool)
				NewAddedButton:SetVisible(Bool)
			end

			function _NewButton:SetDisabled(Bool)
				NewAddedButton:SetDisabled(Bool)
			end

			function _NewButton:OnPressed(Callback)
				NewAddedButton.OnPressed = Callback
				Callback()
			end

			return _NewButton
		end

		function Button:SetVisible(Bool)
			NewButton:SetVisible(Bool)
		end

		return Button
	end

	Library.Sections.Slider = function(self, Properties)
		Properties = Properties or { }

		local Slider = {
			Window = self.Window,
			Page = self.Page,
			Section = self,

			Name = Properties.Name or Properties.name or "Slider",
			Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
			Default = Properties.Default or Properties.default or 0,
			Min = Properties.Min or Properties.min or 0,
			Max = Properties.Max or Properties.max or 100,
			Decimals = Properties.Decimals or Properties.decimals or 1,
			Suffix = Properties.Suffix or Properties.suffix or "",
			Callback = Properties.Callback or Properties.callback or function() end,
			OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			Disabled = Properties.Disabled or Properties.disabled or false,
			Tooltip = Properties.Tooltip or Properties.tooltip or nil
		}

		local NewSlider, SliderItems = Components.Slider({
			Name = Slider.Name,
			Parent = Slider.Section.Items["Content"],
			Flag = Slider.Flag,
			Default = Slider.Default,
			Min = Slider.Min,
			Max = Slider.Max,
			Suffix = Slider.Suffix,
			Decimals = Slider.Decimals,
			Page = Slider.Page,
			Section = Slider.Section,
			OnChanged = Slider.OnChanged,
			Callback = Slider.Callback,
			Disabled = Slider.Disabled
		})

		SliderItems["Slider"]:Tooltip(Slider.Tooltip)

		function Slider:Set(Value)
			NewSlider:Set(Value)
		end

		function Slider:SetDisabled(Bool)
			NewSlider:SetDisabled(Bool)
		end

		function Slider:SetVisible(Bool)
			NewSlider:SetVisible(Bool)
		end

		function Slider:SetMin(Value)
			NewSlider:SetMin(Value)
		end

		function Slider:SetMax(Value)
			NewSlider:SetMax(Value)
		end

		function Slider:OnChanged(Callback)
			NewSlider.OnChanged = Callback
			Callback(NewSlider.Value)
		end

		function Slider:SetSuffix(Text)
			NewSlider:SetSuffix(Text)
		end

		function Slider:SetText(Text)
			NewSlider:SetText(Text)
		end

		return Slider
	end

	Library.Sections.Dropdown = function(self, Properties)
		Properties = Properties or { }

		local Dropdown = {
			Window = self.Window,
			Page = self.Page,
			Section = self,

			Name = Properties.Name or Properties.name or "Dropdown",
			Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
			Default = Properties.Default or Properties.default or nil,
			Items = Properties.Items or Properties.items or { },
			Callback = Properties.Callback or Properties.callback or function() end,
			Multi = Properties.Multi or Properties.multi or false,
			OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			IsLabelDropdown = Properties.IsLabelDropdown or Properties.islabeldropdown or false,
			Disabled = Properties.Disabled or Properties.disabled or false,
			Tooltip = Properties.Tooltip or Properties.tooltip or nil
		}

		local NewDropdown, DropdownItems = Components.Dropdown({
			Name = Dropdown.Name,
			Parent = Dropdown.Section.Items["Content"],
			Flag = Dropdown.Flag,
			Items = Dropdown.Items,
			Default = Dropdown.Default,
			Callback = Dropdown.Callback,
			Multi = Dropdown.Multi,
			Page = Dropdown.Page,
			Window = Dropdown.Window,
			IsLabelDropdown = Dropdown.IsLabelDropdown,
			OnChanged = Dropdown.OnChanged,
			Disabled = Dropdown.Disabled
		})

		DropdownItems["Dropdown"]:Tooltip(Dropdown.Tooltip)

		function Dropdown:Set(Items)
			NewDropdown:Set(Items)
		end

		function Dropdown:OnChanged(Callback)
			NewDropdown.OnChanged = Callback
			Callback(NewDropdown.Value)
		end

		function Dropdown:Remove(Option)
			NewDropdown:Remove(Option)
		end

		function Dropdown:Add(Option, Icon)
			NewDropdown:Add(Option, Icon)
		end

		function Dropdown:Clear()
			NewDropdown:Clear()
		end

		function Dropdown:SetDisabled(Bool)
			NewDropdown:SetDisabled(Bool)
		end

		function Dropdown:SetVisible(Bool)
			NewDropdown:SetVisible(Bool)
		end

		function Dropdown:Refresh(List)
			NewDropdown:Refresh(List)
		end

		function Dropdown:Get()
			return NewDropdown:Get()
		end

		function Dropdown:SetText(Text)
			NewDropdown:SetText(Text)
		end

		function Dropdown:SetMulti(Bool)
			NewDropdown:SetMulti(Bool)
		end

		return Dropdown
	end

	Library.Sections.ToggleDropdown = function(self, Properties)
		Properties = Properties or { }

		local Dropdown = {
			Window = self.Window,
			Page = self.Page,
			Section = self,

			Name = Properties.Name or Properties.name or "Dropdown",
			Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
			Default = Properties.Default or Properties.default or nil,
			Items = Properties.Items or Properties.items or { },
			Callback = Properties.Callback or Properties.callback or function() end,
			Multi = Properties.Multi or Properties.multi or false,
			OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			Disabled = Properties.Disabled or Properties.disabled or false,
			Tooltip = Properties.Tooltip or Properties.tooltip or nil
		}

		local NewDropdown, DropdownItems = Components.ToggleDropdown({
			Name = Dropdown.Name,
			Parent = Dropdown.Section.Items["Content"],
			Flag = Dropdown.Flag,
			Items = Dropdown.Items,
			Default = Dropdown.Default,
			Callback = Dropdown.Callback,
			Multi = Dropdown.Multi,
			Page = Dropdown.Page,
			Window = Dropdown.Window,
			OnChanged = Dropdown.OnChanged,
			Disabled = Dropdown.Disabled
		})

		DropdownItems["Dropdown"]:Tooltip(Dropdown.Tooltip)

		function Dropdown:Set(Items)
			NewDropdown:Set(Items)
		end

		function Dropdown:OnChanged(Callback)
			NewDropdown.OnChanged = Callback
			Callback(NewDropdown.Value)
		end

		function Dropdown:Remove(Option)
			NewDropdown:Remove(Option)
		end

		function Dropdown:Add(Option, Icon)
			NewDropdown:Add(Option, Icon)
		end

		function Dropdown:Clear()
			NewDropdown:Clear()
		end

		function Dropdown:SetDisabled(Bool)
			NewDropdown:SetDisabled(Bool)
		end

		function Dropdown:SetVisible(Bool)
			NewDropdown:SetVisible(Bool)
		end

		function Dropdown:Refresh(List)
			NewDropdown:Refresh(List)
		end

		function Dropdown:Get()
			return NewDropdown:Get()
		end

		function Dropdown:SetText(Text)
			NewDropdown:SetText(Text)
		end

		function Dropdown:SetMulti(Bool)
			NewDropdown:SetMulti(Bool)
		end

		return Dropdown
	end

	Library.Sections.Label = function(self, Text, Alignment, Tooltip, Outline, Icon)
		local Label = {
			Window = self.Window,
			Page = self.Page,
			Section = self,

			Name = Text or "Label",
			Alignment = Alignment or "Left",
			Tooltip = Tooltip or nil,

			Count = 0
		}

		local Items = { } do
			Items["Label"] = Instances:Create("Frame", {
				Parent = Label.Section.Items["Content"].Instance,
				Name = "\0",
				BackgroundTransparency = 1,
				Size = UDim2New(1, 0, 0, 20),
				BorderColor3 = FromRGB(0, 0, 0),
				ZIndex = 2,
				BorderSizePixel = 0,
				BackgroundColor3 = FromRGB(255, 255, 255)
			})

			Items["Label"]:Tooltip(Label.Tooltip)

			Items["Text"] = Instances:Create("TextLabel", {
				Parent = Items["Label"].Instance,
				Name = "\0",
				FontFace = Library.Font,
				TextColor3 = FromRGB(255, 255, 255),
				BorderColor3 = FromRGB(0, 0, 0),
				Text = Label.Name,
				TextXAlignment = Enum.TextXAlignment[Label.Alignment],
				AutomaticSize = Enum.AutomaticSize.X,
				AnchorPoint = Vector2New(0, 0.5),
				Size = UDim2New(0, 0, 0, 15),
				RichText = true,
				BackgroundTransparency = 1,
				Position = UDim2New(0, not Icon and 0 or 32, 0.5, 0),
				BorderSizePixel = 0,
				ZIndex = 2,
				TextSize = 14,
				BackgroundColor3 = FromRGB(255, 0, 255)
			})  Items["Text"]:AddToTheme({TextColor3 = "Text"})

			if Outline then 
				Instances:Create("UIStroke", {
					Parent = Items["Text"].Instance,
					Name = "\0",
					ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual,
					LineJoinMode = Enum.LineJoinMode.Round,
					Color = FromRGB(0, 0, 0),
					Thickness = 1
				})
			end

			if Icon then
				Items["Icon"] = Instances:Create("ImageLabel", {
					Parent = Items["Label"].Instance,
					Name = "\0",
					BorderColor3 = FromRGB(0, 0, 0),
					Size = UDim2New(0, 16, 0, 16),
					AnchorPoint = Vector2New(0, 0.5),
					Image = "rbxassetid://"..Icon,
					BackgroundTransparency = 1,
					Position = UDim2New(0, 8, 0.5, 0),
					ZIndex = 2,
					BorderSizePixel = 0,
					BackgroundColor3 = FromRGB(255, 255, 255)
				})
			end
		end

		function Label:Colorpicker(Properties)
			Properties = Properties or { }

			local Colorpicker = {
				Window = self.Window,
				Page = self.Page,
				Section = self.Section,

				Name = Properties.Name or Properties.name or "Colorpicker",
				Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
				Alpha = Properties.Alpha or Properties.alpha or 0,
				Default = Properties.Default or Properties.default or Color3.fromRGB(255, 255, 255),
				Callback = Properties.Callback or Properties.callback or function() end,
				OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
				Disabled = Properties.Disabled or Properties.disabled or false
			}

			Label.Count += 1

			local NewColorpicker, ColorpickerItems = Components.Colorpicker({
				Name = Colorpicker.Name,
				Count = Label.Count,
				Parent = Items["Label"],
				Flag = Colorpicker.Flag,
				Default = Colorpicker.Default,
				Alpha = Colorpicker.Alpha,
				Page = Colorpicker.Page,
				Section = Colorpicker.Section,
				OnChanged = Colorpicker.OnChanged,
				Window = Colorpicker.Window,
				Callback = Colorpicker.Callback,
				Disabled = Colorpicker.Disabled
			})

			return NewColorpicker
		end

		function Label:Keybind(Properties)
			Properties = Properties or { }

			local Keybind = {
				Window = self.Window,
				Page = self.Page,
				Section = self.Section,

				Name = Properties.Name or Properties.name or "Keybind",
				Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
				Default = Properties.Default or Properties.default or nil,
				Mode = Properties.Mode or Properties.mode or "Toggle",
				Callback = Properties.Callback or Properties.callback or function() end,
				OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			}

			Label.Count += 1

			local NewKeybind, KeybindItems = Components.Keybind({
				Name = Keybind.Name,
				Count = Label.Count,
				Parent = Items["Label"],
				Flag = Keybind.Flag,
				Default = Keybind.Default,
				Mode = Keybind.Mode,
				Page = Keybind.Page,
				Section = Keybind.Section,
				OnChanged = Keybind.OnChanged,
				Window = Keybind.Window,
				Callback = Keybind.Callback
			})

			return NewKeybind
		end

		function Label:SetText(Text)
			Text = tostring(Text)
			Items["Text"].Instance.Text = Text
		end

		function Label:SetTextColor(Color)
			Library:RemoveFromTheme(Items["Text"])
			task.wait(0.1)
			Items["Text"].Instance.TextColor3 = Color
		end

		local SearchData = {
			Name = Label.Name,
			Item = Items["Label"]
		}

		local PageSearchData = Library.SearchItems[Label.Page]

		if not PageSearchData then 
			return 
		end

		TableInsert(PageSearchData, SearchData)

		return Label 
	end

	Library.Sections.Textbox = function(self, Properties)
		Properties = Properties or { }

		local Textbox = {
			Window = self.Window,
			Page = self.Page,
			Section = self,

			Name = Properties.Name or Properties.name or "Textbox",
			Flag = Properties.Flag or Properties.flag or Library:NextFlag(),
			Default = Properties.Default or Properties.default or "",
			Placeholder = Properties.Placeholder or Properties.placeholder or "",
			Callback = Properties.Callback or Properties.callback or function() end,
			OnChanged = Properties.OnChanged or Properties.onchanged or function() end,
			Disabled = Properties.Disabled or Properties.disabled or false,
			Tooltip = Properties.Tooltip or Properties.tooltip or nil
		}

		local NewTextbox, TextboxItems = Components.Textbox({
			Name = Textbox.Name,
			Flag = Textbox.Flag,
			Default = Textbox.Default,
			Parent = Textbox.Section.Items["Content"],
			Placeholder = Textbox.Placeholder,
			Page = Textbox.Page,
			Section = Textbox.Section,
			OnChanged = Textbox.OnChanged,
			Window = Textbox.Window,
			Callback = Textbox.Callback,
			Disabled = Textbox.Disabled
		})

		TextboxItems["Textbox"]:Tooltip(Textbox.Tooltip)

		function Textbox:Set(Value)
			NewTextbox:Set(Value)
		end

		function Textbox:SetText(Text)
			NewTextbox:SetText(Text)
		end

		function Textbox:SetDisabled(Bool)
			NewTextbox:SetDisabled(Bool)
		end

		function Textbox:SetVisible(Bool)
			NewTextbox:SetVisible(Bool)
		end

		return Textbox
	end

	Library.CheckForAutoLoad = function(self)
		local Config = readfile(self.Folders.Directory .. "/autoload.json")

		if not Config or Config == "" then 
			return 
		end

		local Success, Error = Library:LoadConfig(Config)

		if Success then 
			Library:Notification("Success!", "Succesfully autoloaded config.", 5)

			task.wait(0.3)

			Library:Thread(function() -- i do this because sometimes the themes dont update
				for Index, Value in Library.Theme do 
					Library.Theme[Index] = Library.Flags["Theme" .. Index].Color
					Library:ChangeTheme(Index, Library.Flags["Theme" .. Index].Color)
				end    
			end)
		else
			Library:Notification("Error!", "Failed to load config.", 5)
		end
	end

	Library.CreateSettingsPage = function(self, Window, Watermark, KeybindList)
		local SettingsPage = Window:Page({
			Name = "Settings",
			Columns = 2
		})
		do -- pasted from my other ui
			do
				do -- Configs
					local ConfigsSection = SettingsPage:Section({Name = "Configs", Side = 2})

					local ConfigSelected 
					local ConfigName

					do
						local ConfigsDropdown = ConfigsSection:Dropdown({
							Name = "Configs", 
							Flag = "ConfigsList", 
							Items = { }, 
							Multi = false,
							Callback = function(Value)
								ConfigSelected = Value
							end
						})

						ConfigsSection:Textbox({
							Name = "Name", 
							Default = "", 
							Flag = "ConfigName", 
							Placeholder = "...", 
							Callback = function(Value)
								ConfigName = Value
							end
						})

						local CreateDeleteButton = ConfigsSection:Button()

						CreateDeleteButton:Add("Create", function()
							if ConfigName and ConfigName ~= "" then
								writefile(Library.Folders.Configs .. "/" .. ConfigName .. tostring(game.GameId) .. ".json", Library:GetConfig())
								Library:RefreshConfigsList(ConfigsDropdown)
							end
						end, false)

						CreateDeleteButton:Add("Delete", function()
							if ConfigSelected then
								local CurrentConfigName = string.gsub(ConfigSelected, ".json", "")
								CurrentConfigName ..= "" .. game.GameId .. ".json"
								Library:DeleteConfig(CurrentConfigName)
								Library:RefreshConfigsList(ConfigsDropdown)
							end
						end, false)

						local LoadSaveButton = ConfigsSection:Button()

						LoadSaveButton:Add("Load", function()
							if ConfigSelected then
								local CurrentConfigName = string.gsub(ConfigSelected, ".json", "")
								CurrentConfigName ..= "" .. game.GameId .. ".json"
								local Success, Result = Library:LoadConfig(readfile(Library.Folders.Configs .. "/" .. CurrentConfigName))

								if Success then 
									Library:Notification("Success!", "Succesfully loaded config.", 5)

									task.wait(0.3)

									Library:Thread(function() -- i do this because sometimes the themes dont update
										for Index, Value in Library.Theme do 
											Library.Theme[Index] = Library.Flags["Theme" .. Index].Color
											Library:ChangeTheme(Index, Library.Flags["Theme" .. Index].Color)
										end    
									end)
								else
									Library:Notification("Error!", "Failed to load config. Report this to the developers:\n"..Result, 5)
								end
							end
						end, false)

						LoadSaveButton:Add("Save", function()
							if ConfigSelected then
								local CurrentConfigName = string.gsub(ConfigSelected, ".json", "")
								CurrentConfigName ..= "" .. game.GameId .. ".json"  

								local Success, Error = Library:SafeCall(function()
									writefile(Library.Folders.Configs .. "/" .. CurrentConfigName, Library:GetConfig())
								end)

								if not Success then 
									Library:Notification("Error!", "Failed to save config. Report this to the developers:\n"..Error, 5)
								else
									Library:Notification("Success!", "Succesfully saved config.", 5)
								end
							end
						end, false)

						local RefreshlistButton = ConfigsSection:Button()

						RefreshlistButton:Add("Refresh", function()
							Library:RefreshConfigsList(ConfigsDropdown)
						end, false)

						local AutoloadButton = ConfigsSection:Button()

						AutoloadButton:Add("Set autoload", function()
							if ConfigSelected then 
								local CurrentConfigName = string.gsub(ConfigSelected, ".json", "")
								CurrentConfigName ..= "" .. game.GameId .. ".json"
								writefile(Library.Folders.Directory .. "/autoload.json", readfile(Library.Folders.Configs .. "/" .. CurrentConfigName))
								Library:Notification("Success!", "Succesfully set autoload.", 5)
							end
						end)

						AutoloadButton:Add("Clear autoload", function()
							writefile(Library.Folders.Directory .. "/autoload.json", "")
						end)

						ConfigsSection:Toggle({
							Name = "Watermark",
							Flag = "WatermarkEnabled",
							Default = false,
							Callback = function(Value)
								Watermark:SetVisible(Value)
							end
						})

						ConfigsSection:Toggle({
							Name = "Keybind list",
							Flag = "Keybind list",
							Default = false,
							Callback = function(Value)
								KeybindList:SetVisible(Value)
							end
						})

						Library:RefreshConfigsList(ConfigsDropdown)
					end
				end

				do -- Themes
					local ThemeSection = SettingsPage:Section({Name = "Themes", Side = 1})

					local ThemeSelected 
					local ThemeName

					do
						local ThemesDropdown = ThemeSection:Dropdown({
							Name = "Themes", 
							Flag = "ThemesList", 
							Items = { }, 
							Multi = false,
							Callback = function(Value)
								ThemeSelected = Value
							end
						})

						ThemeSection:Textbox({
							Name = "Name", 
							Default = "", 
							Flag = "ThemeName", 
							Placeholder = "...", 
							Callback = function(Value)
								ThemeName = Value
							end
						})

						local CreateDeleteButton = ThemeSection:Button()

						CreateDeleteButton:Add("Create", function()
							if ThemeName and ThemeName ~= "" then
								writefile(Library.Folders.Themes .. "/" .. ThemeName .. ".json", Library:GetConfig())
								Library:RefreshConfigsList(ThemesDropdown)
							end
						end, false)

						CreateDeleteButton:Add("Delete", function()
							if ThemeSelected then
								Library:DeleteConfig(ThemeSelected)
								Library:RefreshConfigsList(ThemesDropdown)
							end
						end, false)

						local LoadSaveButton = ThemeSection:Button()

						LoadSaveButton:Add("Load", function()
							if ThemeSelected then
								local Success, Result = Library:LoadTheme(readfile(Library.Folders.Themes .. "/" .. ThemeSelected))

								if Success then 
									Library:Notification("Success!", "Succesfully loaded theme.", 5)

									task.wait(0.3)

									Library:Thread(function() -- i do this because sometimes the themes dont update
										for Index, Value in Library.Theme do 
											Library.Theme[Index] = Library.Flags["Theme" .. Index].Color
											Library:ChangeTheme(Index, Library.Flags["Theme" .. Index].Color)
										end    
									end)
								else
									Library:Notification("Error!", "Failed to load theme. Report this to the developers:\n"..Result, 5)
								end
							end
						end, false)

						LoadSaveButton:Add("Save", function()
							if ThemeSelected then  
								local Success, Error = Library:SafeCall(function()
									writefile(Library.Folders.Themes .. "/" .. ThemeSelected, Library:GetTheme())
								end)

								if not Success then 
									Library:Notification("Error!", "Failed to save theme. Report this to the developers:\n"..Error, 5)
								else
									Library:Notification("Success!", "Succesfully saved theme.", 5)
								end
							end
						end, false)

						local RefreshlistButton = ThemeSection:Button()

						RefreshlistButton:Add("Refresh", function()
							Library:RefreshThemesList(ThemesDropdown)
						end, false)

						local ThemesPresetDropdown = ThemeSection:Dropdown({
							Name = "Themes Preset", 
							Flag = "ThemesPresetList", 
							Items = { }, 
							Multi = false,
							Callback = function(Value)
								local ThemeData = Library.Themes[Value]

								if not ThemeData then 
									return
								end

								for Index, Value in Library.Theme do 
									Library.Theme[Index] = ThemeData[Index]
									Library:ChangeTheme(Index, ThemeData[Index])

									Library.ThemeColorpickers[Index]:Set(ThemeData[Index])
								end

								task.wait(0.3)

								Library:Thread(function()
									for Index, Value in Library.Theme do 
										Library.Theme[Index] = Library.Flags["Theme" .. Index].Color
										Library:ChangeTheme(Index, Library.Flags["Theme" .. Index].Color)
									end    
								end)
							end
						})

						for Index, Value in Library.Themes do 
							ThemesPresetDropdown:Add(Index)
						end

						Library:RefreshThemesList(ThemesDropdown)
					end
				end

				do -- Settings
					local SettingsSection = SettingsPage:Section({Name = "Settings", Side = 2})

					do
						SettingsSection:Label("Menu keybind", "Left"):Keybind({
							Name = "Menu keybind",
							Flag = "Menu Keybind",
							Default = Enum.KeyCode.RightControl,
							Mode = "Toggle",
							Callback = function(Value)
								Library.MenuKeybind = Library.Flags["Menu Keybind"].Key
							end
						})

						SettingsSection:Slider({
							Name = "Background opacity",
							Min = 0,
							Max = 1,
							Default = 0.3,
							Decimals = 0.01,
							Flag = "Background opacity",
							Callback = function(Value)
								Window:SetBackgroundTransparency(Value)
							end
						})

						SettingsSection:Slider({
							Name = "Tween time",
							Min = 0,
							Max = 5,
							Default = 0.25,
							Decimals = 0.01,
							Flag = "Tween Time",
							Callback = function(Value)
								Library.Tween.Time = Value
							end
						})

						SettingsSection:Dropdown({
							Name = "Style",
							Flag = "TweenStyle",
							Default = "Cubic",
							Items = {"Linear", "Sine", "Quad", "Cubic", "Quart", "Quint", "Exponential", "Circular", "Back", "Elastic", "Bounce"},
							Callback = function(Value)
								Library.Tween.Style = Enum.EasingStyle[Value]
							end
						})

						SettingsSection:Dropdown({
							Name = "Direction",
							Flag = "TweenDirection",
							Default = "Out",
							Items = {"In", "Out", "InOut"},
							Callback = function(Value)
								Library.Tween.Direction = Enum.EasingDirection[Value]
							end
						})
					end
				end

				do
					local ThemeSection = SettingsPage:Section({
						Name = "Theme",
						Side = 1
					})

					do
						for Index, Value in Library.Theme do 
							Library.ThemeColorpickers[Index] = ThemeSection:Label(Index, "Left"):Colorpicker({Name = Index, Default = Value, Flag = "Theme"..Index, Callback = function(Value) 
								Library.Theme[Index] = Value
								Library:ChangeTheme(Index, Value)
							end})
						end
					end
				end
			end
		end
	end
end

getgenv().Library = Library
return Library
