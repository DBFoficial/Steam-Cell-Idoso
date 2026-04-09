<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SêniorPhone Neo - Versão Completa</title>
    <style>
        :root {
            --neon-blue: #00f3ff;
            --neon-pink: #ff00ff;
            --bg-dark: #050505;
            --app-bg: #ffffff;
            --msg-sent: #007aff;
            --msg-received: #e2e2e7;
            --success: #28a745;
            --danger: #dc3545;
        }

        body {
            font-family: 'Segoe UI', Roboto, sans-serif;
            margin: 0; background-color: var(--bg-dark); color: white;
            display: flex; justify-content: center; align-items: center;
            height: 100vh; width: 100vw; overflow: hidden;
        }

        /* --- CONTAINER DO DISPOSITIVO --- */
        #phone-container {
            width: 380px; height: 780px; background: #000;
            border: 14px solid #1a1a1a; border-radius: 60px;
            position: relative; box-shadow: 0 0 60px rgba(0, 243, 255, 0.2);
            overflow: hidden; display: none;
        }

        /* --- TELA DE ENTRADA --- */
        #landing-page { text-align: center; z-index: 5; }
        #landing-page h1 { font-size: 3.5rem; text-transform: uppercase; text-shadow: 0 0 15px var(--neon-blue); margin: 0; }
        .btn-start { background: transparent; color: var(--neon-blue); border: 2px solid var(--neon-blue); padding: 15px 40px; font-size: 1.2rem; font-weight: bold; cursor: pointer; box-shadow: 0 0 15px var(--neon-blue); margin-top: 20px; transition: 0.3s; }
        .btn-start:hover { background: var(--neon-blue); color: #000; box-shadow: 0 0 40px var(--neon-blue); }

        /* --- HOME SCREEN --- */
        #home-screen {
            height: 100%; display: grid; grid-template-columns: 1fr;
            grid-template-rows: repeat(5, 1fr); gap: 15px;
            padding: 60px 25px 80px; box-sizing: border-box;
            background: radial-gradient(circle at center, #1a0033 0%, #000 100%);
        }

        .app-icon {
            display: flex; align-items: center; padding: 0 25px;
            background: rgba(255, 255, 255, 0.1); border-radius: 25px;
            border: 1px solid rgba(255, 255, 255, 0.2); backdrop-filter: blur(10px);
            cursor: pointer; transition: 0.2s;
        }
        .app-icon:active { transform: scale(0.95); }
        .app-icon span { font-size: 1.5rem; font-weight: bold; color: #fff; margin-left: 15px; }

        /* --- INTERFACE DE APPS --- */
        .app-view {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--app-bg); color: #333; display: none;
            flex-direction: column; z-index: 10; padding-top: 40px; box-sizing: border-box;
        }

        .app-header { font-size: 1.6rem; font-weight: 800; padding: 15px 20px; border-bottom: 2px solid #eee; background: white; flex-shrink: 0; display: flex; align-items: center; gap: 10px; }

        /* --- APP MENSAGENS --- */
        .chat-list-item { padding: 20px; border-bottom: 1px solid #eee; cursor: pointer; }
        .chat-preview { font-size: 1.1rem; color: #666; padding: 10px; background: #f1f3f4; border-radius: 12px; border-left: 4px solid var(--msg-sent); margin-top: 5px; }
        
        #active-chat { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: white; display: none; flex-direction: column; z-index: 20; padding-top: 40px; }
        #messages-container { flex-grow: 1; overflow-y: auto; padding: 15px; display: flex; flex-direction: column-reverse; gap: 12px; background: #f0f2f5; margin-bottom: 30px; }
        .bubble { max-width: 85%; padding: 12px 18px; border-radius: 20px; font-size: 1.2rem; }
        .bubble.sent { background: var(--msg-sent); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .bubble.received { background: var(--msg-received); align-self: flex-start; border-bottom-left-radius: 4px; }
        .quick-replies { display: flex; gap: 8px; overflow-x: auto; padding: 10px; background: #f8f9fa; border-bottom: 2px solid #ddd; }
        .quick-btn { background: white; border: 2px solid var(--msg-sent); padding: 10px 18px; border-radius: 15px; font-weight: bold; color: var(--msg-sent); white-space: nowrap; cursor: pointer; }

        /* --- APP TELEFONE --- */
        #display-numero { font-size: 2.8rem; text-align: center; margin: 20px 0; height: 60px; font-weight: bold; color: #000; letter-spacing: 2px; }
        .dialer-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; padding: 0 30px; }
        .dial-btn { background: #f0f2f5; padding: 20px; border-radius: 50%; font-size: 1.8rem; font-weight: bold; text-align: center; cursor: pointer; border: 1px solid #ddd; }
        .dial-btn:active { background: #ddd; }

        /* --- APP SOS & MÉDICO --- */
        .sos-container { padding: 20px; display: flex; flex-direction: column; gap: 20px; overflow-y: auto; }
        .btn-panic { background: var(--danger); color: white; border: none; padding: 30px; border-radius: 20px; font-size: 1.8rem; font-weight: bold; cursor: pointer; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(220, 53, 69, 0.6); } 70% { box-shadow: 0 0 0 20px rgba(220, 53, 69, 0); } 100% { box-shadow: 0 0 0 0 rgba(220, 53, 69, 0); } }
        .medical-card { background: #fff1f1; border: 2px solid #ffcccc; border-radius: 20px; padding: 20px; color: #333; }
        .medical-item { margin-bottom: 12px; font-size: 1.1rem; border-bottom: 1px solid #ffdddd; padding-bottom: 5px; }
        .medical-item b { color: #666; font-size: 0.85rem; display: block; text-transform: uppercase; }

        /* --- TELA DE CHAMADA --- */
        #calling-screen { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: #1a1a1a; color: white; z-index: 100; display: none; flex-direction: column; align-items: center; justify-content: center; }
        .btn-end { background: var(--danger); color: white; border: none; padding: 25px; border-radius: 50%; font-size: 2rem; margin-top: 40px; cursor: pointer; }

        /* --- BARRA HOME --- */
        .home-bar { width: 120px; height: 6px; background: rgba(0,0,0,0.2); border-radius: 10px; position: absolute; bottom: 12px; left: 50%; transform: translateX(-50%); z-index: 110; cursor: pointer; }
        .home-bar.on-dark { background: rgba(255,255,255,0.4); }
    </style>
</head>
<body>

    <div id="landing-page">
        <h1>Sênior <span style="color: var(--neon-blue);">Neo</span></h1>
        <button class="btn-start" onclick="startSim()">INICIAR SIMULAÇÃO</button>
    </div>

    <div id="phone-container">
        
        <div id="calling-screen">
            <div style="font-size: 5rem;">👤</div>
            <div id="caller-name" style="font-size: 2rem; margin-top: 20px;">Chamando...</div>
            <div id="call-timer" style="color: var(--neon-blue); font-size: 1.5rem; margin-top: 10px;">Conectando...</div>
            <button class="btn-end" onclick="endCall()">❌</button>
        </div>

        <div class="screen-content">
            <div id="home-screen">
                <div class="app-icon" onclick="openApp('app-phone')"><span>📞 TELEFONE</span></div>
                <div class="app-icon" onclick="openApp('app-messages')"><span>💬 MENSAGENS</span></div>
                <div class="app-icon" onclick="openApp('app-family')"><span>👨‍👩‍👧 FAMÍLIA</span></div>
                <div class="app-icon" onclick="openApp('app-sos')" style="background: rgba(255,0,0,0.15); border-color: red;">
                    <span style="color: #ff4d4d;">🆘 SOS / MÉDICO</span>
                </div>
                <div class="app-icon" onclick="openApp('app-photos')"><span>📸 FOTOS</span></div>
            </div>

            <div id="app-phone" class="app-view">
                <div class="app-header">📞 Discador</div>
                <div id="display-numero"></div>
                <div class="dialer-grid" id="tg"></div>
                <button onclick="makeCall('numero')" style="background:var(--success); color:white; border:none; margin: 20px 30px; padding:20px; border-radius:20px; font-size:1.5rem; font-weight:bold; cursor:pointer;">LIGAR</button>
            </div>

            <div id="app-messages" class="app-view">
                <div class="app-header">💬 Conversas</div>
                <div class="chat-list-item" onclick="openChat('Maria (Filha)', 'Pai, já tomou o remédio?')">
                    <div style="display:flex; justify-content:space-between; font-weight:bold;"><span>👩‍🦰 Maria (Filha)</span><span style="color:#888; font-size:0.9rem;">10:30</span></div>
                    <div class="chat-preview" id="preview-Maria">Pai, já tomou o remédio?</div>
                </div>
                <div class="chat-list-item" onclick="openChat('Lucas (Neto)', 'Vovô, te amo! ❤️')">
                    <div style="display:flex; justify-content:space-between; font-weight:bold;"><span>👦 Lucas (Neto)</span><span style="color:#888; font-size:0.9rem;">Ontem</span></div>
                    <div class="chat-preview" id="preview-Lucas">Vovô, te amo! ❤️</div>
                </div>

                <div id="active-chat">
                    <div class="app-header">
                        <span onclick="closeChat()" style="cursor:pointer; font-size: 2rem; padding-right:15px;">‹</span> 
                        <span id="active-chat-name">Nome</span>
                    </div>
                    <div class="quick-replies">
                        <button class="quick-btn" onclick="sendMsg('Sim, já tomei!')">Sim, já tomei!</button>
                        <button class="quick-btn" onclick="sendMsg('Tudo bem por aqui.')">Tudo bem.</button>
                        <button class="quick-btn" onclick="sendMsg('Me liga?')">Me liga?</button>
                        <button class="quick-btn" onclick="sendMsg('OK')">OK</button>
                    </div>
                    <div id="messages-container"></div>
                </div>
            </div>

            <div id="app-family" class="app-view">
                <div class="app-header">👨‍👩‍👧 Contatos Rápidos</div>
                <div style="padding:20px; background:#f8f9fa; margin:15px; border-radius:20px; font-weight:bold; cursor:pointer; border:1px solid #ddd;" onclick="makeCall('Maria (Filha)')">📞 Ligar para Maria</div>
                <div style="padding:20px; background:#f8f9fa; margin:15px; border-radius:20px; font-weight:bold; cursor:pointer; border:1px solid #ddd;" onclick="makeCall('Lucas (Neto)')">📞 Ligar para Lucas</div>
            </div>

            <div id="app-sos" class="app-view">
                <div class="app-header" style="color:var(--danger)">🆘 Emergência</div>
                <div class="sos-container">
                    <button class="btn-panic" onclick="makeCall('192 - AMBULÂNCIA')">LIGAR PARA SAMU</button>
                    <div class="medical-card">
                        <h3 style="margin:0 0 15px 0; color:var(--danger)">📋 Ficha Médica</h3>
                        <div class="medical-item"><b>Paciente</b> Sr. José da Silva</div>
                        <div class="medical-item"><b>Sangue</b> O Positivo (O+)</div>
                        <div class="medical-item"><b>Alergias</b> Penicilina, Corantes</div>
                        <div class="medical-item"><b>Crônico</b> Diabetes, Hipertensão</div>
                        <div class="medical-item" style="border:none"><b>Emergência</b> Maria (Filha): (11) 9999-8888</div>
                    </div>
                </div>
            </div>

            <div id="app-photos" class="app-view">
                <div class="app-header">📸 Galeria</div>
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; padding:15px;">
                    <div style="background:#ddd; height:120px; border-radius:10px; display:flex; align-items:center; justify-content:center; font-size:2rem;">🌅</div>
                    <div style="background:#ddd; height:120px; border-radius:10px; display:flex; align-items:center; justify-content:center; font-size:2rem;">🎂</div>
                </div>
            </div>

            <div id="hb" class="home-bar on-dark" onclick="goHome()"></div>
        </div>
    </div>

    <script>
        let currentNum = "";
        let chatContact = "";

        // Inicializar Teclado
        const tg = document.getElementById('tg');
        "123456789C0#".split('').forEach(v => {
            const b = document.createElement('div');
            b.className = 'dial-btn'; b.innerText = v;
            b.onclick = () => {
                if(v === 'C') currentNum = "";
                else if(currentNum.length < 11) currentNum += v;
                document.getElementById('display-numero').innerText = currentNum;
            };
            tg.appendChild(b);
        });

        function startSim() { document.getElementById('landing-page').style.display = 'none'; document.getElementById('phone-container').style.display = 'block'; }
        
        function openApp(id) {
            document.querySelectorAll('.app-view').forEach(v => v.style.display = 'none');
            document.getElementById('home-screen').style.display = 'none';
            document.getElementById(id).style.display = 'flex';
            document.getElementById('hb').classList.remove('on-dark');
        }

        function goHome() {
            document.getElementById('home-screen').style.display = 'grid';
            document.querySelectorAll('.app-view').forEach(v => v.style.display = 'none');
            document.getElementById('calling-screen').style.display = 'none';
            document.getElementById('hb').classList.add('on-dark');
            clearInterval(window.timer);
        }

        // Chamadas
        function makeCall(target) {
            if(target === 'numero' && currentNum === "") return;
            document.getElementById('caller-name').innerText = (target === 'numero' ? currentNum : target);
            document.getElementById('calling-screen').style.display = 'flex';
            document.getElementById('hb').classList.add('on-dark');
            let s = 0; 
            window.timer = setInterval(() => { 
                s++; 
                document.getElementById('call-timer').innerText = `Duração: 00:${s < 10 ? '0'+s : s}`;
            }, 1000);
        }

        function endCall() { 
            document.getElementById('calling-screen').style.display = 'none'; 
            document.getElementById('hb').classList.remove('on-dark');
            currentNum = ""; document.getElementById('display-numero').innerText = "";
            clearInterval(window.timer); 
        }

        // Chat
        function openChat(name, msg) {
            chatContact = name;
            document.getElementById('active-chat').style.display = 'flex';
            document.getElementById('active-chat-name').innerText = name;
            document.getElementById('messages-container').innerHTML = `<div class="bubble received">${msg}</div>`;
        }

        function closeChat() { document.getElementById('active-chat').style.display = 'none'; }

        function sendMsg(text) {
            const container = document.getElementById('messages-container');
            const m = document.createElement('div');
            m.className = 'bubble sent';
            m.innerText = text;
            container.prepend(m);

            // Atualizar Prévia na lista
            const clean = chatContact.split(' ')[0];
            document.getElementById(`preview-${clean}`).innerText = `Você: ${text}`;

            setTimeout(() => {
                const r = document.createElement('div');
                r.className = 'bubble received';
                r.innerText = "Combinado! 😊";
                container.prepend(r);
                document.getElementById(`preview-${clean}`).innerText = "Combinado! 😊";
            }, 1000);
        }
    </script>
</body>
</html>
