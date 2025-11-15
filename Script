-- Steel Brainrot Cheat Menu with Dealer Predictor (Multi-Language)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer

-- Language System
local Language = {
    Current = "RU", -- RU / EN
    Translations = {
        RU = {
            -- Main Menu
            Title = "🧠 Steel Brainrot",
            AutoClicker = "🖱️ Авто-Кликер (10cps): ВЫКЛ",
            MoneyCollector = "💰 Авто-Подбор Денег: ВЫКЛ",
            BrainrotStealer = "🎣 Украсть Бреинроты (Grapple)",
            AntiDetect = "🛡️ Анти-Детект: ВКЛ",
            Predictor = "🔮 Проверить Дилера",
            FlyingPlatform = "🚀 Летающая Платформа: ВЫКЛ",
            IncreaseWalls = "🧱 Увеличить Стены",
            AntiCheat = "🛡️ Анти-Чит: АКТИВЕН",
            LanguageBtn = "🌐 Язык: РУССКИЙ",
            
            -- Predictor
            DealerTimer = "Таймер дилера: ",
            NextRestock = "Следующее пополнение: ",
            BaseClose = "Закрытие базы: 0 секунд",
            SuccessStolen = "✅ Успешно украдено!",
            FailedStolen = "❌ Не удалось украсть!",
            
            -- Rarities
            Rarities = {
                "Редкий",
                "Эпический", 
                "Легендарный",
                "Мифический",
                "Бог Бреинротов",
                "Секретный"
            },
            
            -- Messages
            GrappleNotFound = "❌ Grapple Hook не найден!",
            NoTargetFound = "❌ Подходящий игрок не найден в радиусе!",
            StoleSuccess = "✅ Успешно украдены бреинроты у: "
        },
        
        EN = {
            -- Main Menu
            Title = "🧠 Steel Brainrot",
            AutoClicker = "🖱️ Auto-Clicker (10cps): OFF",
            MoneyCollector = "💰 Auto Money Collect: OFF",
            BrainrotStealer = "🎣 Steal Brainrots (Grapple)",
            AntiDetect = "🛡️ Anti-Detect: ON",
            Predictor = "🔮 Check Dealer",
            FlyingPlatform = "🚀 Flying Platform: OFF",
            IncreaseWalls = "🧱 Increase Walls",
            AntiCheat = "🛡️ Anti-Cheat: ACTIVE",
            LanguageBtn = "🌐 Language: ENGLISH",
            
            -- Predictor
            DealerTimer = "Dealer timer: ",
            NextRestock = "Next restock: ",
            BaseClose = "Base close: 0 seconds",
            SuccessStolen = "✅ Successfully stolen!",
            FailedStolen = "❌ Failed to steal!",
            
            -- Rarities
            Rarities = {
                "Rare",
                "Epic", 
                "Legendary",
                "Mythical",
                "Brainrot God",
                "Secret"
            },
            
            -- Messages
            GrappleNotFound = "❌ Grapple Hook not found!",
            NoTargetFound = "❌ No suitable player found in range!",
            StoleSuccess = "✅ Successfully stolen brainrots from: "
        }
    }
}

function Language:SetLanguage(lang)
    if self.Translations[lang] then
        self.Current = lang
        UpdateUI()
    end
end

function Language:Get(textKey)
    return self.Translations[self.Current][textKey] or textKey
end

-- Anti-Cheat Bypass
local function AntiCheatBypass()
    pcall(function()
        for _, v in pairs(getconnections(game:GetService("ScriptContext").Error)) do
            v:Disable()
        end
    end)
end

-- Anti-AutoClicker Detection
local AntiAutoClicker = {
    Enabled = true,
    LastClickTime = 0,
    ClickPattern = {},
    
    CheckPattern = function(self)
        if not self.Enabled then return true end
        
        local currentTime = tick()
        local timeDiff = currentTime - self.LastClickTime
        
        -- Detect inhuman click patterns
        if timeDiff < 0.05 then -- Too fast (20+ CPS)
            return false
        end
        
        -- Store click pattern for analysis
        table.insert(self.ClickPattern, timeDiff)
        if #self.ClickPattern > 10 then
            table.remove(self.ClickPattern, 1)
        end
        
        self.LastClickTime = currentTime
        return true
    end,
    
    AddRandomDelay = function(self)
        if self.Enabled then
            wait(math.random(5, 15) / 100) -- 0.05-0.15s random delay
        end
    end
}

-- Brainrot Dealer Database with Emojis and Timer
local BrainrotDealer = {
    Brainrots = {
        {
            Name = "Cupcake Koala",
            Emoji = "🧁🐨",
            Rarity = "Редкий",
            RarityEN = "Rare",
            RarityColor = Color3.fromRGB(0, 255, 0),
            BaseChance = 25,
            Value = 100
        },
        {
            Name = "Doi Doi Do", 
            Emoji = "🎵🎵🎵",
            Rarity = "Эпический",
            RarityEN = "Epic",
            RarityColor = Color3.fromRGB(128, 0, 128),
            BaseChance = 15,
            Value = 250
        },
        {
            Name = "Clickerino Crabo",
            Emoji = "🦀👆",
            Rarity = "Легендарный",
            RarityEN = "Legendary",
            RarityColor = Color3.fromRGB(255, 165, 0),
            BaseChance = 8,
            Value = 500
        },
        {
            Name = "Stoppo Luminino",
            Emoji = "✋🌟", 
            Rarity = "Мифический",
            RarityEN = "Mythical",
            RarityColor = Color3.fromRGB(255, 0, 0),
            BaseChance = 4,
            Value = 1000
        },
        {
            Name = "Money Money Man",
            Emoji = "💰👨",
            Rarity = "Бог Бреинротов",
            RarityEN = "Brainrot God",
            RarityColor = Color3.fromRGB(255, 215, 0),
            BaseChance = 2,
            Value = 2000
        },
        {
            Name = "Noo La Polizia",
            Emoji = "👮❌",
            Rarity = "Бог Бреинротов",
            RarityEN = "Brainrot God",
            RarityColor = Color3.fromRGB(255, 215, 0),
            BaseChance = 2,
            Value = 2000
        },
        {
            Name = "Pirulitoita Bicicletaire",
            Emoji = "🍭🚲",
            Rarity = "Секретный",
            RarityEN = "Secret",
            RarityColor = Color3.fromRGB(0, 255, 255),
            BaseChance = 1,
            Value = 5000
        },
        {
            Name = "Los Puggies",
            Emoji = "🐶🐶", 
            Rarity = "Секретный",
            RarityEN = "Secret",
            RarityColor = Color3.fromRGB(0, 255, 255),
            BaseChance = 1,
            Value = 5000
        },
        {
            Name = "Los Spaghettis",
            Emoji = "🍝🇮🇹",
            Rarity = "Секретный",
            RarityEN = "Secret",
            RarityColor = Color3.fromRGB(0, 255, 255),
            BaseChance = 1,
            Value = 5000
        },
        {
            Name = "Fragrama and Chocrama",
            Emoji = "🍓🍫",
            Rarity = "Секретный",
            RarityEN = "Secret",
            RarityColor = Color3.fromRGB(0, 255, 255),
            BaseChance = 1,
            Value = 5000
        }
    },
    
    LastRestock = 0,
    RestockInterval = 30 * 60, -- 30 минут
    BaseCloseTime = 0, -- Мгновенное закрытие базы
    DealerTimer = 0, -- Таймер дилера
    
    CalculateCurrentChances = function(self)
        local currentTime = tick()
        local timeSinceRestock = currentTime - self.LastRestock
        local restockProgress = math.min(timeSinceRestock / self.RestockInterval, 1)
        
        local chances = {}
        local totalChance = 0
        
        for _, brainrot in ipairs(self.Brainrots) do
            -- Симуляция пополнения запасов со временем
            local stockMultiplier = 0.1 + (restockProgress * 0.9)
            local currentChance = math.floor(brainrot.BaseChance * stockMultiplier)
            
            table.insert(chances, {
                Name = brainrot.Name,
                Emoji = brainrot.Emoji,
                Rarity = brainrot.Rarity,
                RarityEN = brainrot.RarityEN,
                RarityColor = brainrot.RarityColor,
                Chance = currentChance,
                StockLevel = math.floor(restockProgress * 100),
                Value = brainrot.Value
            })
            
            totalChance = totalChance + currentChance
        end
        
        -- Нормализация шансов до 100%
        for _, chance in ipairs(chances) do
            chance.Chance = math.floor((chance.Chance / totalChance) * 100)
        end
        
        -- Сортировка по редкости
        table.sort(chances, function(a, b)
            local rarityOrder = {
                ["Секретный"] = 1, ["Secret"] = 1,
                ["Бог Бреинротов"] = 2, ["Brainrot God"] = 2,
                ["Мифический"] = 3, ["Mythical"] = 3,
                ["Легендарный"] = 4, ["Legendary"] = 4,
                ["Эпический"] = 5, ["Epic"] = 5,
                ["Редкий"] = 6, ["Rare"] = 6
            }
            return rarityOrder[a.Rarity] < rarityOrder[b.Rarity]
        end)
        
        return chances
    end,
    
    GetNextRestockTime = function(self)
        local nextRestock = self.LastRestock + self.RestockInterval
        local timeLeft = nextRestock - tick()
        return math.max(0, timeLeft)
    end,
    
    UpdateDealerTimer = function(self)
        self.DealerTimer = tick()
    end,
    
    GetDealerTimeLeft = function(self)
        local dealerDuration = 60 -- 60 секунд дилер
        local timeLeft = dealerDuration - (tick() - self.DealerTimer)
        return math.max(0, timeLeft)
    end,
    
    FormatTime = function(seconds)
        local minutes = math.floor(seconds / 60)
        local secs = math.floor(seconds % 60)
        return string.format("%02d:%02d", minutes, secs)
    end
}

-- Инициализация дилера
BrainrotDealer.LastRestock = tick() - math.random(0, BrainrotDealer.RestockInterval)
BrainrotDealer:UpdateDealerTimer()

-- Auto Clicker с защитой и кликом на все бреинроты
local AutoClicker = {
    Enabled = false,
    Connection = nil,
    Speed = 10, -- 10 кликов в секунду
    ClickAllBrainrots = true -- Кликать на все бреинроты
}

function AutoClicker:Toggle()
    self.Enabled = not self.Enabled
    
    if self.Enabled then
        self.Connection = RunService.Heartbeat:Connect(function()
            if AntiAutoClicker:CheckPattern() then
                if player.Character then
                    -- Кликаем на все доступные инструменты (бреинроты)
                    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
                    if humanoid then
                        for _, tool in pairs(player.Character:GetChildren()) do
                            if tool:IsA("Tool") then
                                tool:Activate()
                                -- Небольшая задержка между кликами на разные инструменты
                                wait(0.05)
                            end
                        end
                    end
                    AntiAutoClicker:AddRandomDelay()
                end
            else
                -- Слишком быстрые клики - автоматическое отключение
                self:Toggle()
                return
            end
        end)
    else
        if self.Connection then
            self.Connection:Disconnect()
            self.Connection = nil
        end
    end
end

-- Auto Money Collector
local MoneyCollector = {
    Enabled = false,
    Connection = nil,
    CollectionRange = 50,
    CollectionSpeed = 1
}

function MoneyCollector:Toggle()
    self.Enabled = not self.Enabled
    
    if self.Enabled then
        self.Connection = RunService.Heartbeat:Connect(function()
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local characterRoot = player.Character.HumanoidRootPart
                
                -- Ищем деньги в радиусе
                for _, obj in pairs(Workspace:GetDescendants()) do
                    if obj:IsA("Part") and obj.Name:lower():find("money") or obj.Name:lower():find("cash") or obj.Name:lower():find("coin") then
                        local distance = (characterRoot.Position - obj.Position).Magnitude
                        
                        if distance <= self.CollectionRange then
                            -- Подбираем деньги
                            obj.CFrame = characterRoot.CFrame
                            wait(0.1)
                        end
                    end
                end
                
                -- Ищем деньги в других игроках
                for _, otherPlayer in pairs(Players:GetPlayers()) do
                    if otherPlayer ~= player and otherPlayer.Character then
                        local otherRoot = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
                        if otherRoot then
                            local distance = (characterRoot.Position - otherRoot.Position).Magnitude
                            
                            if distance <= self.CollectionRange then
                                -- Автоматически забираем деньги у других игроков
                                characterRoot.CFrame = otherRoot.CFrame
                                wait(0.2)
                            end
                        end
                    end
                end
            end
        end)
    else
        if self.Connection then
            self.Connection:Disconnect()
            self.Connection = nil
        end
    end
end

-- Grapple Hook Brainrot Stealer
local BrainrotStealer = {
    Enabled = false,
    GrappleHook = nil,
    StealRange = 100,
    BestBrainrot = nil
}

function BrainrotStealer:FindGrappleHook()
    -- Ищем Grapple Hook в инвентаре игрока
    for _, tool in pairs(player.Backpack:GetChildren()) do
        if tool.Name:lower():find("grapple") or tool.Name:lower():find("hook") then
            self.GrappleHook = tool
            return true
        end
    end
    
    -- Ищем Grapple Hook в рабочем пространстве
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("Tool") and (obj.Name:lower():find("grapple") or obj.Name:lower():find("hook")) then
            self.GrappleHook = obj
            return true
        end
    end
    
    return false
end

function BrainrotStealer:FindBestBrainrot()
    local bestPlayer = nil
    local bestValue = 0
    local bestDistance = math.huge
    
    for _, otherPlayer in pairs(Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character then
            local otherRoot = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
            if otherRoot then
                local distance = (player.Character.HumanoidRootPart.Position - otherRoot.Position).Magnitude
                
                if distance <= self.StealRange then
                    -- Проверяем бреинроты у игрока
                    local playerValue = 0
                    for _, tool in pairs(otherPlayer.Character:GetChildren()) do
                        if tool:IsA("Tool") then
                            for _, brainrot in pairs(BrainrotDealer.Brainrots) do
                                if tool.Name:find(brainrot.Name) then
                                    playerValue = playerValue + brainrot.Value
                                    break
                                end
                            end
                        end
                    end
                    
                    -- Выбираем игрока с самыми ценными бреинротами
                    if playerValue > bestValue or (playerValue == bestValue and distance < bestDistance) then
                        bestPlayer = otherPlayer
                        bestValue = playerValue
                        bestDistance = distance
                    end
                end
            end
        end
    end
    
    return bestPlayer
end

function BrainrotStealer:StealBrainrot()
    if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then
        return false
    end
    
    -- Ищем Grapple Hook
    if not self:FindGrappleHook() then
        warn(Language:Get("GrappleNotFound"))
        return false
    end
    
    -- Ищем лучшего игрока для кражи
    local targetPlayer = self:FindBestBrainrot()
    if not targetPlayer or not targetPlayer.Character then
        warn(Language:Get("NoTargetFound"))
        return false
    end
    
    local targetRoot = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not targetRoot then
        return false
    end
    
    -- Берем Grapple Hook в руку
    self.GrappleHook.Parent = player.Character
    
    -- Используем Grapple Hook чтобы добраться до игрока
    player.Character.HumanoidRootPart.CFrame = targetRoot.CFrame + Vector3.new(0, 3, 0)
    
    -- Крадем бреинроты
    for _, tool in pairs(targetPlayer.Character:GetChildren()) do
        if tool:IsA("Tool") then
            -- Забираем инструмент
            tool.Parent = player.Backpack
            wait(0.2)
        end
    end
    
    -- Возвращаем Grapple Hook в инвентарь
    self.GrappleHook.Parent = player.Backpack
    
    print(Language:Get("StoleSuccess") .. targetPlayer.Name)
    return true
end

-- Flying Platform System
local FlyingPlatform = {
    Enabled = false,
    Platform = nil,
    Walls = {},
    Connection = nil,
    Speed = 10,
    WallHeight = 10
}

function FlyingPlatform:CreatePlatform()
    -- Удаляем старую платформу если есть
    self:RemovePlatform()
    
    -- Создаем платформу
    self.Platform = Instance.new("Part")
    self.Platform.Name = "FlyingPlatform"
    self.Platform.Size = Vector3.new(10, 1, 10)
    self.Platform.Position = player.Character and player.Character:GetPivot().Position + Vector3.new(0, 5, 0) or Vector3.new(0, 10, 0)
    self.Platform.Anchored = true
    self.Platform.CanCollide = true
    self.Platform.Material = Enum.Material.Neon
    self.Platform.BrickColor = BrickColor.new("Bright blue")
    self.Platform.Parent = Workspace
    
    -- Создаем стены
    self:CreateWalls()
    
    -- Телепортируем игрока на платформу
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.Position = self.Platform.Position + Vector3.new(0, 3, 0)
    end
end

function FlyingPlatform:CreateWalls()
    -- Удаляем старые стены
    for _, wall in pairs(self.Walls) do
        wall:Destroy()
    end
    self.Walls = {}
    
    local positions = {
        Vector3.new(5, self.WallHeight/2, 0),   -- Правая
        Vector3.new(-5, self.WallHeight/2, 0),  -- Левая
        Vector3.new(0, self.WallHeight/2, 5),   -- Передняя
        Vector3.new(0, self.WallHeight/2, -5)   -- Задняя
    }
    
    local sizes = {
        Vector3.new(1, self.WallHeight, 10),   -- Правая/Левая
        Vector3.new(10, self.WallHeight, 1)    -- Передняя/Задняя
    }
    
    for i, pos in ipairs(positions) do
        local wall = Instance.new("Part")
        wall.Name = "PlatformWall"
        wall.Size = sizes[i <= 2 and 1 or 2]
        wall.Position = self.Platform.Position + pos
        wall.Anchored = true
        wall.CanCollide = true
        wall.Material = Enum.Material.Neon
        wall.BrickColor = BrickColor.new("Bright red")
        wall.Transparency = 0.3
        wall.Parent = Workspace
        
        table.insert(self.Walls, wall)
    end
end

function FlyingPlatform:IncreaseWalls()
    self.WallHeight = self.WallHeight + 5
    if self.Platform then
        self:CreateWalls()
    end
end

function FlyingPlatform:Toggle()
    self.Enabled = not self.Enabled
    
    if self.Enabled then
        self:CreatePlatform()
        
        -- Движение платформы вверх
        self.Connection = RunService.Heartbeat:Connect(function()
            if self.Platform then
                self.Platform.Position = self.Platform.Position + Vector3.new(0, self.Speed * 0.1, 0)
                
                -- Обновляем позицию стен
                for _, wall in pairs(self.Walls) do
                    local offset = wall.Position - self.Platform.Position
                    wall.Position = self.Platform.Position + Vector3.new(offset.X, self.WallHeight/2, offset.Z)
                end
                
                -- Телепортируем игрока если он на платформе
                if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    local charPos = player.Character.HumanoidRootPart.Position
                    local platformPos = self.Platform.Position
                    local distance = (charPos - platformPos).Magnitude
                    
                    if distance < 20 then -- Если игрок близко к платформе
                        player.Character.HumanoidRootPart.Position = platformPos + Vector3.new(0, 3, 0)
                    end
                end
            end
        end)
    else
        if self.Connection then
            self.Connection:Disconnect()
            self.Connection = nil
        end
        self:RemovePlatform()
    end
end

function FlyingPlatform:RemovePlatform()
    if self.Platform then
        self.Platform:Destroy()
        self.Platform = nil
    end
    for _, wall in pairs(self.Walls) do
        wall:Destroy()
    end
    self.Walls = {}
end

-- Mobile-Optimized UI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "SteelBrainrotCheat"
screenGui.ResetOnSpawn = false
screenGui.Parent = game.CoreGui

-- Main Menu Frame
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 350, 0, 600)
mainFrame.Position = UDim2.new(0.5, -175, 0.5, -300)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 8)
mainCorner.Parent = mainFrame

-- Title Bar
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.Position = UDim2.new(0, 0, 0, 0)
titleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

local titleText = Instance.new("TextLabel")
titleText.Size = UDim2.new(0.7, 0, 1, 0)
titleText.Position = UDim2.new(0.05, 0, 0, 0)
titleText.Text = Language:Get("Title")
titleText.TextColor3 = Color3.fromRGB(255, 255, 255)
titleText.TextSize = 16
titleText.BackgroundTransparency = 1
titleText.TextXAlignment = Enum.TextXAlignment.Left
titleText.Parent = titleBar

-- Close Button
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -35, 0, 5)
closeButton.Text = "✕"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 18
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.BorderSizePixel = 0
closeButton.Parent = titleBar

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(1, 0)
closeCorner.Parent = closeButton

-- Content Area
local contentFrame = Instance.new("Frame")
contentFrame.Size = UDim2.new(1, 0, 1, -40)
contentFrame.Position = UDim2.new(0, 0, 0, 40)
contentFrame.BackgroundTransparency = 1
contentFrame.Parent = mainFrame

-- Language Button
local languageBtn = Instance.new("TextButton")
languageBtn.Size = UDim2.new(0.9, 0, 0, 40)
languageBtn.Position = UDim2.new(0.05, 0, 0, 10)
languageBtn.Text = Language:Get("LanguageBtn")
languageBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
languageBtn.TextSize = 14
languageBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 150)
languageBtn.BorderSizePixel = 0
languageBtn.Parent = contentFrame

local languageCorner = Instance.new("UICorner")
languageCorner.CornerRadius = UDim.new(0, 6)
languageCorner.Parent = languageBtn

-- Auto Clicker Button
local autoClickerBtn = Instance.new("TextButton")
autoClickerBtn.Size = UDim2.new(0.9, 0, 0, 40)
autoClickerBtn.Position = UDim2.new(0.05, 0, 0, 60)
autoClickerBtn.Text = Language:Get("AutoClicker")
autoClickerBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
autoClickerBtn.TextSize = 14
autoClickerBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
autoClickerBtn.BorderSizePixel = 0
autoClickerBtn.Parent = contentFrame

local clickerCorner = Instance.new("UICorner")
clickerCorner.CornerRadius = UDim.new(0, 6)
clickerCorner.Parent = autoClickerBtn

-- Money Collector Button
local moneyCollectorBtn = Instance.new("TextButton")
moneyCollectorBtn.Size = UDim2.new(0.9, 0, 0, 40)
moneyCollectorBtn.Position = UDim2.new(0.05, 0, 0, 110)
moneyCollectorBtn.Text = Language:Get("MoneyCollector")
moneyCollectorBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
moneyCollectorBtn.TextSize = 14
moneyCollectorBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
moneyCollectorBtn.BorderSizePixel = 0
moneyCollectorBtn.Parent = contentFrame

local moneyCorner = Instance.new("UICorner")
moneyCorner.CornerRadius = UDim.new(0, 6)
moneyCorner.Parent = moneyCollectorBtn

-- Brainrot Stealer Button
local stealerBtn = Instance.new("TextButton")
stealerBtn.Size = UDim2.new(0.9, 0, 0, 40)
stealerBtn.Position = UDim2.new(0.05, 0, 0, 160)
stealerBtn.Text = Language:Get("BrainrotStealer")
stealerBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
stealerBtn.TextSize = 14
stealerBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
stealerBtn.BorderSizePixel = 0
stealerBtn.Parent = contentFrame

local stealerCorner = Instance.new("UICorner")
stealerCorner.CornerRadius = UDim.new(0, 6)
stealerCorner.Parent = stealerBtn

-- Anti-AutoClicker Button
local antiAutoClickerBtn = Instance.new("TextButton")
antiAutoClickerBtn.Size = UDim2.new(0.9, 0, 0, 40)
antiAutoClickerBtn.Position = UDim2.new(0.05, 0, 0, 210)
antiAutoClickerBtn.Text = Language:Get("AntiDetect")
antiAutoClickerBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
antiAutoClickerBtn.TextSize = 14
antiAutoClickerBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
antiAutoClickerBtn.BorderSizePixel = 0
antiAutoClickerBtn.Parent = contentFrame

local antiAutoClickerCorner = Instance.new("UICorner")
antiAutoClickerCorner.CornerRadius = UDim.new(0, 6)
antiAutoClickerCorner.Parent = antiAutoClickerBtn

-- Predictor Button
local predictorBtn = Instance.new("TextButton")
predictorBtn.Size = UDim2.new(0.9, 0, 0, 40)
predictorBtn.Position = UDim2.new(0.05, 0, 0, 260)
predictorBtn.Text = Language:Get("Predictor")
predictorBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
predictorBtn.TextSize = 14
predictorBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
predictorBtn.BorderSizePixel = 0
predictorBtn.Parent = contentFrame

local predictorCorner = Instance.new("UICorner")
predictorCorner.CornerRadius = UDim.new(0, 6)
predictorCorner.Parent = predictorBtn

-- Flying Platform Button
local platformBtn = Instance.new("TextButton")
platformBtn.Size = UDim2.new(0.9, 0, 0, 40)
platformBtn.Position = UDim2.new(0.05, 0, 0, 310)
platformBtn.Text = Language:Get("FlyingPlatform")
platformBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
platformBtn.TextSize = 14
platformBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
platformBtn.BorderSizePixel = 0
platformBtn.Parent = contentFrame

local platformCorner = Instance.new("UICorner")
platformCorner.CornerRadius = UDim.new(0, 6)
platformCorner.Parent = platformBtn

-- Increase Walls Button
local wallsBtn = Instance.new("TextButton")
wallsBtn.Size = UDim2.new(0.9, 0, 0, 40)
wallsBtn.Position = UDim2.new(0.05, 0, 0, 360)
wallsBtn.Text = Language:Get("IncreaseWalls")
wallsBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
wallsBtn.TextSize = 14
wallsBtn.BackgroundColor3 = Color3.fromRGB(100, 50, 0)
wallsBtn.BorderSizePixel = 0
wallsBtn.Parent = contentFrame

local wallsCorner = Instance.new("UICorner")
wallsCorner.CornerRadius = UDim.new(0, 6)
wallsCorner.Parent = wallsBtn

-- Anti-Cheat Button
local antiCheatBtn = Instance.new("TextButton")
antiCheatBtn.Size = UDim2.new(0.9, 0, 0, 40)
antiCheatBtn.Position = UDim2.new(0.05, 0, 0, 410)
antiCheatBtn.Text = Language:Get("AntiCheat")
antiCheatBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
antiCheatBtn.TextSize = 14
antiCheatBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
antiCheatBtn.BorderSizePixel = 0
antiCheatBtn.Parent = contentFrame

local antiCheatCorner = Instance.new("UICorner")
antiCheatCorner.CornerRadius = UDim.new(0, 6)
antiCheatCorner.Parent = antiCheatBtn

-- Prediction Display
local predictionFrame = Instance.new("Frame")
predictionFrame.Size = UDim2.new(0.9, 0, 0, 220)
predictionFrame.Position = UDim2.new(0.05, 0, 0, 460)
predictionFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
predictionFrame.BorderSizePixel = 0
predictionFrame.Visible = false
predictionFrame.Parent = contentFrame

local predictionCorner = Instance.new("UICorner")
predictionCorner.CornerRadius = UDim.new(0, 6)
predictionCorner.Parent = predictionFrame

local predictionScroll = Instance.new("ScrollingFrame")
predictionScroll.Size = UDim2.new(0.95, 0, 0.9, 0)
predictionScroll.Position = UDim2.new(0.025, 0, 0.05, 0)
predictionScroll.BackgroundTransparency = 1
predictionScroll.BorderSizePixel = 0
predictionScroll.ScrollBarThickness = 4
predictionScroll.Parent = predictionFrame

-- Function to update UI language
function UpdateUI()
    titleText.Text = Language:Get("Title")
    languageBtn.Text = Language:Get("LanguageBtn")
    autoClickerBtn.Text = Language:Get("AutoClicker"):gsub(": .*", ": " .. (AutoClicker.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    moneyCollectorBtn.Text = Language:Get("MoneyCollector"):gsub(": .*", ": " .. (MoneyCollector.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    stealerBtn.Text = Language:Get("BrainrotStealer")
    antiAutoClickerBtn.Text = Language:Get("AntiDetect"):gsub(": .*", ": " .. (AntiAutoClicker.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    predictorBtn.Text = Language:Get("Predictor")
    platformBtn.Text = Language:Get("FlyingPlatform"):gsub(": .*", ": " .. (FlyingPlatform.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    wallsBtn.Text = Language:Get("IncreaseWalls")
    antiCheatBtn.Text = Language:Get("AntiCheat")
    
    if predictionFrame.Visible then
        UpdatePredictionDisplay()
    end
end

-- Button Handlers
languageBtn.MouseButton1Click:Connect(function()
    if Language.Current == "RU" then
        Language:SetLanguage("EN")
    else
        Language:SetLanguage("RU")
    end
end)

autoClickerBtn.MouseButton1Click:Connect(function()
    AutoClicker:Toggle()
    autoClickerBtn.Text = Language:Get("AutoClicker"):gsub(": .*", ": " .. (AutoClicker.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    autoClickerBtn.BackgroundColor3 = AutoClicker.Enabled and Color3.fromRGB(0, 100, 0) or Color3.fromRGB(50, 50, 50)
end)

moneyCollectorBtn.MouseButton1Click:Connect(function()
    MoneyCollector:Toggle()
    moneyCollectorBtn.Text = Language:Get("MoneyCollector"):gsub(": .*", ": " .. (MoneyCollector.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    moneyCollectorBtn.BackgroundColor3 = MoneyCollector.Enabled and Color3.fromRGB(0, 100, 0) or Color3.fromRGB(50, 50, 50)
end)

stealerBtn.MouseButton1Click:Connect(function()
    local success = BrainrotStealer:StealBrainrot()
    if success then
        stealerBtn.Text = Language:Get("SuccessStolen")
        stealerBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
        wait(2)
        stealerBtn.Text = Language:Get("BrainrotStealer")
        stealerBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
    else
        stealerBtn.Text = Language:Get("FailedStolen")
        stealerBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
        wait(2)
        stealerBtn.Text = Language:Get("BrainrotStealer")
        stealerBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
    end
end)

antiAutoClickerBtn.MouseButton1Click:Connect(function()
    AntiAutoClicker.Enabled = not AntiAutoClicker.Enabled
    antiAutoClickerBtn.Text = Language:Get("AntiDetect"):gsub(": .*", ": " .. (AntiAutoClicker.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    antiAutoClickerBtn.BackgroundColor3 = AntiAutoClicker.Enabled and Color3.fromRGB(0, 100, 0) or Color3.fromRGB(100, 0, 0)
end)

predictorBtn.MouseButton1Click:Connect(function()
    predictionFrame.Visible = not predictionFrame.Visible
    
    if predictionFrame.Visible then
        UpdatePredictionDisplay()
    end
end)

platformBtn.MouseButton1Click:Connect(function()
    FlyingPlatform:Toggle()
    platformBtn.Text = Language:Get("FlyingPlatform"):gsub(": .*", ": " .. (FlyingPlatform.Enabled and (Language.Current == "RU" and "ВКЛ" or "ON") or (Language.Current == "RU" and "ВЫКЛ" or "OFF")))
    platformBtn.BackgroundColor3 = FlyingPlatform.Enabled and Color3.fromRGB(0, 100, 0) or Color3.fromRGB(50, 50, 50)
end)

wallsBtn.MouseButton1Click:Connect(function()
    FlyingPlatform:IncreaseWalls()
    wallsBtn.Text = Language:Get("IncreaseWalls")
end)

antiCheatBtn.MouseButton1Click:Connect(function()
    AntiCheatBypass()
    antiCheatBtn.Text = Language:Get("AntiCheat"):gsub(": .*", ": " .. (Language.Current == "RU" and "ОБОЙДЕН" or "BYPASSED"))
    antiCheatBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
end)

closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
    FlyingPlatform:RemovePlatform()
    if AutoClicker.Connection then
        AutoClicker.Connection:Disconnect()
    end
    if MoneyCollector.Connection then
        MoneyCollector.Connection:Disconnect()
    end
end)

function UpdatePredictionDisplay()
    local chances = BrainrotDealer:CalculateCurrentChances()
    local nextRestock = BrainrotDealer:GetNextRestockTime()
    local dealerTimeLeft = BrainrotDealer:GetDealerTimeLeft()
    
    predictionScroll:ClearAllChildren()
    
    local totalHeight = 0
    local yPosition = 5

    -- Таймер дилера
    local dealerTimerLabel = Instance.new("TextLabel")
    dealerTimerLabel.Size = UDim2.new(1, 0, 0, 20)
    dealerTimerLabel.Position = UDim2.new(0, 0, 0, yPosition)
    dealerTimerLabel.Text = Language:Get("DealerTimer") .. BrainrotDealer:FormatTime(dealerTimeLeft)
    dealerTimerLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
    dealerTimerLabel.TextSize = 12
    dealerTimerLabel.BackgroundTransparency = 1
    dealerTimerLabel.TextXAlignment = Enum.TextXAlignment.Left
    dealerTimerLabel.Parent = predictionScroll
    
    yPosition = yPosition + 25
    totalHeight = totalHeight + 25

    -- Информация о пополнении
    local restockLabel = Instance.new("TextLabel")
    restockLabel.Size = UDim2.new(1, 0, 0, 20)
    restockLabel.Position = UDim2.new(0, 0, 0, yPosition)
    restockLabel.Text = Language:Get("NextRestock") .. BrainrotDealer:FormatTime(nextRestock)
    restockLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    restockLabel.TextSize = 12
    restockLabel.BackgroundTransparency = 1
    restockLabel.TextXAlignment = Enum.TextXAlignment.Left
    restockLabel.Parent = predictionScroll
    
    yPosition = yPosition + 25
    totalHeight = totalHeight + 25

    local closeTimeLabel = Instance.new("TextLabel")
    closeTimeLabel.Size = UDim2.new(1, 0, 0, 20)
    closeTimeLabel.Position = UDim2.new(0, 0, 0, yPosition)
    closeTimeLabel.Text = Language:Get("BaseClose")
    closeTimeLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
    closeTimeLabel.TextSize = 12
    closeTimeLabel.BackgroundTransparency = 1
    closeTimeLabel.TextXAlignment = Enum.TextXAlignment.Left
    closeTimeLabel.Parent = predictionScroll
    
    yPosition = yPosition + 25
    totalHeight = totalHeight + 25
    
    for i, brainrot in ipairs(chances) do
        local brainrotFrame = Instance.new("Frame")
        brainrotFrame.Size = UDim2.new(1, 0, 0, 30)
        brainrotFrame.Position = UDim2.new(0, 0, 0, yPosition)
        brainrotFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
        brainrotFrame.BorderSizePixel = 0
        brainrotFrame.Parent = predictionScroll
        
        local emojiLabel = Instance.new("TextLabel")
        emojiLabel.Size = UDim2.new(0.1, 0, 1, 0)
        emojiLabel.Position = UDim2.new(0, 5, 0, 0)
        emojiLabel.Text = brainrot.Emoji
        emojiLabel.TextColor3 = brainrot.RarityColor
        emojiLabel.TextSize = 14
        emojiLabel.BackgroundTransparency = 1
        emojiLabel.Parent = brainrotFrame
        
        local nameLabel = Instance.new("TextLabel")
        nameLabel.Size = UDim2.new(0.5, 0, 1, 0)
        nameLabel.Position = UDim2.new(0.1, 0, 0, 0)
        nameLabel.Text = brainrot.Name
        nameLabel.TextColor3 = brainrot.RarityColor
        nameLabel.TextSize = 11
        nameLabel.BackgroundTransparency = 1
        nameLabel.TextXAlignment = Enum.TextXAlignment.Left
        nameLabel.Parent = brainrotFrame
        
        local rarityLabel = Instance.new("TextLabel")
        rarityLabel.Size = UDim2.new(0.25, 0, 1, 0)
        rarityLabel.Position = UDim2.new(0.6, 0, 0, 0)
        rarityLabel.Text = Language.Current == "RU" and brainrot.Rarity or brainrot.RarityEN
        rarityLabel.TextColor3 = brainrot.RarityColor
        rarityLabel.TextSize = 10
        rarityLabel.BackgroundTransparency = 1
        rarityLabel.Parent = brainrotFrame
        
        local chanceLabel = Instance.new("TextLabel")
        chanceLabel.Size = UDim2.new(0.15, 0, 1, 0)
        chanceLabel.Position = UDim2.new(0.85, 0, 0, 0)
        chanceLabel.Text = brainrot.Chance .. "%"
        chanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        chanceLabel.TextSize = 11
        chanceLabel.BackgroundTransparency = 1
        chanceLabel.Parent = brainrotFrame
        
        yPosition = yPosition + 35
        totalHeight = totalHeight + 35
    end
    
    predictionScroll.CanvasSize = UDim2.new(0, 0, 0, totalHeight)
end

-- Mobile drag functionality
local dragging = false
local dragInput, dragStart, startPos

titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
    end
end)

titleBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

titleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input == dragInput) then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Автоматическое обновление предсказаний
spawn(function()
    while true do
        if predictionFrame.Visible then
            UpdatePredictionDisplay()
        end
        wait(1) -- Обновление каждую секунду для таймера
    end
end)

-- Обновление таймера дилера каждую секунду
spawn(function()
    while true do
        BrainrotDealer:UpdateDealerTimer()
        wait(1)
    end
end)

-- Инициализация
AntiCheatBypass()

-- Многоязычные сообщения загрузки
local loadMessages = {
    RU = {
        "✅ Steel Brainrot Чит загружен!",
        "🖱️ Авто-Кликер 10cps готов (кликает все бреинроты)",
        "💰 Авто-подбор денег активирован",
        "🎣 Grapple Hook вор бреинротов готов", 
        "🚀 Летающая платформа активирована",
        "🧱 Система стен добавлена",
        "⏰ Таймер дилера запущен",
        "🛡️ Анти-Детект активирован",
        "🔮 Предиктор дилера запущен",
        "⚡ Закрытие базы: 0 секунд",
        "🌐 Многоязычный интерфейс активирован"
    },
    EN = {
        "✅ Steel Brainrot Cheat loaded!",
        "🖱️ Auto-Clicker 10cps ready (clicks all brainrots)",
        "💰 Auto money collection activated", 
        "🎣 Grapple Hook brainrot thief ready",
        "🚀 Flying platform activated",
        "🧱 Wall system added",
        "⏰ Dealer timer started",
        "🛡️ Anti-Detect activated",
        "🔮 Dealer predictor started",
        "⚡ Base close: 0 seconds",
        "🌐 Multi-language interface activated"
    }
}

for _, message in ipairs(loadMessages[Language.Current]) do
    print(message)
end
