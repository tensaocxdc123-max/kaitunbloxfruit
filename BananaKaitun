loadstring(game:HttpGet("https://raw.githubusercontent.com/x2RunE/paid_script_cracked/refs/heads/main/banana-cat/kaitunLoader.lua"))()
getgenv().SettingFarm ={
    ["Hide UI"] = false,
    ["Reset Teleport"] = {
        ["Enabled"] = false,
        ["Delay Reset"] = 3,
        ["Item Dont Reset"] = {
            ["Fruit"] = {
                ["Enabled"] = true,
                ["All Fruit"] = true, 
                ["Select Fruit"] = {
                    ["Enabled"] = false,
                    ["Fruit"] = {},
                },
            },
        },
    },
    ["White Screen"] = false,
    ["Lock Fps"] = {
        ["Enabled"] = false,
        ["FPS"] = 20,
    },
    ["Get Items"] = {
        ["Saber"] = true,
        ["Godhuman"] =  true,
        ["Skull Guitar"] = true,
        ["Mirror Fractal"] = true,
        ["Cursed Dual Katana"] = true,
        ["Upgrade Race V2-V3"] = true,
        ["Auto Pull Lever"] = true,
        ["Shark Anchor"] = true,
    },
    ["Get Rare Items"] = {
        ["Rengoku"] = false,
        ["Dragon Trident"] = false, 
        ["Pole (1st Form)"] = false,
        ["Gravity Blade"]  = false,
    },
    ["Farm Fragments"] = {
        ["Enabled"]  = false,
        ["Fragment"] = 50000,
    },
    ["Auto Chat"] = {
        ["Enabled"] = false,
        ["Text"] = "",
    },
    ["Auto Summon Rip Indra"] = true,
    ["Select Hop"] = {
        ["Hop Server If Have Player Near"] = false, 
        ["Hop Find Rip Indra Get Valkyrie Helm or Get Tushita"] = true, 
        ["Hop Find Dough King Get Mirror Fractal"] = false,
        ["Hop Find Raids Castle [CDK]"] = true,
        ["Hop Find Cake Queen [CDK]"] = true,
        ["Hop Find Soul Reaper [CDK]"] = true,
        ["Hop Find Darkbeard [SG]"] = true,
        ["Hop Find Mirage [ Pull Lever ]"] = false,
    },
    ["Farm Mastery"] = {
        ["Melee"] = false,
        ["Sword"] = false,
    },
    ["Buy Haki"] = {
        ["Enhancement"] = true,
        ["Skyjump"] = true,
        ["Flash Step"] = true,
        ["Observation"] = true,
    },
    ["Sniper Fruit Shop"] = {
        ["Enabled"] = true,
        ["Fruit"] = {"Leopard-Leopard","Kitsune-Kitsune","Dragon-Dragon","Yeti-Yeti","Gas-Gas"},
    },
    ["Lock Fruit"] = {},
    ["Webhook"] = {
        ["Enabled"] = true,
        ["WebhookUrl"] = "https://discord.com/api/webhooks/1554035782296014898/P5MgBoBDNg2QV1zFvYNZxqVqpRJJkk4dzhvUxCRbYp5bpRlTgaLfx3xpmxM3V4y2LtKT",
        ["LogLevel"] = "all",         -- info | success | warn | error | all
        ["HeartbeatSeconds"] = 300,   -- log trạng thái mỗi 5 phút
        ["DetailedItems"] = true,     -- log từng item nhận được
        ["TrackBoss"] = true,         -- log boss spawn / kill
        ["TrackTeleport"] = true,     -- log mỗi lần teleport
        ["TrackDeath"] = true,        -- log mỗi lần chết
    }
}

-- ============================================================
-- ADVANCED WEBHOOK LOGGER
-- ============================================================
local HttpService = game:GetService("HttpService")
local Players     = game:GetService("Players")
local RunService  = game:GetService("RunService")
local LP          = Players.LocalPlayer

local COLORS = {
    info    = 3447003,
    success = 3066993,
    warn    = 16776960,
    error   = 15158332,
    farm    = 10181046,
    hop     = 15844367,
    combat  = 15105570,
    item    = 5763719,
    session = 2123412,
}
local ICONS = {
    info    = "ℹ️", success = "✅", warn = "⚠️", error = "❌",
    farm    = "🌾", hop = "🔁", combat = "⚔️", item = "🎁", session = "📊",
}

-- ============================================================
-- SESSION STATS
-- ============================================================
local Session = {
    startTime    = tick(),
    startClock   = os.time(),
    hops         = 0,
    itemsGotten  = {},
    bossesKilled = {},
    deaths       = 0,
    teleports    = 0,
    chatsSent    = 0,
    fruitBought  = {},
    fragmentsAt  = 0,
    masteryExp   = {},
    level        = LP.Data and LP.Data.Level and LP.Data.Level.Value or 0,
}

local function uptimeStr()
    local s = math.floor(tick() - Session.startTime)
    local h = math.floor(s / 3600)
    local m = math.floor((s % 3600) / 60)
    local sec = s % 60
    if h > 0 then return string.format("%dh %dm %ds", h, m, sec) end
    if m > 0 then return string.format("%dm %ds", m, sec) end
    return sec .. "s"
end

local function ts() return os.date("!%Y-%m-%d %H:%M:%S") .. " UTC" end

-- ============================================================
-- CORE SENDER
-- ============================================================
local function sendWebhook(level, title, desc, fields)
    local wh = getgenv().SettingFarm["Webhook"]
    if not wh or not wh.Enabled or wh.WebhookUrl == "" then return end

    local lvl = wh.LogLevel or "all"
    if lvl ~= "all" and lvl ~= level then
        if not (lvl == "info" and level == "success") then
            if lvl ~= "info" then return end
        end
    end

    local embed = {
        title       = (ICONS[level] or "") .. " " .. title,
        description = desc or "",
        color       = COLORS[level] or COLORS.info,
        timestamp   = os.date("!%Y-%m-%dT%H:%M:%SZ"),
        footer      = { text = string.format("Kaitun | %s | %s", LP.Name, game.PlaceId) },
        fields      = fields or {},
    }

    local payload = HttpService:JSONEncode({
        username = "Kaitun Logger",
        embeds   = {embed},
    })

    pcall(function()
        local req = (syn and syn.request) or (http and http.request) or http_request or request
        if req then
            req({
                Url = wh.WebhookUrl,
                Method = "POST",
                Headers = {["Content-Type"] = "application/json"},
                Body = payload,
            })
        else
            HttpService:PostAsync(wh.WebhookUrl, payload, Enum.HttpContentType.ApplicationJson)
        end
    end)
end

local function logInfo(t,d,f)    sendWebhook("info",    t, d, f) end
local function logOk(t,d,f)      sendWebhook("success", t, d, f) end
local function logWarn(t,d,f)    sendWebhook("warn",    t, d, f) end
local function logErr(t,d,f)     sendWebhook("error",   t, d, f) end
local function logFarm(t,d,f)    sendWebhook("farm",    t, d, f) end
local function logHop(t,d,f)     sendWebhook("hop",     t, d, f) end
local function logCombat(t,d,f)  sendWebhook("combat",  t, d, f) end
local function logItem(t,d,f)    sendWebhook("item",    t, d, f) end
local function logSession(t,d,f) sendWebhook("session", t, d, f) end

getgenv().KaitunLog = {
    info=logInfo, ok=logOk, warn=logWarn, err=logErr,
    farm=logFarm, hop=logHop, combat=logCombat, item=logItem,
    session=logSession, send=sendWebhook, stats=Session,
}

-- ============================================================
-- STARTUP
-- ============================================================
logInfo("Script khởi động", "Kaitun loader đã chạy.", {
    {name="Người chơi",  value=LP.Name,                        inline=true},
    {name="DisplayName", value=LP.DisplayName,                 inline=true},
    {name="UserId",      value=tostring(LP.UserId),            inline=true},
    {name="PlaceId",     value=tostring(game.PlaceId),         inline=true},
    {name="JobId",       value=game.JobId,                     inline=false},
    {name="Level",       value=tostring(Session.level),        inline=true},
    {name="Thời gian",   value=ts(),                           inline=false},
})

logOk("SettingFarm đã load", "Cấu hình sẵn sàng.", {
    {name="Get Items (bật)",
        value=(function()
            local t={}
            for k,v in pairs(getgenv().SettingFarm["Get Items"]) do if v then table.insert(t,k) end end
            return #t>0 and table.concat(t,", ") or "none"
        end)(), inline=false},
    {name="Sniper Fruit",
        value=table.concat(getgenv().SettingFarm["Sniper Fruit Shop"]["Fruit"], ", "), inline=false},
    {name="Hop Targets (bật)",
        value=(function()
            local t={}
            for k,v in pairs(getgenv().SettingFarm["Select Hop"]) do if v then table.insert(t,k) end end
            return #t>0 and table.concat(t,", ") or "none"
        end)(), inline=false},
})

-- ============================================================
-- PLAYER TRACKING
-- ============================================================
Players.PlayerAdded:Connect(function(p)
    logInfo("Player join", p.Name, {
        {name="DisplayName", value=p.DisplayName,        inline=true},
        {name="UserId",      value=tostring(p.UserId),   inline=true},
        {name="Tổng players",value=tostring(#Players:GetPlayers()), inline=true},
    })
end)

Players.PlayerRemoving:Connect(function(p)
    logInfo("Player leave", p.Name, {
        {name="UserId",      value=tostring(p.UserId), inline=true},
        {name="Còn lại",     value=tostring(#Players:GetPlayers()-1), inline=true},
    })
end)

-- ============================================================
-- CHARACTER / DEATH / RESPAWN
-- ============================================================
local function hookCharacter(char)
    if not char then return end

    char.ChildAdded:Connect(function(c)
        if c:IsA("Tool") and getgenv().SettingFarm["Webhook"]["DetailedItems"] then
            logItem("Tool nhận được", "**" .. c.Name .. "**", {
                {name="Người chơi", value=LP.Name, inline=true},
                {name="Thời gian",  value=ts(),    inline=true},
            })
            Session.itemsGotten[c.Name] = (Session.itemsGotten[c.Name] or 0) + 1
        end
    end)

    local hum = char:WaitForChild("Humanoid", 5)
    if hum then
        hum.Died:Connect(function()
            if not getgenv().SettingFarm["Webhook"]["TrackDeath"] then return end
            Session.deaths += 1
            logCombat("Nhân vật chết", string.format("Tổng số lần chết: **%d**", Session.deaths), {
                {name="Vị trí", value=(function()
                    local hrp = char:FindFirstChild("HumanoidRootPart")
                    return hrp and string.format("%.0f, %.0f, %.0f", hrp.Position.X, hrp.Position.Y, hrp.Position.Z) or "unknown"
                end)(), inline=true},
                {name="Uptime", value=uptimeStr(), inline=true},
            })
        end)
    end
end

if LP.Character then hookCharacter(LP.Character) end
LP.CharacterAdded:Connect(function(c)
    hookCharacter(c)
    logInfo("Respawn", "Nhân vật hồi sinh.", {
        {name="Deaths", value=tostring(Session.deaths), inline=true},
        {name="Uptime", value=uptimeStr(),              inline=true},
    })
end)

-- ============================================================
-- TELEPORT TRACKING (hook CFrame)
-- ============================================================
task.spawn(function()
    while task.wait(0.5) do
        if not getgenv().SettingFarm["Webhook"]["TrackTeleport"] then continue end
        local char = LP.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        if not hrp then continue end
        local last = Session._lastPos
        if last then
            local d = (hrp.Position - last).Magnitude
            if d > 500 then
                Session.teleports += 1
                logInfo("Teleport", string.format("Di chuyển **%.0f** studs", d), {
                    {name="Từ", value=string.format("%.0f, %.0f, %.0f", last.X, last.Y, last.Z), inline=true},
                    {name="Đến", value=string.format("%.0f, %.0f, %.0f", hrp.Position.X, hrp.Position.Y, hrp.Position.Z), inline=true},
                    {name="Tổng teleport", value=tostring(Session.teleports), inline=true},
                })
            end
        end
        Session._lastPos = hrp.Position
    end
end)

-- ============================================================
-- CHAT TRACKING
-- ============================================================
LP.Chatted:Connect(function(msg)
    if not getgenv().SettingFarm["Auto Chat"]["Enabled"] then return end
    Session.chatsSent += 1
    logInfo("Auto Chat", "```" .. msg .. "```", {
        {name="Tổng chat", value=tostring(Session.chatsSent), inline=true},
    })
end)

-- ============================================================
-- FRUIT SNIPER TRACKING
-- ============================================================
local wantedFruits = {}
for _, f in ipairs(getgenv().SettingFarm["Sniper Fruit Shop"]["Fruit"]) do
    wantedFruits[f:lower()] = true
end

local function scanTools(container, tag)
    if not container then return end
    for _, tool in ipairs(container:GetChildren()) do
        if not tool:IsA("Tool") then continue end
        local name = tool.Name:lower()
        if wantedFruits[name] then
            Session.fruitBought[tool.Name] = (Session.fruitBought[tool.Name] or 0) + 1
            logFarm("Fruit sniper mua", "**" .. tool.Name .. "** → " .. tag, {
                {name="Số lượng", value=tostring(Session.fruitBought[tool.Name]), inline=true},
                {name="Thời gian",value=ts(), inline=true},
            })
            wantedFruits[name] = nil
        end
    end
end

task.spawn(function()
    while task.wait(10) do
        if not getgenv().SettingFarm["Sniper Fruit Shop"]["Enabled"] then continue end
        scanTools(LP:FindFirstChild("Backpack"), "Backpack")
        scanTools(LP:FindFirstChild("Character"), "Equipped")
    end
end)

-- ============================================================
-- BACKPACK CHANGE (mọi tool mới)
-- ============================================================
if LP:FindFirstChild("Backpack") then
    LP.Backpack.ChildAdded:Connect(function(tool)
        if not tool:IsA("Tool") then return end
        if not getgenv().SettingFarm["Webhook"]["DetailedItems"] then return end
        Session.itemsGotten[tool.Name] = (Session.itemsGotten[tool.Name] or 0) + 1
        logItem("Backpack +1", "**" .. tool.Name .. "**", {
            {name="Tổng loại này", value=tostring(Session.itemsGotten[tool.Name]), inline=true},
            {name="Tổng items",    value=tostring((function() local c=0 for _,v in pairs(Session.itemsGotten) do c+=v end return c end)()), inline=true},
        })
    end)
end

-- ============================================================
-- BOSS SPAWN / KILL TRACKING
-- ============================================================
if getgenv().SettingFarm["Webhook"]["TrackBoss"] then
    local BOSS_NAMES = {
        ["Rip Indra"]=true, ["Dough King"]=true, ["Cake Queen"]=true,
        ["Soul Reaper"]=true, ["Darkbeard"]=true, ["Mirage"]=true,
        ["Kitsune"]=true, ["Leopard"]=true,
    }
    local function scanBosses()
        for _, obj in ipairs(workspace:GetDescendants()) do
            if not obj:IsA("Model") then continue end
            local ok, hum = pcall(function() return obj:FindFirstChild("Humanoid") end)
            if not ok or not hum then continue end
            if not BOSS_NAMES[obj.Name] then continue end
            if Session.bossesKilled[obj.Name] then continue end
            -- heuristic: hum > 0 và object có tag boss thường
            if hum.Health > 0 and hum.MaxHealth > 5000 then
                logCombat("Boss spawn", "**" .. obj.Name .. "** xuất hiện", {
                    {name="HP",     value=string.format("%d/%d", hum.Health, hum.MaxHealth), inline=true},
                    {name="Vị trí", value=obj:FindFirstChild("HumanoidRootPart") and string.format("%.0f, %.0f, %.0f", obj.HumanoidRootPart.Position.X, obj.HumanoidRootPart.Position.Y, obj.HumanoidRootPart.Position.Z) or "n/a", inline=true},
                })
                Session.bossesKilled[obj.Name] = "spawned"
            end
        end
    end
    task.spawn(function()
        while task.wait(5) do
            pcall(scanBosses)
        end
    end)
end

-- ============================================================
-- FRAGMENT PROGRESS
-- ============================================================
task.spawn(function()
    local lastMilestone = 0
    while task.wait(20) do
        if not getgenv().SettingFarm["Farm Fragments"]["Enabled"] then continue end
        local target = getgenv().SettingFarm["Farm Fragments"]["Fragment"]
        -- thử nhiều cách đọc fragment
        local frag = nil
        pcall(function()
            local stats = LP:FindFirstChild("leaderstats")
            if stats and stats:FindFirstChild("Fragments") then
                frag = stats.Fragments.Value
            end
        end)
        if frag and frag - lastMilestone >= 5000 then
            lastMilestone = frag
            logFarm("Fragment mốc", string.format("**%d / %d**", frag, target), {
                {name="Tiến độ", value=string.format("%.1f%%", (frag/target)*100), inline=true},
                {name="Uptime",  value=uptimeStr(), inline=true},
            })
        end
    end
end)

-- ============================================================
-- MASTERY TRACKING
-- ============================================================
task.spawn(function()
    while task.wait(60) do
        local sfm = getgenv().SettingFarm["Farm Mastery"]
        if not (sfm["Melee"] or sfm["Sword"]) then continue end
        -- đọc mastery hiện tại nếu có
        pcall(function()
            local data = LP:FindFirstChild("Data")
            local inv  = data and data:FindFirstChild("Inventory")
            local melee = inv and inv:FindFirstChild("Melee")
            if melee then
                for _, tool in ipairs(melee:GetChildren()) do
                    local exp = tool:FindFirstChild("Exp") and tool.Exp.Value or nil
                    if exp and Session.masteryExp[tool.Name] and exp > Session.masteryExp[tool.Name] then
                        local gain = exp - Session.masteryExp[tool.Name]
                        if gain > 100 then
                            logFarm("Mastery +", string.format("**%s** +%d exp", tool.Name, gain), {
                                {name="Tổng", value=tostring(exp), inline=true},
                            })
                        end
                    end
                    Session.masteryExp[tool.Name] = exp
                end
            end
        end)
    end
end)

-- ============================================================
-- GLOBAL warn() HOOK
-- ============================================================
local oldWarn = getgenv().warn
getgenv().warn = function(...)
    local args = {...}
    local msg = table.concat((function()
        local t={} for _,v in ipairs(args) do table.insert(t,tostring(v)) end return t
    end)(), " ")
    logWarn("warn()", "```" .. msg .. "```")
    if oldWarn then oldWarn(...) end
end

-- ============================================================
-- HEARTBEAT
-- ============================================================
task.spawn(function()
    while true do
        task.wait(getgenv().SettingFarm["Webhook"]["HeartbeatSeconds"] or 300)
        local fps = 0
        pcall(function()
            local t = tick()
            local frames = 0
            local conn
            conn = RunService.RenderStepped:Connect(function()
                frames += 1
                if tick() - t >= 1 then conn:Disconnect() end
            end)
            task.wait(1.1)
            fps = math.floor(frames / (tick() - t))
        end)
        logSession("Heartbeat", "Trạng thái session", {
            {name="Uptime",     value=uptimeStr(),                        inline=true},
            {name="Players",    value=tostring(#Players:GetPlayers()),    inline=true},
            {name="FPS",        value=tostring(fps),                      inline=true},
            {name="Deaths",     value=tostring(Session.deaths),           inline=true},
            {name="Teleports",  value=tostring(Session.teleports),        inline=true},
            {name="Hops",       value=tostring(Session.hops),             inline=true},
            {name="Items",      value=tostring((function() local c=0 for _,v in pairs(Session.itemsGotten) do c+=v end return c end)()), inline=true},
            {name="Chats",      value=tostring(Session.chatsSent),        inline=true},
            {name="JobId",      value=game.JobId,                         inline=false},
        })
    end
end)

-- ============================================================
-- INJECT HELPERS (dùng từ script gốc)
-- ============================================================
getgenv().KaitunLog.hopCalled = function(reason)
    Session.hops += 1
    logHop("Hop server", "Lý do: **" .. (reason or "n/a") .. "**", {
        {name="Tổng hops", value=tostring(Session.hops), inline=true},
        {name="Uptime",    value=uptimeStr(),            inline=true},
    })
end

getgenv().KaitunLog.bossKilled = function(name)
    Session.bossesKilled[name] = (Session.bossesKilled[name] or 0)
    if type(Session.bossesKilled[name]) == "number" then
        Session.bossesKilled[name] += 1
    else
        Session.bossesKilled[name] = 1
    end
    logCombat("Boss killed", "**" .. name .. "**", {
        {name="Số lần", value=tostring(Session.bossesKilled[name]), inline=true},
    })
end

logOk("Logger toàn diện đã kích hoạt", "Mọi hoạt động sẽ được báo lên Discord.", {
    {name="Heartbeat", value=tostring(getgenv().SettingFarm["Webhook"]["HeartbeatSeconds"]) .. "s", inline=true},
    {name="LogLevel",  value=getgenv().SettingFarm["Webhook"]["LogLevel"],                          inline=true},
    {name="Uptime",    value=uptimeStr(),                                                           inline=true},
})
