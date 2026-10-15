-- DS HUB | Davi Scripts
-- Interface para testes locais/autorizados no seu próprio jogo.

local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
local pg = player:WaitForChild("PlayerGui")

local old = pg:FindFirstChild("DSHub")
if old then old:Destroy() end

local B = Color3.fromRGB(24, 117, 255)
local BG = Color3.fromRGB(5, 12, 28)
local PANEL = Color3.fromRGB(9, 23, 48)
local PANEL2 = Color3.fromRGB(13, 34, 68)
local TXT = Color3.fromRGB(237, 245, 255)
local MUTED = Color3.fromRGB(151, 179, 220)
local GRID = Color3.fromRGB(28, 83, 157)

local original = {
    FogStart = Lighting.FogStart,
    FogEnd = Lighting.FogEnd,
    FogColor = Lighting.FogColor,
    Ambient = Lighting.Ambient,
    OutdoorAmbient = Lighting.OutdoorAmbient,
    Brightness = Lighting.Brightness,
}
local originalSkies = {}
for _, item in ipairs(Lighting:GetChildren()) do
    if item:IsA("Sky") then table.insert(originalSkies, item:Clone()) end
end

local function make(className, props, parent)
    local object = Instance.new(className)
    for key, value in pairs(props or {}) do object[key] = value end
    object.Parent = parent
    return object
end

local function round(object, radius)
    make("UICorner", {CornerRadius = UDim.new(0, radius or 12)}, object)
end

local function outline(object, color, transparency)
    make("UIStroke", {
        Color = color or GRID,
        Transparency = transparency or 0.25,
        Thickness = 1,
    }, object)
end

local main

local gui = make("ScreenGui", {
    Name = "DSHub",
    ResetOnSpawn = false,
    ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
}, pg)

local function notify(message)
    local toast = make("TextLabel", {
        AnchorPoint = Vector2.new(0.5, 1),
        Position = UDim2.new(0.5, 0, 1, -12),
        Size = UDim2.new(1, -24, 0, 36),
        BackgroundColor3 = PANEL2,
        Text = message,
        TextWrapped = true,
        TextColor3 = TXT,
        Font = Enum.Font.GothamMedium,
        TextSize = 12,
        BorderSizePixel = 0,
        ZIndex = 100,
    }, main)
    round(toast, 14)
    outline(toast, B, 0.1)
    task.delay(2.4, function()
        if toast and toast.Parent then
            TweenService:Create(toast, TweenInfo.new(0.18), {
                BackgroundTransparency = 1,
                TextTransparency = 1,
            }):Play()
            task.delay(0.2, function()
                if toast and toast.Parent then toast:Destroy() end
            end)
        end
    end)
end

local bubble = make("ImageButton", {
    Name = "DSHubFloatingButton",
    AnchorPoint = Vector2.new(1, 0.5),
    Position = UDim2.new(1, -18, 0.55, 0),
    Size = UDim2.new(0, 58, 0, 58),
    BackgroundColor3 = B,
    Image = "rbxassetid://125168523230863",
    ScaleType = Enum.ScaleType.Fit,
    BorderSizePixel = 0,
    AutoButtonColor = false,
    ZIndex = 50,
}, gui)
round(bubble, 100)
outline(bubble, Color3.fromRGB(135, 197, 255), 0.08)
make("UIGradient", {
    Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(44, 148, 255)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(8, 57, 165)),
    }),
    Rotation = 45,
}, bubble)

main = make("Frame", {
    Name = "Main",
    AnchorPoint = Vector2.new(0.5, 0.5),
    Position = UDim2.fromScale(0.5, 0.5),
    Size = UDim2.fromOffset(1, 1),
    BackgroundColor3 = BG,
    BackgroundTransparency = 0.04,
    BorderSizePixel = 0,
    ClipsDescendants = true,
    Visible = false,
    ZIndex = 10,
}, gui)
round(main, 20)
outline(main, B, 0.12)
make("UISizeConstraint", {
    MinSize = Vector2.new(620, 400),
    MaxSize = Vector2.new(1400, 900),
}, main)

local gridLayer = make("Frame", {
    Name = "GridLayer",
    Size = UDim2.fromScale(1, 1),
    BackgroundTransparency = 1,
    BorderSizePixel = 0,
    ZIndex = 10,
}, main)
for xIndex = 0, 14 do
    make("Frame", {
        Position = UDim2.new(xIndex / 14, 0, 0, 0),
        Size = UDim2.new(0, 1, 1, 0),
        BackgroundColor3 = GRID,
        BackgroundTransparency = 0.84,
        BorderSizePixel = 0,
        ZIndex = 10,
    }, gridLayer)
end
for yIndex = 0, 9 do
    make("Frame", {
        Position = UDim2.new(0, 0, yIndex / 9, 0),
        Size = UDim2.new(1, 0, 0, 1),
        BackgroundColor3 = GRID,
        BackgroundTransparency = 0.84,
        BorderSizePixel = 0,
        ZIndex = 10,
    }, gridLayer)
end

local header = make("Frame", {
    Size = UDim2.new(1, 0, 0, 62),
    BackgroundColor3 = PANEL,
    BackgroundTransparency = 0.05,
    BorderSizePixel = 0,
    ZIndex = 12,
}, main)
round(header, 20)
make("Frame", {
    Position = UDim2.new(0, 0, 1, -15),
    Size = UDim2.new(1, 0, 0, 15),
    BackgroundColor3 = PANEL,
    BorderSizePixel = 0,
    ZIndex = 12,
}, header)

local headerLogo = make("ImageLabel", {
    Name = "DSLogo",
    Position = UDim2.new(0, 14, 0, 9),
    Size = UDim2.fromOffset(42, 42),
    BackgroundTransparency = 1,
    Image = "rbxassetid://125168523230863",
    ScaleType = Enum.ScaleType.Fit,
    ZIndex = 14,
}, header)

make("TextLabel", {
    Position = UDim2.new(0, 64, 0, 6),
    Size = UDim2.new(0.55, 0, 0, 30),
    BackgroundTransparency = 1,
    Text = "DS HUB",
    Font = Enum.Font.GothamBlack,
    TextSize = 24,
    TextColor3 = TXT,
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 13,
}, header)
make("TextLabel", {
    Position = UDim2.new(0, 66, 0, 35),
    Size = UDim2.new(0.65, 0, 0, 15),
    BackgroundTransparency = 1,
    Text = "DAVI SCRIPTS  •  DARK BLUE",
    Font = Enum.Font.GothamMedium,
    TextSize = 9,
    TextColor3 = Color3.fromRGB(89, 181, 255),
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 13,
}, header)

local close = make("TextButton", {
    AnchorPoint = Vector2.new(1, 0.5),
    Position = UDim2.new(1, -12, 0.5, 0),
    Size = UDim2.fromOffset(32, 32),
    Text = "×",
    Font = Enum.Font.GothamBold,
    TextSize = 23,
    TextColor3 = TXT,
    BackgroundColor3 = PANEL2,
    BorderSizePixel = 0,
    ZIndex = 14,
}, header)
round(close, 100)

local side = make("Frame", {
    Position = UDim2.new(0, 10, 0, 72),
    Size = UDim2.new(0, 126, 1, -84),
    BackgroundColor3 = PANEL,
    BackgroundTransparency = 0.05,
    BorderSizePixel = 0,
    ZIndex = 12,
}, main)
round(side, 15)
outline(side, GRID, 0.5)

local body = make("ScrollingFrame", {
    Position = UDim2.new(0, 146, 0, 72),
    Size = UDim2.new(1, -156, 1, -84),
    BackgroundTransparency = 1,
    BorderSizePixel = 0,
    ScrollBarThickness = 3,
    ScrollBarImageColor3 = B,
    AutomaticCanvasSize = Enum.AutomaticSize.Y,
    CanvasSize = UDim2.new(),
    ZIndex = 12,
}, main)
make("UIPadding", {
    PaddingTop = UDim.new(0, 2),
    PaddingRight = UDim.new(0, 5),
    PaddingBottom = UDim.new(0, 8),
}, body)

local pages, navButtons = {}, {}
local function page(name)
    local p = make("Frame", {
        Name = "Page" .. name,
        Size = UDim2.new(1, 0, 0, 0),
        AutomaticSize = Enum.AutomaticSize.Y,
        BackgroundTransparency = 1,
        Visible = false,
        ZIndex = 12,
    }, body)
    make("UIListLayout", {
        Padding = UDim.new(0, 9),
        SortOrder = Enum.SortOrder.LayoutOrder,
    }, p)
    pages[name] = p
    return p
end

local function show(name)
    for pageName, pageObject in pairs(pages) do
        pageObject.Visible = pageName == name
    end
    for buttonName, buttonObject in pairs(navButtons) do
        buttonObject.BackgroundColor3 = buttonName == name and B or PANEL2
    end
end

local function card(parent, title, subtitle)
    local frame = make("Frame", {
        Size = UDim2.new(1, 0, 0, 0),
        AutomaticSize = Enum.AutomaticSize.Y,
        BackgroundColor3 = PANEL,
        BackgroundTransparency = 0.04,
        BorderSizePixel = 0,
        ZIndex = 13,
    }, parent)
    round(frame, 15)
    outline(frame, GRID, 0.5)
    make("UIPadding", {
        PaddingTop = UDim.new(0, 12),
        PaddingBottom = UDim.new(0, 12),
        PaddingLeft = UDim.new(0, 12),
        PaddingRight = UDim.new(0, 12),
    }, frame)
    make("UIListLayout", {
        Padding = UDim.new(0, 8),
        SortOrder = Enum.SortOrder.LayoutOrder,
    }, frame)
    make("TextLabel", {
        Size = UDim2.new(1, 0, 0, 23),
        BackgroundTransparency = 1,
        Text = title,
        Font = Enum.Font.GothamBold,
        TextSize = 14,
        TextColor3 = TXT,
        TextXAlignment = Enum.TextXAlignment.Left,
        ZIndex = 14,
    }, frame)
    if subtitle then
        make("TextLabel", {
            Size = UDim2.new(1, 0, 0, 18),
            BackgroundTransparency = 1,
            Text = subtitle,
            Font = Enum.Font.Gotham,
            TextSize = 10,
            TextColor3 = MUTED,
            TextXAlignment = Enum.TextXAlignment.Left,
            ZIndex = 14,
        }, frame)
    end
    return frame
end

local function label(parent, text)
    return make("TextLabel", {
        Size = UDim2.new(1, 0, 0, 20),
        BackgroundTransparency = 1,
        Text = text,
        Font = Enum.Font.Gotham,
        TextSize = 11,
        TextColor3 = MUTED,
        TextXAlignment = Enum.TextXAlignment.Left,
        TextWrapped = true,
        ZIndex = 14,
    }, parent)
end

local function button(parent, text, callback)
    local b = make("TextButton", {
        Size = UDim2.new(1, 0, 0, 36),
        BackgroundColor3 = PANEL2,
        Text = text,
        TextColor3 = TXT,
        Font = Enum.Font.GothamMedium,
        TextSize = 11,
        BorderSizePixel = 0,
        AutoButtonColor = false,
        ZIndex = 14,
    }, parent)
    round(b, 12)
    outline(b, GRID, 0.55)
    b.MouseEnter:Connect(function()
        TweenService:Create(b, TweenInfo.new(0.12), {
            BackgroundColor3 = Color3.fromRGB(18, 52, 102),
        }):Play()
    end)
    b.MouseLeave:Connect(function()
        TweenService:Create(b, TweenInfo.new(0.12), {
            BackgroundColor3 = PANEL2,
        }):Play()
    end)
    b.MouseButton1Click:Connect(function()
        if callback then
            local ok, err = pcall(callback)
            if not ok then warn("[DS HUB] Erro:", err); notify("Erro: "..tostring(err)) end
        end
    end)
    return b
end

local function textbox(parent, placeholder, value)
    local t = make("TextBox", {
        Size = UDim2.new(1, 0, 0, 36),
        BackgroundColor3 = Color3.fromRGB(5, 16, 36),
        Text = value or "",
        PlaceholderText = placeholder,
        PlaceholderColor3 = MUTED,
        TextColor3 = TXT,
        Font = Enum.Font.Gotham,
        TextSize = 12,
        ClearTextOnFocus = false,
        BorderSizePixel = 0,
        ZIndex = 14,
    }, parent)
    round(t, 12)
    outline(t, GRID, 0.45)
    return t
end

local function toggle(parent, text, initial, callback)
    local row = make("Frame", {
        Size = UDim2.new(1, 0, 0, 32),
        BackgroundTransparency = 1,
        ZIndex = 14,
    }, parent)
    make("TextLabel", {
        Size = UDim2.new(1, -58, 1, 0),
        BackgroundTransparency = 1,
        Text = text,
        TextColor3 = TXT,
        Font = Enum.Font.Gotham,
        TextSize = 11,
        TextXAlignment = Enum.TextXAlignment.Left,
        ZIndex = 14,
    }, row)
    local on = initial == true
    local sw = make("TextButton", {
        AnchorPoint = Vector2.new(1, 0.5),
        Position = UDim2.new(1, 0, 0.5, 0),
        Size = UDim2.new(0, 43, 0, 23),
        Text = "",
        BackgroundColor3 = on and B or Color3.fromRGB(43, 55, 77),
        BorderSizePixel = 0,
        ZIndex = 15,
    }, row)
    round(sw, 100)
    local knob = make("Frame", {
        AnchorPoint = Vector2.new(0, 0.5),
        Position = on and UDim2.new(1, -20, 0.5, 0) or UDim2.new(0, 3, 0.5, 0),
        Size = UDim2.new(0, 16, 0, 16),
        BackgroundColor3 = Color3.new(1, 1, 1),
        BorderSizePixel = 0,
        ZIndex = 16,
    }, sw)
    round(knob, 100)
    sw.MouseButton1Click:Connect(function()
        on = not on
        TweenService:Create(sw, TweenInfo.new(0.14), {
            BackgroundColor3 = on and B or Color3.fromRGB(43, 55, 77),
        }):Play()
        TweenService:Create(knob, TweenInfo.new(0.14), {
            Position = on and UDim2.new(1, -19, 0.5, 0) or UDim2.new(0, 3, 0.5, 0),
        }):Play()
        if callback then callback(on) end
    end)
end

local function restoreLighting()
    for key, value in pairs(original) do Lighting[key] = value end
    for _, item in ipairs(Lighting:GetChildren()) do
        if item:IsA("Sky") then item:Destroy() end
    end
    for _, sky in ipairs(originalSkies) do sky:Clone().Parent = Lighting end
    notify("Iluminação restaurada.")
end

-- ===== PESQUISA GLOBAL (lupa) =====
local searchTargets = {}
local function registerSearchable(frame) table.insert(searchTargets, frame) end
local function filterAll(query)
    query = (query or ""):lower()
    for _, frame in ipairs(searchTargets) do
        if not frame.Parent then continue end
        if query == "" then frame.Visible = true; continue end
        local found = false
        for _, d in ipairs(frame:GetDescendants()) do
            if (d:IsA("TextLabel") or d:IsA("TextButton") or d:IsA("TextBox")) and d.Text and d.Text:lower():find(query, 1, true) then
                found = true; break
            end
        end
        frame.Visible = found
    end
end

-- Navegação lateral (adicionada aba Admin)
local menu = {"Principal", "Skybox", "Neblina", "Personagens", "Players", "Teleport", "Visual", "Admin", "Credits", "Configurações"}
local icons = {"⌂", "☁", "≋", "♟", "➤", "◎", "◉", "🔍", "★", "⚙"}
for index, name in ipairs(menu) do
    local nav = make("TextButton", {
        Position = UDim2.new(0, 7, 0, 8 + (index - 1) * 38),
        Size = UDim2.new(1, -14, 0, 32),
        BackgroundColor3 = name == "Principal" and B or PANEL2,
        Text = icons[index] .. "  " .. name,
        TextColor3 = TXT,
        Font = Enum.Font.GothamMedium,
        TextSize = 10,
        TextXAlignment = Enum.TextXAlignment.Left,
        BorderSizePixel = 0,
        AutoButtonColor = false,
        ZIndex = 14,
    }, side)
    round(nav, 11)
    navButtons[name] = nav
    nav.MouseButton1Click:Connect(function() show(name) end)
end

local p = page("Principal")
local c = card(p, "Bem-vindo ao DS HUB", "Davi Scripts • Interface azul neon")
label(c, "Painel horizontal inspirado na sua referência.")
button(c, "Restaurar iluminação original", restoreLighting)
button(c, "Abrir Skybox", function() show("Skybox") end)
button(c, "Abrir Players", function() show("Players") end)
button(c, "Abrir Admin", function() show("Admin") end)

p = page("Skybox")
c = card(p, "Skybox", "Efeito visual local no seu ambiente de teste")
label(c, "ID numérico de uma imagem compatível com Skybox")
local skyBox = textbox(c, "Digite o ID da imagem")
button(c, "Aplicar Skybox", function()
    local id = skyBox.Text:match("%d+")
    if not id then notify("Digite um ID numérico válido."); return end
    for _, item in ipairs(Lighting:GetChildren()) do
        if item:IsA("Sky") then item:Destroy() end
    end
    local sky = Instance.new("Sky")
    local asset = "rbxassetid://" .. id
    sky.SkyboxBk, sky.SkyboxDn, sky.SkyboxFt = asset, asset, asset
    sky.SkyboxLf, sky.SkyboxRt, sky.SkyboxUp = asset, asset, asset
    sky.Parent = Lighting
    notify("Skybox aplicada localmente.")
end)
button(c, "Restaurar Skybox e iluminação", restoreLighting)

p = page("Neblina")
c = card(p, "Neblina", "Efeitos locais")
toggle(c, "Neblina escura", false, function(on)
    if on then
        Lighting.FogColor = Color3.fromRGB(6, 10, 22)
        Lighting.FogStart, Lighting.FogEnd = 0, 95
        Lighting.Ambient = Color3.fromRGB(22, 26, 43)
        Lighting.OutdoorAmbient = Color3.fromRGB(18, 22, 39)
        notify("Neblina escura ativada.")
    else
        restoreLighting()
    end
end)
button(c, "Neblina azul", function()
    Lighting.FogColor = Color3.fromRGB(13, 44, 100)
    Lighting.FogStart, Lighting.FogEnd = 0, 125
    notify("Neblina azul aplicada.")
end)
button(c, "Restaurar iluminação", restoreLighting)

p = page("Personagens")
c = card(p, "Personagens", "Presets demonstrativos")
label(c, "Esses botões não trocam o avatar automaticamente.")
for _, characterName in ipairs({"R6", "R15", "Gojo", "Luffy", "Zoro"}) do
    button(c, "Selecionar " .. characterName, function()
        notify("Preset selecionado: " .. characterName)
    end)
end

p = page("Players")
c = card(p, "Players", "Movimento e personagem no seu próprio jogo")
local infiniteJumpEnabled = false
local jumpConnection
toggle(c, "Infinite Jump", false, function(on)
    infiniteJumpEnabled = on
    if on then
        if jumpConnection then jumpConnection:Disconnect() end
        jumpConnection = UIS.JumpRequest:Connect(function()
            if not infiniteJumpEnabled then return end
            local character = player.Character
            local humanoid = character and character:FindFirstChildOfClass("Humanoid")
            if humanoid then humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
        end)
        notify("Infinite Jump ativado.")
    else
        if jumpConnection then jumpConnection:Disconnect(); jumpConnection = nil end
        notify("Infinite Jump desativado.")
    end
end)
local invisible = false
local savedTransparency = {}
toggle(c, "Invisibility", false, function(on)
    local character = player.Character
    if not character then notify("Personagem não encontrado."); return end
    invisible = on
    if on then
        savedTransparency = {}
        for _, obj in ipairs(character:GetDescendants()) do
            if obj:IsA("BasePart") or obj:IsA("Decal") then
                savedTransparency[obj] = obj.Transparency
                obj.Transparency = 1
            elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") then
                obj.Enabled = false
            end
        end
        notify("Invisibilidade visual local ativada.")
    else
        for obj, value in pairs(savedTransparency) do
            if obj and obj.Parent then obj.Transparency = value end
        end
        savedTransparency = {}
        for _, obj in ipairs(character:GetDescendants()) do
            if obj:IsA("ParticleEmitter") or obj:IsA("Trail") then obj.Enabled = true end
        end
        notify("Visibilidade restaurada.")
    end
end)
toggle(c, "Fly", false, function(on)
    notify(on and "Fly não está implementado com segurança nesta versão." or "Fly desativado.")
end)
toggle(c, "Noclip", false, function(on)
    notify(on and "Noclip não está implementado nesta versão." or "Noclip desativado.")
end)
label(c, "Velocidade de caminhada")
local speed = textbox(c, "Velocidade (padrão 16)", "16")
button(c, "Aplicar velocidade", function()
    local number = tonumber(speed.Text)
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if not number or not humanoid then notify("Valor inválido ou personagem indisponível."); return end
    number = math.clamp(number, 0, 100)
    humanoid.WalkSpeed = number
    notify("Velocidade aplicada: " .. number)
end)
button(c, "Restaurar velocidade padrão", function()
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then humanoid.WalkSpeed = 16 end
    speed.Text = "16"
    notify("Velocidade restaurada.")
end)

p = page("Teleport")
c = card(p, "Teleport", "Coordenadas do seu próprio ambiente de teste")
local x = textbox(c, "Coordenada X", "0")
local y = textbox(c, "Coordenada Y", "10")
local z = textbox(c, "Coordenada Z", "0")
button(c, "Teleportar para coordenadas", function()
    local a, b, d = tonumber(x.Text), tonumber(y.Text), tonumber(z.Text)
    local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not (a and b and d and root) then notify("Confira as coordenadas."); return end
    root.CFrame = CFrame.new(a, b, d)
    notify("Teleport realizado.")
end)

p = page("Visual")
c = card(p, "Visual", "Iluminação local")
toggle(c, "Iluminação azul", false, function(on)
    if on then
        Lighting.Ambient = Color3.fromRGB(34, 63, 116)
        Lighting.OutdoorAmbient = Color3.fromRGB(20, 39, 82)
    else
        Lighting.Ambient = original.Ambient
        Lighting.OutdoorAmbient = original.OutdoorAmbient
    end
    notify(on and "Iluminação azul ativada." or "Iluminação restaurada.")
end)
toggle(c, "Brilho alto", false, function(on)
    Lighting.Brightness = on and 3 or original.Brightness
    notify(on and "Brilho aumentado." or "Brilho restaurado.")
end)

-- ===== ABA ADMIN =====
p = page("Admin")

local cSearch = card(p, "🔍 Pesquisa Global", "Filtra botões em TODAS as abas em tempo real")
local searchBox = textbox(cSearch, "Digite para pesquisar (ex: fly, speed, skybox)...")
searchBox:GetPropertyChangedSignal("Text"):Connect(function()
    filterAll(searchBox.Text)
end)
button(cSearch, "Limpar pesquisa", function()
    searchBox.Text = ""
    filterAll("")
end)

local cIY = card(p, "Infinite Yield", "Carrega e executa comandos • Ambiente autorizado")
local IYLoaded = false

button(cIY, "Carregar Infinite Yield", function()
    if IYLoaded then notify("Infinite Yield já foi carregado."); return end
    local ok, err = pcall(function()
        loadstring(game:HttpGet("https://scriptblox.com/raw/Universal-Script-inf-yeld-61578"))()
    end)
    if ok then
        IYLoaded = true
        notify("Infinite Yield carregado com sucesso.")
    else
        warn("[DS HUB] Falha ao carregar IY:", err)
        notify("Falha ao carregar IY: " .. tostring(err))
    end
end)

label(cIY, "Atalhos (executam se o IY estiver carregado):")

local function runIY(cmd)
    if not IYLoaded then notify("Carregue o Infinite Yield primeiro."); return end
    local exec = rawget(_G, "IY_ExecuteCommand") or (getgenv and getgenv().IY_ExecuteCommand)
    if type(exec) == "function" then
        local ok, err = pcall(exec, cmd)
        if ok then notify("Comando IY: " .. cmd)
        else notify("Erro IY: " .. tostring(err)) end
    else
        notify("Executor IY não exposto. Digite no chat do IY: " .. cmd)
    end
end

local iyCmds = {
    {"Fly", "fly"}, {"Noclip", "noclip"}, {"Unfly", "unfly"}, {"Unnoclip", "unnoclip"},
    {"Speed 16", "speed 16"}, {"Speed 50", "speed 50"}, {"God", "god"}, {"Ungod", "ungod"},
    {"Heal", "heal"}, {"Refresh", "refresh"}, {"Rejoin", "rejoin"}, {"Serverhop", "serverhop"},
    {"ESP", "esp"}, {"NoESP", "noesp"}, {"Fullbright", "fullbright"}, {"Anti-AFK", "antiafk"},
    {"Kill All", "killall"}, {"Invisible", "invisible"}, {"Visible", "visible"},
}
for _, pair in ipairs(iyCmds) do
    button(cIY, "IY: " .. pair[1], function() runIY(pair[2]) end)
end
label(cIY, "Use apenas em ambiente próprio/autorizado.")

p = page("Credits")
c = card(p, "Credits", "Créditos do projeto")
label(c, "Script feito por: Davi Scripts")
label(c, "Interface: DS HUB")
label(c, "Logo: textura enviada pelo criador")
button(c, "Infinite Yield", function()
    notify("Por segurança, o carregador remoto de scripts foi omitido. Use ferramentas próprias no seu jogo de teste.")
end)

p = page("Configurações")
c = card(p, "Configurações", "Personalização")
toggle(c, "Notificações (visual)", true, function(on)
    notify(on and "Notificações ativadas." or "Notificações desativadas.")
end)
button(c, "Tema azul neon", function() notify("Tema azul neon já está aplicado.") end)
button(c, "Restaurar iluminação", restoreLighting)
button(c, "Fechar DS HUB", function() gui:Destroy() end)

-- Registrar todas as páginas para a pesquisa global
for _, pageFrame in pairs(pages) do
    registerSearchable(pageFrame)
end

local isOpen = false
local animating = false
local openSize = UDim2.new(0.82, 0, 0.84, 0)
local closedSize = UDim2.fromOffset(12, 12)

local function openPanel()
    if isOpen or animating or not gui.Parent then return end
    animating = true
    isOpen = true
    main.Visible = true
    main.Size = closedSize
    main.BackgroundTransparency = 0.45
    main.Rotation = -3
    local target = TweenService:Create(main, TweenInfo.new(0.42, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = openSize,
        BackgroundTransparency = 0.04,
        Rotation = 0,
    })
    target:Play()
    target.Completed:Wait()
    animating = false
end

local function closePanel()
    if not isOpen or animating or not gui.Parent then return end
    animating = true
    isOpen = false
    local target = TweenService:Create(main, TweenInfo.new(0.30, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
        Size = closedSize,
        BackgroundTransparency = 0.55,
        Rotation = 3,
    })
    target:Play()
    target.Completed:Wait()
    main.Visible = false
    main.Rotation = 0
    animating = false
end

bubble.MouseButton1Click:Connect(function()
    if bubbleMoved then bubbleMoved = false; return end
    if isOpen then closePanel() else openPanel() end
end)
close.MouseButton1Click:Connect(closePanel)

local bubbleDragging = false
local bubbleDragStart, bubbleStartPosition, bubbleMoved
bubble.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        bubbleDragging = true
        bubbleMoved = false
        bubbleDragStart = input.Position
        bubbleStartPosition = bubble.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then bubbleDragging = false end
        end)
    end
end)
UIS.InputChanged:Connect(function(input)
    if bubbleDragging and bubbleDragStart and bubbleStartPosition and
       (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - bubbleDragStart
        if math.abs(delta.X) + math.abs(delta.Y) > 6 then bubbleMoved = true end
        bubble.Position = UDim2.new(bubbleStartPosition.X.Scale, bubbleStartPosition.X.Offset + delta.X,
            bubbleStartPosition.Y.Scale, bubbleStartPosition.Y.Offset + delta.Y)
    end
end)

local panelDragging = false
local panelDragStart, panelStartPosition

header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        panelDragging = true
        panelDragStart = input.Position
        panelStartPosition = main.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                panelDragging = false
            end
        end)
    end
end)

UIS.InputChanged:Connect(function(input)
    if panelDragging and panelDragStart and panelStartPosition and
        (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - panelDrag
