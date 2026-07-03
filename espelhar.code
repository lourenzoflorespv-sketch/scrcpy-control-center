#!/usr/bin/env lua

-- ====================================================================
-- 🎨 CONFIGURAÇÃO GERAL (TEXTOS, IMAGENS E PASTAS)
-- ====================================================================
local CONFIG = {
    -- [!] LOCAL DAS IMAGENS: Altere aqui a pasta onde ficam suas imagens
    PASTA_IMAGENS = os.getenv("HOME") .. "coloque o nome da pasta com as imagens sem u=fundo AQUI",

    LARGURA_JANELA  = 640,
    BORDAS_JANELA   = 2,
    TAMANHO_IMAGEM  = 140,

    -- [!] TEXTOS DA INTERFACE: Altere os nomes e títulos aqui
    TXT_TITULO_APP = "Scrcpy Control Center Pro",
    TXT_CABECALHO  = "<span font='Sans Bold 14' foreground='#ffffff'>Painel de Transmissão Scrcpy</span>",
    TXT_SUBTITULO  = "<span font='Sans 10' foreground='#8a92a6'>Configure os parâmetros de desempenho e conectividade do dispositivo:</span>",
    
    LBL_CONEXAO   = "Método de Conexão:CB",
    LBL_VID_SRC   = "Fonte de Transmissão:CB",
    LBL_PERF_V    = "Perfil de Vídeo:CB",
    LBL_CODEC_V   = "Codec de Vídeo:CB",
    LBL_FPS       = "Taxa de Quadros (FPS):CB",
    LBL_AUDIO_OUT = "Saída de Áudio:CB",
    LBL_CONTROL   = "Modo de Controle:CB",
    
    LBL_AWAKE     = "Manter tela do dispositivo ativa",
    LBL_SCR_OFF   = "Desligar tela física do aparelho (Economia)",
    LBL_ON_TOP    = "Fixar janela sobre as outras",
    LBL_RECORD    = "Gravar sessão (Salva em ~/Vídeos)",
    
    -- Botões com ícones do sistema GTK (compreensíveis)
    BTN_INICIAR = "Iniciar!media-playback-start:0",
    BTN_MANUAL  = "Manual e Tutorial!help-about:2",
    BTN_CANCEL  = "Sair!application-exit:1",

    -- Opções dos Menus
    OPC_USB      = "Cabo USB (Autodetectar)",
    OPC_WIFI     = "Rede Wi-Fi (Salvo/Sem Fio)",
    OPC_TUTORIAL = "Cadastrar Novo Dispositivo (Wi-Fi)",

    OPC_SRC_TELA  = "Tela Principal",
    OPC_SRC_CAM_T = "Câmera Traseira",
    OPC_SRC_CAM_F = "Câmera Frontal",
    
    OPC_V_ULTRA  = "Ultra (32 Mbps - Máxima)",
    OPC_V_HIGH   = "Alta (16 Mbps - 1080p)",
    OPC_V_BAL    = "Equilibrado (8 Mbps - 720p)",

    OPC_C_H264   = "H.264 (Maior Compatibilidade)",
    OPC_C_H265   = "H.265 (Melhor para Wi-Fi)",
    OPC_C_AV1    = "AV1 (Dispositivos Novos)",

    OPC_FPS_60   = "60 FPS (Fluidez)",
    OPC_FPS_30   = "30 FPS (Econômico)",
    
    OPC_A_OPUS   = "Opus (Baixa Latência/PC)",
    OPC_A_AAC    = "AAC (Alta Qualidade/PC)",
    OPC_A_MUTE   = "Mudo (Tocar no Celular)",

    OPC_CTRL_TOTAL = "Teclado e Mouse (Padrão)",
    OPC_CTRL_UHID  = "Modo Gamepad Físico",
    OPC_CTRL_NONE  = "Apenas Visualização (Somente Leitura)",

    -- Textos do Assistente e Manual/Tutorial
    WIZARD_PASSO1 = "<span font='11' weight='bold'>Cadastrar Novo Aparelho na Rede</span>\n\n1. Conecte o cabo USB temporariamente.\n2. Certifique-se que o PC e o celular estão na mesma rede Wi-Fi.\n\nClique em Analisar para mapear o endereço IP automaticamente.",
    WIZARD_OFFLINE= "Nenhum aparelho detectado! Verifique se está conectado no cabo e se a Depuração USB está ativa.",
    WIZARD_SUCESSO= "<b>Aparelho Reconhecido e Salvo!</b>\n\nPode desconectar o cabo. O IP foi salvo para as próximas conexões sem fio.",
    
    MANUAL_TEXTO  = "<b>TUTORIAL: CONFIGURAR O TELEMÓVEL</b>\n\n" ..
                    "Para permitir o compartilhamento de tela, siga estes passos no seu Android:\n\n" ..
                    "1. Abra as <b>Configurações</b> do celular.\n" ..
                    "2. Vá até <b>Sobre o Telefone</b> ou <b>Sistema</b>.\n" ..
                    "3. Procure por <b>Número da Versão</b> (ou Número de Compilação).\n" ..
                    "4. Toque <b>7 vezes seguidas</b> nesse número até aparecer a mensagem 'Você agora é um desenvolvedor'.\n" ..
                    "5. Volte ao menu anterior e entre em <b>Opções do Desenvolvedor</b>.\n" ..
                    "6. Ative a opção <b>Depuração USB</b>.\n" ..
                    "7. Conecte o telemóvel ao PC via cabo USB. Surgirá um aviso na tela do celular pedindo autorização; marque a caixa <i>'Sempre permitir deste computador'</i> e clique em <b>Permitir</b>.\n\n" ..
                    "--------------------------------------------------\n" ..
                    "<b>DICAS DE USO DO PAINEL:</b>\n" ..
                    "<b>- Conexão:</b> Use USB para resposta imediata. Use Wi-Fi para usar sem cabos (faça o Cadastro primeiro).\n" ..
                    "<b>- Gravação:</b> Os arquivos ficam guardados em formato MP4 na pasta <i>Vídeos/Scrcpy_Records</i> na sua Home.\n" ..
                    "<b>- Áudio:</b> O formato 'Opus' oferece a menor latência de áudio no PC.",

    TRAY_TITULO    = "Scrcpy Pro",
    TRAY_ENCERRA   = "Encerrar Sessão",
}
-- ====================================================================

-- Comente a linha de baixo com '--' se preferir ignorar os avisos de dependências em falta
-- check_dependencies()

local SCRIPT_PATH = arg[0]
local CONFIG_FILE = os.getenv("HOME") .. "/.scrcpy_gui.conf"
local ACTION = arg[1]

local function load_config()
    local c = { ip="", modo="usb", vid_src="tela", perf="high", codec_v="h264", fps="60", audio="opus", ctrl="sdk", awake="false", scr_off="false", on_top="false", record="false" }
    local f = io.open(CONFIG_FILE, "r")
    if f then
        for line in f:lines() do
            local k, v = line:match("^(%w+)=(.*)$")
            if k and v then c[k] = v end
        end
        f:close()
    end
    return c
end

local function save_config(c)
    local f = io.open(CONFIG_FILE, "w")
    if f then
        for k, v in pairs(c) do f:write(k .. "=" .. tostring(v) .. "\n") end
        f:close()
    end
end

local function notify(title, msg)
    os.execute(string.format("notify-send '%s' '%s' --icon=smartphone", title, msg))
end

local function encerrar_tudo()
    os.execute("killall scrcpy 2>/dev/null")
    os.execute("pkill -f 'ScrcpyTray' 2>/dev/null") 
end

-- Busca e renderização da imagem lateral aleatória em Base64
local function obter_imagem_aleatoria()
    local comando = "find '" .. CONFIG.PASTA_IMAGENS .. "' -type f \\( -name '*.png' -o -name '*.svg' -o -name '*.jpg' -o -name '*.jpeg' \\) 2>/dev/null"
    local p = io.popen(comando)
    local imagens = {}
    if p then
        for linha in p:lines() do table.insert(imagens, linha) end
        p:close()
    end

    if #imagens > 0 then
        math.randomseed(math.floor(os.time() + os.clock() * 1000))
        local img_original = imagens[math.random(1, #imagens)]
        local ext = img_original:match("%.([^%.]+)$") or "png"
        ext = ext:lower()
        local mime = (ext == "jpg" or ext == "jpeg") and "image/jpeg" or (ext == "svg" and "image/svg+xml" or "image/png")
        
        local handle_b64 = io.popen("base64 -w 0 '" .. img_original .. "' 2>/dev/null")
        local b64_data = handle_b64:read("*a")
        handle_b64:close()
        
        if b64_data and b64_data ~= "" then
            local tmp_svg = "/tmp/scrcpy_icone_reduzido.svg"
            local f = io.open(tmp_svg, "w")
            if f then
                local svg_content = string.format([[<svg width="%d" height="%d" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"> <image xlink:href="data:%s;base64,%s" width="%d" height="%d" preserveAspectRatio="xMidYMid meet" /> </svg>]], CONFIG.TAMANHO_IMAGEM, CONFIG.TAMANHO_IMAGEM, mime, b64_data:gsub("%s+", ""), CONFIG.TAMANHO_IMAGEM, CONFIG.TAMANHO_IMAGEM)
                f:write(svg_content)
                f:close()
                return tmp_svg, img_original 
            end
        end
        return img_original, img_original
    end
    return "video-display", nil
end

local function extrair_cor_dominante(img_path)
    if not img_path then return "#4f5b66" end
    local cmd = string.format("convert '%s' -resize 1x1\\! -format '%%[hex:u]' info:- 2>/dev/null || magick '%s' -resize 1x1\\! -format '%%[hex:u]' info:- 2>/dev/null", img_path, img_path)
    local p = io.popen(cmd)
    local hex = p:read("*a")
    p:close()
    hex = hex and hex:match("(%x%x%x%x%x%x)")
    if hex then return "#" .. hex end
    return "#4f5b66"
end

local function gerar_css_dinamico(cor_hex)
    local css_path = "/tmp/scrcpy_tema_dinamico.css"
    local f = io.open(css_path, "w")
    if f then
        local css = string.format([[
            window { background-color: #14161d; color: #e3e6ed; }
            grid, box { background-color: alpha(%s, 0.06); }
            label { font-family: 'Sans'; font-size: 11px; color: #a4a9b8; }
            entry, combobox, combobox window { background-color: #1c1f27; color: #ffffff; border: 1px solid #2d323f; border-radius: 4px; padding: 5px; }
            entry:focus, combobox:focus { border-color: %s; }
            checkbutton { color: #e3e6ed; }
            button { font-family: 'Sans Bold'; font-size: 12px; border: 1px solid #2d3240; border-radius: 4px; padding: 6px 16px; background-color: #1f232e; color: #ffffff; }
            button:hover { background-color: #262b38; border-color: %s; }
            button:first-child { background-color: alpha(%s, 0.2); border-color: %s; }
            button:first-child:hover { background-color: alpha(%s, 0.3); }
        ]], cor_hex, cor_hex, cor_hex, cor_hex, cor_hex, cor_hex)
        f:write(css)
        f:close()
        return "--css=" .. css_path
    end
    return ""
end

-- ====================================================================
-- AÇÕES DE SEGUNDO PLANO (BACKGROUND / TRAY)
-- ====================================================================
if ACTION == "start_bg" then
    local c = load_config()
    local cmd_args = ""

    if c.vid_src == "cam_t" then cmd_args = cmd_args .. " --video-source=camera --camera-facing=back"
    elseif c.vid_src == "cam_f" then cmd_args = cmd_args .. " --video-source=camera --camera-facing=front" end

    if c.perf == "ultra" then cmd_args = cmd_args .. " --no-downsize --video-bit-rate=32M"
    elseif c.perf == "high" then cmd_args = cmd_args .. " --max-size=1920 --video-bit-rate=16M"
    else cmd_args = cmd_args .. " --max-size=1280 --video-bit-rate=8M" end

    cmd_args = cmd_args .. " --video-codec=" .. c.codec_v .. " --max-fps=" .. c.fps

    if c.awake == "true" then cmd_args = cmd_args .. " --stay-awake" end
    if c.scr_off == "true" then cmd_args = cmd_args .. " --turn-screen-off" end
    if c.on_top == "true" then cmd_args = cmd_args .. " --always-on-top" end
    
    if c.record == "true" then
        local record_dir = os.getenv("HOME") .. "/Vídeos/Scrcpy_Records"
        os.execute("mkdir -p '" .. record_dir .. "'")
        local record_file = string.format("%s/scrcpy_%s.mp4", record_dir, os.date("%Y%m%d_%H%M%S"))
        cmd_args = cmd_args .. string.format(" --record='%s'", record_file)
    end

    if c.ctrl == "uhid" then cmd_args = cmd_args .. " --keyboard=uhid --mouse=uhid"
    elseif c.ctrl == "none" then cmd_args = cmd_args .. " --no-control" end

    if c.audio == "opus" then cmd_args = cmd_args .. " --audio-codec=opus"
    elseif c.audio == "aac" then cmd_args = cmd_args .. " --audio-codec=aac"
    else cmd_args = cmd_args .. " --no-audio" end

    local base_conn = ""
    if c.modo == "wifi" and c.ip ~= "" then 
        os.execute("adb connect " .. c.ip .. ":5555")
        base_conn = "-e "
    else 
        os.execute("adb start-server")
        base_conn = "-d "
    end
    
    notify("Scrcpy", "Conectando ao dispositivo...")
    os.execute("nohup scrcpy " .. base_conn .. cmd_args .. " >/dev/null 2>&1 &")
    
    local menu = string.format("Apagar Tela do Aparelho!adb shell cmd display power-off 0|")
    menu = menu .. string.format("Acender Tela do Aparelho!adb shell cmd display power-on 0|")
    menu = menu .. string.format("-----------------|%s!bash -c '%s kill'", CONFIG.TRAY_ENCERRA, SCRIPT_PATH)

    os.execute(string.format("yad --notification --class='ScrcpyTray' --image='smartphone' --text='%s' --menu=\"%s\"", CONFIG.TRAY_TITULO, menu))
    os.exit(0)

elseif ACTION == "kill" then
    encerrar_tudo()
    notify("Scrcpy", "Conexão encerrada.")
    os.exit(0)
end

-- ====================================================================
-- INTERFACE GRÁFICA INTERATIVA (GUI)
-- ====================================================================
local c = load_config()

local imagem_lateral, img_original = obter_imagem_aleatoria()
local arg_css = ""
if img_original then
    local cor_sutil = extrair_cor_dominante(img_original)
    arg_css = gerar_css_dinamico(cor_sutil)
else
    arg_css = gerar_css_dinamico("#4f5b66")
end

local function abrir_interface()
    local def_conexao = (c.modo == "usb") and string.format("^%s!%s!%s", CONFIG.OPC_USB, CONFIG.OPC_WIFI, CONFIG.OPC_TUTORIAL) or string.format("%s!^%s!%s", CONFIG.OPC_USB, CONFIG.OPC_WIFI, CONFIG.OPC_TUTORIAL)
    local def_src = (c.vid_src == "tela") and string.format("^%s!%s!%s", CONFIG.OPC_SRC_TELA, CONFIG.OPC_SRC_CAM_T, CONFIG.OPC_SRC_CAM_F) or ((c.vid_src == "cam_t") and string.format("%s!^%s!%s", CONFIG.OPC_SRC_TELA, CONFIG.OPC_SRC_CAM_T, CONFIG.OPC_SRC_CAM_F) or string.format("%s!%s!^%s", CONFIG.OPC_SRC_TELA, CONFIG.OPC_SRC_CAM_T, CONFIG.OPC_SRC_CAM_F))
    local def_perf = (c.perf == "ultra") and string.format("^%s!%s!%s", CONFIG.OPC_V_ULTRA, CONFIG.OPC_V_HIGH, CONFIG.OPC_V_BAL) or ((c.perf == "high") and string.format("%s!^%s!%s", CONFIG.OPC_V_ULTRA, CONFIG.OPC_V_HIGH, CONFIG.OPC_V_BAL) or string.format("%s!%s!^%s", CONFIG.OPC_V_ULTRA, CONFIG.OPC_V_HIGH, CONFIG.OPC_V_BAL))
    local def_codec = (c.codec_v == "h264") and string.format("^%s!%s!%s", CONFIG.OPC_C_H264, CONFIG.OPC_C_H265, CONFIG.OPC_C_AV1) or ((c.codec_v == "h265") and string.format("%s!^%s!%s", CONFIG.OPC_C_H264, CONFIG.OPC_C_H265, CONFIG.OPC_C_AV1) or string.format("%s!%s!^%s", CONFIG.OPC_C_H264, CONFIG.OPC_C_H265, CONFIG.OPC_C_AV1))
    local def_fps = (c.fps == "60") and string.format("^%s!%s", CONFIG.OPC_FPS_60, CONFIG.OPC_FPS_30) or string.format("%s!^%s", CONFIG.OPC_FPS_60, CONFIG.OPC_FPS_30)
    local def_audio = (c.audio == "opus") and string.format("^%s!%s!%s", CONFIG.OPC_A_OPUS, CONFIG.OPC_A_AAC, CONFIG.OPC_A_MUTE) or ((c.audio == "aac") and string.format("%s!^%s!%s", CONFIG.OPC_A_OPUS, CONFIG.OPC_A_AAC, CONFIG.OPC_A_MUTE) or string.format("%s!%s!^%s", CONFIG.OPC_A_OPUS, CONFIG.OPC_A_AAC, CONFIG.OPC_A_MUTE))
    local def_control = (c.ctrl == "sdk") and string.format("^%s!%s!%s", CONFIG.OPC_CTRL_TOTAL, CONFIG.OPC_CTRL_UHID, CONFIG.OPC_CTRL_NONE) or ((c.ctrl == "uhid") and string.format("%s!^%s!%s", CONFIG.OPC_CTRL_TOTAL, CONFIG.OPC_CTRL_UHID, CONFIG.OPC_CTRL_NONE) or string.format("%s!%s!^%s", CONFIG.OPC_CTRL_TOTAL, CONFIG.OPC_CTRL_UHID, CONFIG.OPC_CTRL_NONE))

    local comando_yad = string.format([[yad --title="%s" --window-icon="smartphone" --image="%s" %s --form --width=%d --borders=%d --text="%s\n%s" --separator="|" --field="%s" "%s" --field="%s" "%s" --field="%s" "%s" --field="%s" "%s" --field="%s" "%s" --field="%s" "%s" --field="%s" "%s" --field="%s:CHK" %s --field="%s:CHK" %s --field="%s:CHK" %s --field="%s:CHK" %s --button="%s" --button="%s" --button="%s"]], 
    CONFIG.TXT_TITULO_APP, imagem_lateral, arg_css, CONFIG.LARGURA_JANELA, CONFIG.BORDAS_JANELA, CONFIG.TXT_CABECALHO, CONFIG.TXT_SUBTITULO, 
    CONFIG.LBL_CONEXAO, def_conexao, CONFIG.LBL_VID_SRC, def_src, CONFIG.LBL_PERF_V, def_perf, CONFIG.LBL_CODEC_V, def_codec, 
    CONFIG.LBL_FPS, def_fps, CONFIG.LBL_AUDIO_OUT, def_audio, CONFIG.LBL_CONTROL, def_control, 
    CONFIG.LBL_AWAKE, (c.awake == "true") and "TRUE" or "FALSE", CONFIG.LBL_SCR_OFF, (c.scr_off == "true") and "TRUE" or "FALSE", 
    CONFIG.LBL_ON_TOP, (c.on_top == "true") and "TRUE" or "FALSE", CONFIG.LBL_RECORD, (c.record == "true") and "TRUE" or "FALSE", 
    CONFIG.BTN_INICIAR, CONFIG.BTN_MANUAL, CONFIG.BTN_CANCEL)

    return os.execute(comando_yad .. " > /tmp/scrcpy_out.txt 2>/dev/null")
end

-- Loop de execução estável da GUI
while true do
    local _, _, ret = abrir_interface()
    
    if ret == 1 or ret == 256 then 
        os.exit(0)
    elseif ret == 2 then 
        os.execute(string.format("yad --title='Manual e Tutorial de Configuração' --image='help-browser' --text=\"%s\" --button='Voltar para o Painel:0' --width=520", CONFIG.MANUAL_TEXTO))
    elseif ret == 0 then 
        local f = io.open("/tmp/scrcpy_out.txt", "r")
        local resposta = f:read("*a")
        f:close()
        
        local valores = {}
        for valor in string.gmatch(resposta, "([^|]+)") do table.insert(valores, valor) end

        if string.find(valores[1], "Cadastrar", 1, true) then
            os.execute("adb start-server >/dev/null 2>&1")
            os.execute(string.format("yad --title='Assistente' --image='network-wireless' --text=\"%s\" --button='Analisar:0' --width=450", CONFIG.WIZARD_PASSO1))
            
            local p_ip = io.popen("adb shell ip route | awk '{print $NF}' | grep -E '^192\\.' | head -n 1 2>/dev/null")
            local novo_ip = p_ip:read("*a"):gsub("%s+", "")
            p_ip:close()
            
            if novo_ip and novo_ip ~= "" then
                c.ip = novo_ip
                os.execute("adb tcpip 5555")
                os.execute(string.format("yad --title='Sucesso' --image='dialog-ok' --text=\"%s\" --button='Concluir:0' --width=400", CONFIG.WIZARD_SUCESSO))
                c.modo = "wifi"
            else
                os.execute(string.format("yad --title='Erro' --image='dialog-error' --text=\"%s\" --button='Voltar:0' --width=400", CONFIG.WIZARD_OFFLINE))
                c.modo = "usb"
            end
        elseif string.find(valores[1], "Cabo", 1, true) then c.modo = "usb"
        else c.modo = "wifi" end

        c.vid_src = string.find(valores[2], "Traseira", 1, true) and "cam_t" or (string.find(valores[2], "Frontal", 1, true) and "cam_f" or "tela")
        c.perf    = string.find(valores[3], "Ultra", 1, true) and "ultra" or (string.find(valores[3], "Alta", 1, true) and "high" or "balance")
        c.codec_v = string.find(valores[4], "H.265", 1, true) and "h265" or (string.find(valores[4], "AV1", 1, true) and "av1" or "h264")
        c.fps     = string.find(valores[5], "60", 1, true) and "60" or "30"
        c.audio   = string.find(valores[6], "Opus", 1, true) and "opus" or (string.find(valores[6], "AAC", 1, true) and "aac" or "mute")
        c.ctrl    = string.find(valores[7], "Teclado", 1, true) and "sdk" or (string.find(valores[7], "Gamepad", 1, true) and "uhid" or "none")
        
        c.awake   = (valores[8] == "TRUE") and "true" or "false"
        c.scr_off = (valores[9] == "TRUE") and "true" or "false"
        c.on_top  = (valores[10] == "TRUE") and "true" or "false"
        c.record  = (valores[11] == "TRUE") and "true" or "false"

        save_config(c)
        encerrar_tudo()
        os.execute("nohup " .. SCRIPT_PATH .. " start_bg >/dev/null 2>&1 &")
        break
    end
end
