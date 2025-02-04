<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caça ao Tesouro Romântica em Florianópolis</title>
    <style>
        body {
            font-family: 'Courier New', cursive;
            text-align: center;
            margin: 0;
            padding: 0;
            background: url('https://drive.google.com/uc?id=1ucn22uaFBDVc2sIo-HpdSvqhU_lK0oT-') no-repeat center center fixed;
            background-size: cover;
            color: #fff;
            overflow: hidden;
        }
        #brasao {
            width: 100px;
            margin: 20px auto;
        }
        #avatar-selection {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 20px;
        }
        .avatar {
            cursor: pointer;
            width: 100px;
            border: 2px solid transparent;
            border-radius: 50%;
            transition: transform 0.3s, border-color 0.3s;
        }
        .avatar:hover {
            transform: scale(1.1);
            border-color: #ff6f91;
        }
        #pista-container {
            display: none;
            margin: 20px auto;
            padding: 20px;
            background-color: rgba(255, 255, 255, 0.9);
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            width: 80%;
            max-width: 600px;
            animation: fadeIn 1s;
            color: #333;
        }
        button {
            padding: 15px 20px;
            cursor: pointer;
            margin-top: 10px;
            border: none;
            background-color: #ff6f91;
            color: #ffffff;
            border-radius: 8px;
            font-size: 18px;
            transition: background-color 0.3s, transform 0.3s;
            display: block;
            margin: 10px auto;
            width: 80%;
        }
        button:hover {
            background-color: #ff4e67;
            transform: scale(1.1);
        }
        #mensagem {
            margin-top: 20px;
            font-weight: bold;
            color: #333333;
        }
        #mapa {
            margin-top: 20px;
            height: 400px;
            width: 100%;
            max-width: 600px;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .heart {
            position: absolute;
            width: 50px;
            height: 50px;
            background: url('https://drive.google.com/uc?id=17jgRNtqBFcVhr4b-S4-Ko6pYagFMdqSD') no-repeat center center;
            background-size: cover;
            animation: float 5s infinite;
            opacity: 0.8;
        }
        @keyframes float {
            0% { transform: translateY(0); }
            50% { transform: translateY(-100px); }
            100% { transform: translateY(0); }
        }
        .correct {
            color: green;
            font-weight: bold;
            animation: correctAnimation 1s forwards;
        }
        .incorrect {
            color: red;
            font-weight: bold;
            animation: incorrectAnimation 1s forwards;
        }
        @keyframes correctAnimation {
            from { transform: scale(1); }
            to { transform: scale(1.1); }
        }
        @keyframes incorrectAnimation {
            from { transform: scale(1); }
            to { transform: scale(0.9); }
        }
    </style>
</head>
<body>
    <img id="brasao" src="https://drive.google.com/uc?id=1JeGOidonTIDj0Z0vuEC-jjRM6CAoBpBX" alt="Brasão UniGOIAS">
    <h1>Bem-vinda à Caça ao Tesouro Romântica em Florianópolis!</h1>
    <p>Escolha um avatar para começar a sua aventura romântica.</p>
    <div id="avatar-selection">
        <img src="https://example.com/avatar1.png" class="avatar" alt="Avatar 1" onclick="selecionarAvatar('avatar1')">
        <img src="https://example.com/avatar2.png" class="avatar" alt="Avatar 2" onclick="selecionarAvatar('avatar2')">
        <img src="https://example.com/avatar3.png" class="avatar" alt="Avatar 3" onclick="selecionarAvatar('avatar3')">
    </div>
    <div id="pista-container">
        <h2 id="pista"></h2>
        <div id="opcoes"></div>
        <p id="mensagem"></p>
        <div id="mapa"></div>
    </div>
    
    <button id="iniciar-button" onclick="iniciarJogo()" style="display:none;">Iniciar Jogo</button>

    <!-- Adicionando o leitor de QR Code -->
    <div id="qr-reader" style="width: 500px; margin: 20px auto;"></div>
    <p id="qr-reader-results"></p>

    <!-- Adicionando sons -->
    <audio id="somCorreto" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="somIncorreto" src="https://www.soundjay.com/button/beep-10.wav"></audio>
    <audio id="musicaFundo" src="https://www.soundjay.com/nature/sounds/rain-01.mp3" loop></audio>

    <!-- Adicionando o assistente virtual -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/annyang/2.6.1/annyang.min.js"></script>
    
    <script src="https://unpkg.com/html5-qrcode/minified/html5-qrcode.min.js"></script>
    <script>
        const pistas = [
            {
                charada: "Um espelho d’água onde histórias de amor são refletidas, rodeado por dunas e natureza. Onde estou?",
                localizacao: { lat: -27.5969, lng: -48.5012 },
                resposta: "Lagoa da Conceição",
                proximaPista: "Parabéns! Vá até a Lagoa da Conceição para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr1.png"
            },
            {
                charada: "Ela já foi um símbolo do passado, mas hoje conecta o coração da cidade. Iluminada à noite, guarda segredos e promessas. Onde estou?",
                localizacao: { lat: -27.5945, lng: -48.5480 },
                resposta: "Ponte Hercílio Luz",
                proximaPista: "Muito bem! Vá até a Ponte Hercílio Luz para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr2.png"
            },
            {
                charada: "Um deserto dourado no meio da ilha, onde aventureiros deslizam ao vento. Um dos lugares mais únicos para um encontro. Onde estou?",
                localizacao: { lat: -27.6229, lng: -48.4331 },
                resposta: "Dunas da Joaquina",
                proximaPista: "Ótimo! Vá até as Dunas da Joaquina para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr3.png"
            },
            {
                charada: "Um paraíso de luxo e diversão, onde o pôr do sol é digno de aplausos e os casais aproveitam a brisa do mar. Onde estou?",
                localizacao: { lat: -27.4367, lng: -48.4742 },
                resposta: "Praia de Jurerê Internacional",
                proximaPista: "Muito bom! Vá até a Praia de Jurerê Internacional para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr4.png"
            },
            {
                charada: "Aqui o aroma de frutos do mar e a música ao vivo criam uma atmosfera encantadora. Muitos encontros já começaram entre essas mesas. Onde estou?",
                localizacao: { lat: -27.5954, lng: -48.5480 },
                resposta: "Mercado Público de Florianópolis",
                proximaPista: "Ótimo! Vá até o Mercado Público de Florianópolis para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr5.png"
            },
            {
                charada: "Deste ponto alto, toda a cidade se revela diante dos olhos. Um lugar perfeito para uma declaração apaixonada. Onde estou?",
                localizacao: { lat: -27.5878, lng: -48.5253 },
                resposta: "Mirante do Morro da Cruz",
                proximaPista: "Parabéns! Vá até o Mirante do Morro da Cruz para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr6.png"
            }
        ];
        
        let indiceAtual = 0;
        let avatarSelecionado = '';
        
        function selecionarAvatar(avatar) {
            avatarSelecionado = avatar;
            document.getElementById("iniciar-button").style.display = "block";
            const avatares = document.querySelectorAll('.avatar');
            avatares.forEach(av => av.style.borderColor = 'transparent');
            document.querySelector(`[alt="${avatar.charAt(0).toUpperCase() + avatar.slice(1)}"]`).style.borderColor = '#ff6f91';
        }

        function iniciarJogo() {
            document.getElementById("pista-container").style.display = "block";
            mostrarCharada(indiceAtual);
            criarCorações();
            document.getElementById("musicaFundo").play();
        }
        
        function mostrarCharada(indice) {
            const pista = pistas[indice];
            document.getElementById("pista").textContent = pista.charada;
            const opcoesContainer = document.getElementById("opcoes");
            opcoesContainer.innerHTML = "";
            const botao = document.createElement("button");
            botao.textContent = "Verificar Localização";
            botao.onclick = () => verificarLocalizacao(pista.localizacao);
            opcoesContainer.appendChild(botao);
        }
        
        function verificarLocalizacao(localizacao) {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(
                    (position) => {
                        const distancia = calcularDistancia(position.coords.latitude, position.coords.longitude, localizacao.lat, localizacao.lng);
                        if (distancia < 0.05) { // 50 meters radius
                            desbloquearProximaPista();
                        } else {
                            document.getElementById("mensagem").textContent = "Você ainda não está no local correto. Continue buscando!";
                        }
                    },
                    (error) => {
                        console.error(error);
                        document.getElementById("mensagem").textContent = "Erro ao obter localização. Tente novamente.";
                    }
                );
            } else {
                document.getElementById("mensagem").textContent = "Geolocalização não é suportada pelo seu navegador.";
            }
        }

        function calcularDistancia(lat1, lon1, lat2, lon2) {
            const R = 6371; // Radius of the Earth in km
            const dLat = (lat2 - lat1) * (Math.PI / 180);
            const dLon = (lon2 - lon1) * (Math.PI / 180);
            const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) +
                      Math.cos(lat1 * (Math.PI / 180)) * Math.cos(lat2 * (Math.PI / 180)) *
                      Math.sin(dLon / 2) * Math.sin(dLon / 2);
            const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
            const d = R * c; // Distance in km
            return d;
        }
        
        function desbloquearProximaPista() {
            const somCorreto = document.getElementById("somCorreto");
            somCorreto.play();
            document.getElementById("mensagem").textContent = pistas[indiceAtual].proximaPista;
            const qrCodeImage = document.createElement('img');
            qrCodeImage.src = pistas[indiceAtual].qrCode;
            qrCodeImage.alt = "QR Code";
            qrCodeImage.style.maxWidth = "200px";
            document.getElementById("mensagem").appendChild(qrCodeImage);
            mostrarMapa(pistas[indiceAtual].localizacao);
            indiceAtual++;
            if (indiceAtual < pistas.length) {
                setTimeout(() => {
                    mostrarCharada(indiceAtual);
                    document.getElementById("mensagem").textContent = "";
                }, 3000);
            } else {
                document.getElementById("mensagem").textContent = "Parabéns! Você completou a caça ao tesouro e encontrou seu presente!";
            }
        }

        function mostrarMapa(localizacao) {
            const mapa = document.getElementById("mapa");
            mapa.innerHTML = `<iframe width="600" height="450" style="border:0" loading="lazy" allowfullscreen
                src="https://www.google.com/maps/embed/v1/place?key=YOUR_API_KEY&q=${localizacao.lat},${localizacao.lng}"></iframe>`;
        }

        function criarCorações() {
            const numCorações = 20;
            for (let i = 0; i < numCorações; i++) {
                const coração = document.createElement('div');
                coração.className = 'heart';
                coração.style.left = `${Math.random() * 100}vw`;
                coração.style.animationDuration = `${Math.random() * 5 + 5}s`;
                document.body.appendChild(coração);
            }
        }

        // Leitor de QR Code
        function onScanSuccess(decodedText, decodedResult) {
            document.getElementById('qr-reader-results').innerHTML = `Código QR Lido: ${decodedText}`;
            verificarResposta(decodedText);
        }

        var html5QrcodeScanner = new Html5QrcodeScanner(
            "qr-reader", { fps: 10, qrbox: 250 });
        html5QrcodeScanner.render(onScanSuccess);

        // Assistente virtual
        if (annyang) {
            const comandos = {
                'iniciar jogo': iniciarJogo,
                'responder *resposta': verificarResposta,
            };
            annyang.addCommands(comandos);
            annyang.start();
        }
    </script>
</body>
</html>
