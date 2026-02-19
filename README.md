<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>iObras - Chat Acessível</title>
    <style>
        :root {
            --bg-chat: #1a1a1a;
            --bubble-me: #0056b3;
            --bubble-you: #333333;
            --text: #ffffff;
            --accent: #FFCC00;
        }

        body { font-family: sans-serif; background: #f0f0f0; display: flex; justify-content: center; padding: 20px; }
        
        .chat-container {
            width: 100%; max-width: 400px; height: 600px;
            background: var(--bg-chat); border-radius: 20px;
            display: flex; flex-direction: column; overflow: hidden;
        }

        .chat-header { padding: 15px; background: #000; color: var(--accent); text-align: center; font-weight: bold; }

        .messages { flex: 1; padding: 15px; overflow-y: auto; display: flex; flex-direction: column; gap: 10px; }

        .bubble { padding: 12px; border-radius: 15px; max-width: 80%; font-size: 1rem; line-height: 1.4; }
        .me { align-self: flex-end; background: var(--bubble-me); color: white; border-bottom-right-radius: 2px; }
        .you { align-self: flex-start; background: var(--bubble-you); color: white; border-bottom-left-radius: 2px; }

        .input-area { padding: 15px; background: #000; display: flex; gap: 10px; align-items: center; }

        input { 
            flex: 1; padding: 12px; border-radius: 25px; border: none; 
            font-size: 1rem; background: #333; color: white;
        }

        .btn-icon {
            background: var(--accent); border: none; width: 45px; height: 45px;
            border-radius: 50%; cursor: pointer; font-size: 1.2rem;
            display: flex; align-items: center; justify-content: center;
        }

        .listening { animation: pulse 1.5s infinite; background: #ff4444; }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.1); } 100% { transform: scale(1); } }
    </style>
</head>
<body>

<main class="chat-container">
    <div class="chat-header" role="banner">Conversa com João Pintor</div>
    
    <section class="messages" id="chat-box" aria-live="polite">
        <div class="bubble you">Olá! Podemos agendar a visita para o orçamento?</div>
        <div class="bubble me">Claro! Pode ser amanhã às 9h?</div>
    </section>

    <div class="input-area">
        <input type="text" id="msg-input" placeholder="Digite ou use a voz..." aria-label="Escrever mensagem">
        
        <button id="voice-btn" class="btn-icon" aria-label="Ditar mensagem por voz" title="Clique para ditar">
            🎤
        </button>
        
        <button id="send-btn" class="btn-icon" aria-label="Enviar mensagem">
            ▶
        </button>
    </div>
</main>

<script>
    const voiceBtn = document.getElementById('voice-btn');
    const inputField = document.getElementById('msg-input');
    const chatBox = document.getElementById('chat-box');

    // Configuração da API de Reconhecimento de Voz
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    
    if (SpeechRecognition) {
        const recognition = new SpeechRecognition();
        recognition.lang = 'pt-BR';
        recognition.interimResults = false;

        voiceBtn.addEventListener('click', () => {
            recognition.start();
            voiceBtn.classList.add('listening');
        });

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript;
            inputField.value = transcript;
            voiceBtn.classList.remove('listening');
            // Feedback tátil simples (se disponível em mobile)
            if (navigator.vibrate) navigator.vibrate(50);
        };

        recognition.onspeechend = () => {
            recognition.stop();
            voiceBtn.classList.remove('listening');
        };

        recognition.onerror = () => {
            voiceBtn.classList.remove('listening');
            alert("Erro ao acessar microfone. Verifique as permissões.");
        };
    } else {
        voiceBtn.style.display = 'none'; // Esconde se o navegador não suportar
        console.log("Navegador não suporta Web Speech API");
    }

    // Função simples de enviar
    document.getElementById('send-btn').onclick = () => {
        if(inputField.value.trim() !== "") {
            const newMessage = document.createElement('div');
            newMessage.className = 'bubble me';
            newMessage.textContent = inputField.value;
            chatBox.appendChild(newMessage);
            inputField.value = "";
            chatBox.scrollTop = chatBox.scrollHeight;
        }
    };
</script>

</body>
</html># fuzzy-guide
iObra serviço de reformas e construções
