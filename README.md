<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caça ao Tesouro </title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background: linear-gradient(120deg, #ff758c, #ff7eb3);
            color: #fff;
            overflow-x: hidden;
        }
        h1 {
            font-family: 'Dancing Script', cursive;
            font-size: 3em;
            margin-top: 20px;
        }
        #form-container, #inicio-container, #pista-container {
            display: none;
            background: rgba(255, 255, 255, 0.9);
            color: #333;
            padding: 20px;
            border-radius: 10px;
            margin: 20px auto;
            max-width: 600px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            animation: fadeIn 1s;
        }
        #form-container.active, #inicio-container.active, #pista-container.active {
            display: block;
        }
        input, button {
            padding: 15px 20px;
            margin: 10px;
            border: none;
            border-radius: 8px;
            font-size: 18px;
        }
        button {
            background-color: #ff4081;
            color: #ffffff;
            cursor: pointer;
            transition: background-color 0.3s, transform 0.3s;
        }
        button:hover {
            background-color: #e91e63;
            transform: scale(1.1);
        }
        #mapa {
            width: 100%;
            height: 300px;
            margin-top: 10px;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        img {
            max-width: 100%;
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <div id="form-container" class="active">
        <h1>Identifique-se</h1>
        <form id="identification-form">
            <input type="text" id="nome" placeholder="Nome" required>
            <input type="text" id="cidade" placeholder="Cidade de Origem" required>
            <input type="date" id="data-jogo" placeholder="Data do Jogo" required>
            <button type="submit">Iniciar</button>
        </form>
    </div>
    <div id="inicio-container">
        <h1>Bem-vindo(a), <span id="user-nome"></span>!</h1>
        <p>Origem: <span id="user-cidade"></span></p>
        <p>Data do Jogo: <span id="user-data"></span></p>
        <img src= https://drive.google.com/file/d/1yG53CU4XX6-Aap6FJBHl_PLKvccgXg0j/view?usp=drive_link alt="Imagem Enigmática">
        <button onclick="mostrarPista()">Começar o Jogo</button>
    </div>
    <div id="pista-container">
        <h2 id="pista"></h2>
        <p id="mensagem"></p>
        <button onclick="verificarLocalizacao()">Verificar Localização</button>
        <div id="mapa"></div>
    </div>

    <!-- Adicionando sons -->
    <audio id="somCorreto" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="somIncorreto" src="https://www.soundjay.com/button/beep-10.wav"></audio>
    <audio id="musicaFundo" src="https://www.soundjay.com/nature/sounds/rain-01.mp3" loop></audio>

    <script>
        const pistasOriginais = [
            { charada: "🌊 Um espelho d’água cercado por dunas e natureza. Casais adoram remar aqui. Onde estou?", latitude: -27.5969, longitude: -48.4846, nome: "Lagoa da Conceição" },
            { charada: "🌉 Uma ponte que une passado e presente, iluminando noites românticas. Onde estou?", latitude: -27.5973, longitude: -48.5515, nome: "Ponte Hercílio Luz" },
            { charada: "🏄‍♂️ Dunas douradas onde aventureiros deslizam ao vento. Um encontro perfeito. Onde estou?", latitude: -27.6206, longitude: -48.4354, nome: "Dunas da Joaquina" },
            { charada: "🏖️ Um paraíso de luxo e diversão onde o pôr do sol é digno de aplausos. Onde estou?", latitude: -27.4368, longitude: -48.4916, nome: "Praia de Jurerê" },
            { charada: "🍽️ Frutos do mar, cultura e encontros românticos entre as mesas. Onde estou?", latitude: -27.5951, longitude: -48.5480, nome: "Mercado Público" },
            { charada: "🌅 No alto da ilha, uma vista que revela toda a beleza de Floripa. Onde estou?", latitude: -27.5888, longitude: -48.5350, nome: "Mirante do Morro da Cruz" }
        ];

        let pistas = [];
        let indiceAtual = 0;
        let avatarSelecionado = '';

        document.getElementById('identification-form').addEventListener('submit', function(event) {
            event.preventDefault();

            const nome = document.getElementById('nome').value;
            const cidade = document.getElementById('cidade').value;
            const dataJogo = document.getElementById('data-jogo').value;

            document.getElementById('user-nome').innerText = nome;
            document.getElementById('user-cidade').innerText = cidade;
            document.getElementById('user-data').innerText = new Date(dataJogo).toLocaleDateString('pt-BR');

            document.getElementById('form-container').classList.remove('active');
            document.getElementById('inicio-container').classList.add('active');
        });

        function mostrarPista() {
            pistas = embaralharPistas("chave-fixa-teste");

            document.getElementById('inicio-container').classList.remove('active');
            document.getElementById('pista-container').classList.add('active');
            document.getElementById("musicaFundo").play();
            criarCorações();
            exibirPista();
        }

        function embaralharPistas(chave) {
            let seed = hashString(chave);
            let pistasEmbaralhadas = [...pistasOriginais];

            for (let i = pistasEmbaralhadas.length - 1; i > 0; i--) {
                let j = seed % (i + 1);
                [pistasEmbaralhadas[i], pistasEmbaralhadas[j]] = [pistasEmbaralhadas[j], pistasEmbaralhadas[i]];
                seed = (seed * 33) % 1000003;
            }

            return pistasEmbaralhadas;
        }

        function hashString(str) {
            let hash = 0;
            for (let i = 0; i < str.length; i++) {
                hash = (hash * 31 + str.charCodeAt(i)) % 1000003;
            }
            return hash;
        }

        function exibirPista() {
            document.getElementById("pista").textContent = pistas[indiceAtual].charada;
            document.getElementById("mensagem").textContent = `Vá até ${pistas[indiceAtual].nome} e clique no botão abaixo!`;
            mostrarMapa(pistas[indiceAtual].latitude, pistas[indiceAtual].longitude);
        }

        function verificarLocalizacao() {
            // Coordenadas fixas para facilitar os testes
            const latUsuario = -27.5969;  // Latitude da Lagoa da Conceição
            const longUsuario = -48.4846; // Longitude da Lagoa da Conceição
            const latPista = pistas[indiceAtual].latitude;
            const longPista = pistas[indiceAtual].longitude;

            const distancia = calcularDistancia(latUsuario, longUsuario, latPista, longPista);

            if (distancia < 0.2) {
                desbloquearProximaPista();
            } else {
                document.getElementById("mensagem").textContent = "📍 Você ainda não chegou ao local certo! Continue procurando!";
                const somIncorreto = document.getElementById("somIncorreto");
                somIncorreto.play();
            }
        }

        function mostrarMapa(lat, long) {
            document.getElementById("mapa").innerHTML = `<iframe width="100%" height="300" frameborder="0"
                src="https://www.google.com/maps?q=${lat},${long}&output=embed"></iframe>`;
        }

        function calcularDistancia(lat1, lon1, lat2, lon2) {
            const R = 6371; 
            const dLat = (lat2 - lat1) * Math.PI / 180;
            const dLon = (lon2 - lon1) * Math.PI / 180;
            const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
                      Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
                      Math.sin(dLon/2) * Math.sin(dLon/2);
            return R * (2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)));
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
            mostrarMapa(pistas[indiceAtual].latitude, pistas[indiceAtual].longitude);
            indiceAtual++;
            if (indiceAtual < pistas.length) {
                setTimeout(() => {
                    exibirPista();
                    document.getElementById("mensagem").textContent = "";
                }, 3000);
            } else {
                document.getElementById("pista").textContent = "🎉 Parabéns! Você encontrou o tesouro romântico! 🎁";
                document.getElementById("mensagem").textContent = "";
            }
        }

        function criarCorações() {
            const numCoracoes = 20;
            for (let i = 0; i < numCoracoes; i++) {
                const coracao = document.createElement('div');
                coracao.className = 'heart';
                coracao.style.left = `${Math.random() * 100}vw`;
                coracao.style.animationDuration = `${Math.random() * 5 + 5}s`;
                document.body.appendChild(coracao);
            }
        }
    </script>
</body>
</html>
