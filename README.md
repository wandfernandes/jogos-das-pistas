<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caça ao Tesouro Romântica</title>
    <style>
        body {
            font-family: 'Courier New', cursive;
            text-align: center;
            margin: 50px;
            background: linear-gradient(to right top, #ff9a9e, #fad0c4);
            color: #5d5151;
        }
        #pista-container {
            display: none;
            margin-top: 20px;
            padding: 20px;
            background-color: #ffffff;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            animation: fadeIn 1s;
        }
        input {
            padding: 15px;
            width: 300px;
            margin-top: 10px;
            border: 2px solid #fad0c4;
            border-radius: 8px;
            font-size: 16px;
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
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #ff4e67;
        }
        #mensagem {
            margin-top: 20px;
            font-weight: bold;
            color: #333333;
        }
        #mapa {
            margin-top: 20px;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        .heart {
            color: #ff6f91;
            animation: pulse 1.5s infinite;
        }
    </style>
</head>
<body>
    <h1>Bem-vinda à Caça ao Tesouro Romântica!</h1>
    <p>Resolva as charadas para encontrar o presente final. Insira a resposta correta para receber a localização do QR Code com a próxima pista. <span class="heart">&#x2764;</span></p>
    
    <div id="pista-container">
        <h2 id="pista"></h2>
        <input type="text" id="resposta" placeholder="Digite sua resposta">
        <button onclick="verificarResposta()">Enviar</button>
        <p id="mensagem"></p>
        <div id="mapa"></div>
    </div>
    
    <button onclick="iniciarJogo()">Iniciar Jogo</button>

    <!-- Adicionando o leitor de QR Code -->
    <div id="qr-reader" style="width: 500px; margin: 20px auto;"></div>
    <p id="qr-reader-results"></p>
    
    <script src="https://unpkg.com/html5-qrcode/minified/html5-qrcode.min.js"></script>
    <script>
        const pistas = [
            {
                charada: "Ele foi o homem que sonhou e construiu. No coração da cidade, sua presença ainda se faz sentir. Seu olhar repousa sobre aqueles que passam. Onde ele está?",
                resposta: "praça cívica",
                proximaPista: "Parabéns! Vá até a Praça Cívica e procure um QR Code próximo à estátua de Pedro Ludovico para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr1.png"
            },
            {
                charada: "Um espaço de lazer no coração da cidade, onde vacas pastavam antigamente. Agora, é um lugar para caminhar, correr e relaxar junto ao lago. Onde estou?",
                resposta: "parque vaca brava",
                proximaPista: "Muito bem! Vá até o Parque Vaca Brava e encontre um QR Code próximo ao lago!",
                qrCode: "https://www.exemplo.com/qr2.png"
            },
            {
                charada: "Um mercado histórico onde se encontram sabores e aromas regionais. De frutas frescas a artesanatos, este lugar é um verdadeiro tesouro. Onde estou?",
                resposta: "mercado central",
                proximaPista: "Ótimo! Vá até o Mercado Central e procure um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr3.png"
            },
            {
                charada: "Um parque onde a natureza e os animais convivem em harmonia. Caminhe pelas trilhas e observe os saguis brincando nas árvores. Onde estou?",
                resposta: "parque areião",
                proximaPista: "Você acertou! Vá até o Parque Areião e encontre um QR Code próximo ao playground!",
                qrCode: "https://www.exemplo.com/qr4.png"
            },
            {
                charada: "Um teatro que é um marco da arquitetura art déco na cidade. Aqui, a cultura se encontra com a história em cada apresentação. Onde estou?",
                resposta: "teatro goiânia",
                proximaPista: "Muito bom! Vá até o Teatro Goiânia e encontre um QR Code próximo à bilheteria!",
                qrCode: "https://www.exemplo.com/qr5.png"
            }
        ];
        
        let indiceAtual = 0;
        
        function iniciarJogo() {
            document.getElementById("pista-container").style.display = "block";
            document.getElementById("pista").textContent = pistas[indiceAtual].charada;
        }
        
        function verificarResposta() {
            let resposta = document.getElementById("resposta").value.toLowerCase().trim();
            if (resposta === pistas[indiceAtual].resposta) {
                document.getElementById("mensagem").textContent = pistas[indiceAtual].proximaPista;
                document.getElementById("mensagem").innerHTML += `<br><img src="${pistas[indiceAtual].qrCode}" alt="QR Code" style="max-width: 200px;">`;
                mostrarMapa(pistas[indiceAtual].proximaPista);
                indiceAtual++;
                if (indiceAtual < pistas.length) {
                    setTimeout(() => {
                        document.getElementById("pista").textContent = pistas[indiceAtual].charada;
                        document.getElementById("mensagem").textContent = "";
                        document.getElementById("resposta").value = "";
                    }, 3000);
                } else {
                    document.getElementById("mensagem").textContent = "Parabéns! Você completou a caça ao tesouro e encontrou seu presente!";
                }
            } else {
                document.getElementById("mensagem").textContent = "Resposta incorreta. Tente novamente!";
            }
        }

        function mostrarMapa(local) {
            let mapa = document.getElementById("mapa");
            mapa.innerHTML = `<iframe width="600" height="450" style="border:0" loading="lazy" allowfullscreen
                src="https://www.google.com/maps/embed/v1/place?key=YOUR_API_KEY&q=${encodeURIComponent(local)}"></iframe>`;
        }

        // Leitor de QR Code
        function onScanSuccess(decodedText, decodedResult) {
            document.getElementById('qr-reader-results').innerHTML = `Código QR Lido: ${decodedText}`;
        }

        var html5QrcodeScanner = new Html5QrcodeScanner(
            "qr-reader", { fps: 10, qrbox: 250 });
        html5QrcodeScanner.render(onScanSuccess);
    </script>
</body>
</html>
