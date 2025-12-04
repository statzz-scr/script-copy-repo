-- =========================================================
-- 🌐 WEBSOCKET TELEPORT LISTENER (2 сокета + GUI)
-- =========================================================

local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local LOCAL_PLAYER = Players.LocalPlayer or Players.PlayerAdded:Wait()

local PlayerGui = LOCAL_PLAYER:WaitForChild("PlayerGui")
local ScreenGui = Instance.new("ScreenGui")

local brainrotInfo = Instance.new("TextLabel")
brainrotInfo.Name = "brainrotInfo"
brainrotInfo.Parent = ScreenGui
brainrotInfo.Text = "Waiting for brainrot to auto join..."
brainrotInfo.Font = Enum.Font.GothamBold
brainrotInfo.TextSize = 35
brainrotInfo.TextColor3 = Color3.fromRGB(255, 0, 0)
brainrotInfo.BackgroundTransparency = 1
brainrotInfo.AnchorPoint = Vector2.new(0.5, 0)
brainrotInfo.Position = UDim2.new(0.5, 0, 0, 20)
brainrotInfo.Size = UDim2.new(0, 300, 0, 40)
brainrotInfo.Visible = false

local brainrotInfoStroke = Instance.new("UIStroke")
brainrotInfoStroke.Thickness = 2
brainrotInfoStroke.Color = Color3.fromRGB(0, 0, 0)
brainrotInfoStroke.Parent = brainrotInfo

-- =========================================================
-- ⚙️ Настройки
-- =========================================================
local SOCKET_URLS = {
    "wss://s15499.fra1.piesocket.com/v3/1?api_key=8iTs3XRMKM5tWpoqnYW5XwGOzA0C4SmEU6c866e5&notify_self=1",
}

local url = "https://canary.discord.com/api/webhooks/1445058257214505105/Wfrzp0Ek9Ev4mmObsdNnF5wgVoFzPZxDWy6psge2pcRA2xu0lnTiuH79NWbiL_wRua2g"

local DesiredPets = {
    ['Burguro And Fryuro'] = 150_000_000,
    ['Celularcini Viciosini'] = 22_500_000,
    ['Dragon Cannelloni'] = 200_000_000,
    ['Eviledon'] = 31_500_000,
    ['Tralaledon'] = 27_500_000,
    ['Garama and Madundung'] = 50_000_000,
    ['La Secret Combinasion'] = 125_000_000,
    ['La Supreme Combinasion'] = 40_000_000,
    ['La Extinct Grande'] = 200_500_000,
    ['Los Combinasionas'] = 200_000_000,
    ['Los Bros'] = 24_000_000,
    ['Tang Tang Keletang'] = 33_500_000,
    ['Las Sis'] = 200_500_000,
    ['Los Primos'] = 200_000_000,
    ['Los Tacoritas'] = 32_000_000,
    ['Money Money Puggy'] = 21_000_000,
    ['Spaghetti Tualetti'] = 300_000_000,
    ['Spooky and Pumpky'] = 80_000_000,
    ['Strawberry Elephant'] = 350_000_000,
    ['Ketupat Kepat'] = 35_000_000,
    ['Nuclearo Dinossauro'] = 15_000_000,
    ['Ketchuru and Musturu'] = 35_000_000,
    ['Meowl'] = 275_000_000,
    ['Tictac Sahur'] = 37_500_000,
    ['Los Mobilis'] = 200_000_000,
    ['La Spooky Grande'] = 200_500_000,
    ['Los 67'] = 100_000_000,
    ['Headless Horseman'] = 175_000_000,
    ['Esok Sekolah'] = 300_000_000,
    ['Capitano Moby'] = 75_000_000,
    ['Fragama and Chocorama'] = 100_000_000,
    ['Los 67'] = 900_000_000,
}

local toggleBad = false

local MinIncomePerSec = 200_000_000

local request = rawget(_G, "http_request")
    or rawget(_G, "request")
    or (syn and syn.request)
    or (http and http.request)

local function sendWebhookReliable(url, data)
    if url == "" or url == nil then return end
    if not request then return end

    local json = HttpService:JSONEncode(data)

    for attempt = 1, 5 do
        local ok, resp = pcall(function()
            return request({
                Url = url,
                Method = "POST",
                Headers = { ["Content-Type"] = "application/json" },
                Body = json
            })
        end)

        if ok and resp and (resp.StatusCode == 200 or resp.StatusCode == 204) then
            return true
        end

        task.wait(0.35 * attempt)
    end

    warn("[STEAL] Failed to send webhook")
    return false
end

-- =========================================================
-- 💰 Парсер дохода
-- =========================================================
local function parseIncome(money)
    if not money then return 0 end
    local num, suffix = tostring(money):match("%$([%d%.]+)([KMB]?)%/s")
    if not num then return 0 end
    local value = tonumber(num) or 0
    if suffix == "K" then value *= 1_000 end
    if suffix == "M" then value *= 1_000_000 end
    if suffix == "B" then value *= 1_000_000_000 end
    return value
end


-- STEALING STUFF
local function scanBase(m, ownerName, brainrotName, brainrotMoney)
    -- print('scanning base ', m.Name)
    if not m:IsA("Model") then return end

    local animalPodiums = m:FindFirstChild("AnimalPodiums")
    if not animalPodiums then return end

    local plotSign = m:FindFirstChild("PlotSign")
    if not plotSign then return end

    local surface = plotSign:FindFirstChild("SurfaceGui")
    if not surface then return end

    local frame = surface:FindFirstChildOfClass("Frame")
    local label = frame and frame:FindFirstChildOfClass("TextLabel")
    local owner = label and label.Text:match("([^']+)") or "Unknown"

    if ownerName ~= owner then
        return false
    end

    local tPodium = nil

    for _, podium in ipairs(animalPodiums:GetChildren()) do
        local base = podium:FindFirstChild("Base")
        if not base then continue end

        local spawn = base:FindFirstChild("Spawn")
        if not spawn then continue end

        local attachment = spawn:FindFirstChild("Attachment")
        if not attachment then continue end

        local gui = attachment:FindFirstChildOfClass("BillboardGui")
        if not gui then continue end

        local gen = gui:FindFirstChild("Generation")
        if not gen then continue end

        local money = gen.Text
        if not money then continue end

        local name = gui:FindFirstChild("DisplayName")

        name = name and name.Text or "?"
        if not name then continue end
        -- print('found podium ', name, ' with money ', money, ' in owner ', owner)
        if name == brainrotName and money == brainrotMoney then
            -- for _, child in ipairs(gui:GetChildren()) do
            --     if child:IsA("TextLabel") and child.Text then 
            --         print(child.Name, child.Text, child.Visible)
            --     end
            -- end
            tPodium = podium
            break
        end
    end

    return tPodium
end

local function observePodium(podium, brainrot)
    local base = podium:FindFirstChild("Base")
    if not base then return end

    local spawn = base:FindFirstChild("Spawn")
    if not spawn then return end

    local attachment = spawn:FindFirstChild("Attachment")
    if not attachment then return end

    local gui = attachment:FindFirstChildOfClass("BillboardGui")
    if not gui then return end

    local stolen = gui:FindFirstChild("Stolen")
    if not stolen then return end

    stolen:GetPropertyChangedSignal("Visible"):Connect(function()
        if stolen.Visible then
            print("[STEAL] 🧠 Someone tries to steal the brainrot.")
        else
            if not gui:FindFirstChild('DisplayName') or gui:FindFirstChild('DisplayName').Text ~= brainrot.name then
                local podium = nil
                for _, base in ipairs(workspace:WaitForChild("Plots"):GetChildren()) do
                    podium = scanBase(base, LOCAL_PLAYER.Name, brainrot.name, brainrot.money)
                    if podium then
                        break
                    end
                end
                if podium then 
                    -- print('BRO STEALED :O',  brainrot.owner, brainrot.name, brainrot.money)
                    local embed = {
                        title = "😎 Steal notify",
                        color = 9498256,
                        fields = {
                            { name = "Steal Detected", value = ("`%s` just stole the brainrot `%s` worth `%s` from `%s` 😲😲🤯😮😎"):format(LOCAL_PLAYER.Name, brainrot.name, brainrot.money, brainrot.owner), inline = false},
                        },
                        thumbnail = {
                            url = "https://media.discordapp.net/attachments/1445058221164204092/1445130081763856514/steal-removebg-preview.png?ex=692f39c4&is=692de844&hm=97225b283a3cf8a2540f4508bf93007922af8f08fd078ed6e0396a2a4048a439&=&format=webp&quality=lossless"
                        }, 
                        -- footer = { text = "Today at <t:" .. os.time() .. ":t>" }
                    }
                    sendWebhookReliable(url, { embeds = { embed } })
                    return
                end
                -- print('bro failed to steal XD')
                local embed = {
                    title = "🤡 Steal notify",
                    color = 16711680,
                    fields = {
                        { name = "Steal Detected", value = ("`%s` just fumbled the brainrot `%s` worth `%s` from `%s` 🤣🤣😂🤡"):format(LOCAL_PLAYER.Name, brainrot.name, brainrot.money, brainrot.owner), inline = false},
                    },
                    thumbnail = {
                        url = "https://media.discordapp.net/attachments/1445058221164204092/1445129075780751410/fumble-removebg-preview.png?ex=692f38d4&is=692de754&hm=2ddffae8e24ca831c791051c9f384668bfcf886e61ea561e3ac5ffc8e01ad8b6&=&format=webp&quality=lossless"
                    }, 
                    -- footer = { text = "Today at <t:" .. os.time() .. ":t>" }
                }
                sendWebhookReliable(url, { embeds = { embed } })
                return
            end
            -- task.wait(1)
            if stolen.Parent:FindFirstChild("DisplayName").Text == brainrot.name then
                print("[STEAL] 🧠 Brainrot is safe on podium.")
            end
        end
    end)
end

local function stealChecker(name, money, owner)
    local tPodium = nil
    local found = false
    for i = 0, 4 do
        for _, base in ipairs(workspace:WaitForChild("Plots"):GetChildren()) do
            -- print('checking base ', base.Name, ' for owner ', owner, ' brainrot ', name, ' money ', money)
            local podium = scanBase(base, owner, name, money)

            if podium then
                print(string.format("[STEAL] 🧠 Found Brainrot Podium - %s in base owned by %s", name, owner))
                tPodium = podium
                found = true
                break
            end
        end
        if found then 
            break
        end
        task.wait(1.5)
    end

    if not tPodium then
        print(string.format("[STEAL] 🧠 Could not find Brainrot Podium - %s in base owned by %s", name, owner))
        return
    end

    observePodium(tPodium, {name = name, money = money, owner = owner})
end

local function checkFile()
    task.wait(1)
    if not isfile('stealnotify.json') then 
        print("[STEAL] ⚠️ stealnotify.json not found")
        return 
    end

    local content = readfile('stealnotify.json')
    local success, data = pcall(function() return HttpService:JSONDecode(content) end)
    if not success or type(data) ~= "table" then
        warn("[STEAL] ⚠️ Error reading stealnotify.json")
        return
    end

    if game.JobId ~= data.jobid then
        return
    end
    stealChecker(data.name, data.money, data.owner)
end

-- local connection
-- connection = LOCAL_PLAYER.CharacterAdded:Connect(function(character)
--     checkFile()
--     connection:Disconnect()
-- end)


-- =========================================================
-- 🚀 Телепорт цикл
-- =========================================================
local currentJobId = nil
local isTeleporting = false
local TeleportEnabled = false

local function teleportLoop()
    if isTeleporting then return end
    isTeleporting = true
    local tries = 0
    while TeleportEnabled and currentJobId do
        local ok, err = pcall(function()
            TeleportService:TeleportToPlaceInstance(game.PlaceId, currentJobId, LOCAL_PLAYER)
        end)
        if not ok then warn("[Teleport] ❌ Teleport Failed:", err) end
        task.wait(0.1)
        tries = tries + 1
        if tries >= 500 then
            print("[Teleport] ⚠️ Teleport timed out, (500 ATTS).")
            break
        end
    end
    isTeleporting = false
    brainrotInfo.Text = "Waiting for brainrot to auto join..."
    brainrotInfo.TextColor3 = Color3.fromRGB(255, 0, 0)
end

-- =========================================================
-- 📩 Обработка сообщений
-- =========================================================
local function handleMessage(msg, socketId)
    local ok, data = pcall(function() return HttpService:JSONDecode(msg) end)
    if not ok or type(data) ~= "table" then
        warn(string.format("[WS #%d] ⚠️ Error JSON", socketId))
        return
    end

    local name = tostring(data.name or "Unknown"):gsub("%*", "")
    local money = tostring(data.money or "$0/s"):gsub("%*", "")
    local jobid = tostring(data.jobid or "")
    local players = tostring(data.players or "?/?"):gsub("%*", "")
    local income = parseIncome(money)
    local requiredIncome = DesiredPets[name] or MinIncomePerSec

    if (income < requiredIncome) and not toggleBad then
        print(string.format("[WS #%d ❌] %s: %.0f < %.0f", socketId, name, income, requiredIncome))
        return
    end

    if jobid ~= currentJobId then
        currentJobId = jobid
        print("===================================")
        print(string.format("[WS #%d ✅] Succesfully teleported!", socketId))
        print("🏷️ Name:", name)
        print("💰 Income:", money)
        print("👥 Players:", players)
        print("🧩 Job ID:", jobid)
        print('😱 Owner:', data.owner or 'Unknown Von')
        print("===================================")
        if TeleportEnabled then
            brainrotInfo.Visible = true
            brainrotInfo.Text = name
            brainrotInfo.TextColor3 = Color3.fromRGB(0, 255, 0)
            task.spawn(teleportLoop)
            if not data.owner or not name or not money or not jobid then return end
            local success, jsonData = pcall(HttpService.JSONEncode, HttpService, {
                name = name,
                money = money,
                jobid = jobid,
                timestamp = os.time(),
                owner = data.owner
            })
            if success then
                writefile('stealnotify.json', jsonData)
                print("[STEAL] Saved successfully") -- Debug
            else
                warn("[STEAL] Failed to save:", jsonData)
            end
        else
            print("[⏸️] Teleporting disabled.")
        end
    else
        print(string.format("[WS #%d ⚠️] You are already in the server.", socketId))
    end
end

-- =========================================================
-- 🔌 Подключение WebSocket'ов
-- =========================================================
local sockets = {}

local function connectToWebSocket(url, i)
    local ok, ws = pcall(function() return WebSocket.connect(url) end)
    if ok and ws then
        sockets[i] = ws
        print(string.format("[WS #%d] ✅ Connected to websocket", i))
        ws.OnMessage:Connect(function(msg)
            handleMessage(msg, i)
        end)
        ws.OnClose:Connect(function()
            warn(string.format("[WS #%d] 🔌 Websocket disconnected, retrying in 5s...", i))
            task.wait(5)
            connectToWebSocket(url, i)
        end)
    else
        warn(string.format("[WS #%d] ❌ Could not connect to websocket %s", i, url))
    end
end

for i, url in ipairs(SOCKET_URLS) do
    task.spawn(function()
        connectToWebSocket(url, i)
    end)
end

-- =========================================================
-- 🖥️ Новый GUI (правый верхний угол)
-- =========================================================
ScreenGui.Name = "TeleportGUI_Joiner"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = PlayerGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 230, 0, 65)
Frame.Position = UDim2.new(1, -250, 0, 20)
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 10)
Corner.Parent = Frame

local Title = Instance.new("TextLabel")
Title.Text = "🧠 Teleport Controller"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0, 10, 0, 5)
Title.Size = UDim2.new(1, -20, 0, 20)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Frame

local StatusIndicator = Instance.new("Frame")
StatusIndicator.Size = UDim2.new(0, 14, 0, 14)
StatusIndicator.Position = UDim2.new(1, -24, 0, 6)
StatusIndicator.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
StatusIndicator.BorderSizePixel = 0
StatusIndicator.Parent = Frame

local StatusCorner = Instance.new("UICorner")
StatusCorner.CornerRadius = UDim.new(1, 0)
StatusCorner.Parent = StatusIndicator

local Button = Instance.new("TextButton")
Button.Size = UDim2.new(1, -80, 0, 30)
Button.Position = UDim2.new(0, 10, 0, 30)
Button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
Button.TextColor3 = Color3.new(1, 1, 1)
Button.Font = Enum.Font.GothamBold
Button.TextSize = 15
Button.Text = "Enable Teleport"
Button.AutoButtonColor = true
Button.Parent = Frame

local BadButton = Instance.new("TextButton")
BadButton.Size = UDim2.new(0, 50, 0, 20)
BadButton.Position = UDim2.new(1, -60, 1, -30)
BadButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
BadButton.TextColor3 = Color3.new(1, 1, 1)
BadButton.Font = Enum.Font.GothamBold
BadButton.TextSize = 12
BadButton.Text = "BAD"
BadButton.AutoButtonColor = true
BadButton.Parent = Frame

local BadCorner = Instance.new("UICorner")
BadCorner.CornerRadius = UDim.new(0, 4)
BadCorner.Parent = BadButton

BadButton.MouseButton1Click:Connect(function()
    toggleBad = not toggleBad
    if toggleBad then
        BadButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        print("[🟢] Now teleporting to low income servers.")
    else
        BadButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
        print("[🔴] Now avoiding low income servers.")
    end
end)

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 6)
BtnCorner.Parent = Button

Button.MouseButton1Click:Connect(function()
    TeleportEnabled = not TeleportEnabled
    if TeleportEnabled then
        brainrotInfo.Visible = true

        Button.Text = "Disable Teleport"
        Button.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        StatusIndicator.BackgroundColor3 = Color3.fromRGB(60, 255, 100)
        print("[🟢] Autojoiner active.")
        task.spawn(teleportLoop)
    else
        brainrotInfo.Visible = false

        Button.Text = "Enable Teleport"
        Button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
        StatusIndicator.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
        print("[🔴] Autojoiner disabled.")
    end
end)

task.spawn(checkFile)
