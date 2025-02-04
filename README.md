<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caça ao Tesouro Romântica - Florianópolis</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <style>
        /* Template de design vibrante e moderno */
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(45deg, #ff4081, #6200ea);
            color: #fff;
            margin: 0;
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
        }
        h1, h2 {
            text-shadow: 2px 2px 10px rgba(0,0,0,0.6);
            font-size: 3rem;
        }
        button {
            background: linear-gradient(45deg, #ff4081, #e91e63);
            border: none;
            color: white;
            padding: 20px 30px;
            font-size: 1.5rem;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        button:hover {
            transform: scale(1.1);
            background: #e91e63;
        }
        #mapa {
            width: 100%;
            height: 500px;
            margin-top: 20px;
            border-radius: 15px;
            box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.3);
        }
        .pista-container {
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 15px;
            animation: fadeIn 1s ease-in-out;
        }
        /* Animação de fade-in */
        @keyframes fadeIn {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }
        /* Avatar interativo */
        .avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: url('https://example.com/avatar.jpg') no-repeat center;
            background-size: cover;
            position: absolute;
            bottom: 30px;
            left: 30px;
            animation: float 2s ease-in-out infinite;
        }
        @keyframes float {
            0% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
            100% { transform: translateY(0); }
        }
        input {
            padding: 15px;
            font-size: 1.2rem;
            margin: 20px 0;
            border-radius: 8px;
            border: none;
        }
        .form-container, .inicio-container {
            display: none;
            background: rgba(255, 255, 255, 0.9);
            color: #333;
            padding: 20px;
            border-radius: 10px;
            margin: 20px auto;
            max-width: 600px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            animation: fadeIn 1s ease-in-out;
        }
        .form-container.active, .inicio-container.active, .pista-container.active {
            display: block;
        }
        .avatar-selection img {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            margin: 10px;
            cursor: pointer;
            transition: transform 0.3s;
        }
        .avatar-selection img:hover {
            transform: scale(1.1);
        }
        .selected-avatar {
            border: 3px solid #ff4081;
        }
    </style>
</head>
<body>

    <div class="form-container active" id="form-container">
        <h1>🌸 Identifique-se 🌸</h1>
        <form id="identification-form">
            <input type="text" id="nome" placeholder="Nome" required>
            <input type="text" id="cidade" placeholder="Cidade de Origem" required>
            <input type="date" id="data-jogo" placeholder="Data do Jogo" required>
            <div class="avatar-selection">
                <img src="https://example.com/avatar1.jpg" alt="Avatar 1" onclick="selecionarAvatar(this)">
                <img src="https://example.com/avatar2.jpg" alt="Avatar 2" onclick="selecionarAvatar(this)">
                <img src="https://example.com/avatar3.jpg" alt="Avatar 3" onclick="selecionarAvatar(this)">
            </div>
            <button type="submit">Iniciar</button>
        </form>
    </div>

    <div class="inicio-container" id="inicio-container">
        <h1>Bem-vindo(a), <span id="user-nome"></span>!</h1>
        <p>Origem: <span id="user-cidade"></span></p>
        <p>Data do Jogo: <span id="user-data"></span></p>
        <img id="user-avatar" src="" alt="Avatar Selecionado">
        <button onclick="mostrarPista()">Começar o Jogo</button>
    </div>

    <div class="pista-container" id="pista-container">
        <h2 id="pista">🌊 Local da Primeira Pista</h2>
        <p id="mensagem">Descubra a pista nas belezas naturais de Florianópolis!</p>
        <button onclick="desbloquearProximaPista()">Próxima Pista</button>
        <div id="mapa"></div>
    </div>

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

        function selecionarAvatar(img) {
            avatarSelecionado = img.src;
            document.querySelectorAll('.avatar-selection img').forEach(el => el.classList.remove('selected-avatar'));
            img.classList.add('selected-avatar');
            document.getElementById('user-avatar').src = avatarSelecionado;
        }

        function mostrarPista() {
            pistas = embaralharPistas("chave-fixa-teste");

            document.getElementById('inicio-container').classList.remove('active');
            document.getElementById('pista-container').classList.add('active');
            document.getElementById("musicaFundo").play();
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

        function mostrarMapa(lat, long) {
            document.getElementById("mapa").innerHTML = `<iframe width="100%" height="300" frameborder="0"
                src="https://www.google.com/maps?q=${lat},${long}&output=embed"></iframe>`;
        }

        function desbloquearProximaPista() {
            const somCorreto = document.getElementById("somCorreto");
            somCorreto.play();
            document.getElementById("mensagem").textContent = `Parabéns! Você encontrou a próxima pista!`;
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
    </script>
</body>
</html>
