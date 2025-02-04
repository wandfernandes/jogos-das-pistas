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
            margin: 0;
            padding: 0;
            background: url('https://drive.google.com/uc?id=1ucn22uaFBDVc2sIo-HpdSvqhU_lK0oT-') no-repeat center center fixed;
            background-size: cover;
            color: #5d5151;
            overflow: hidden;
        }
        #brasao {
            width: 100px;
            margin: 20px auto;
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
            display: block;
            margin: 10px auto;
            width: 80%;
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
    </style>
</head>
<body>
    <img id="brasao" src="https://drive.google.com/uc?id=1JeGOidonTIDj0Z0vuEC-jjRM6CAoBpBX" alt="Brasão UniGOIAS">
    <h1>Bem-vinda à Caça ao Tesouro Romântica!</h1>
    <p>Resolva as charadas para encontrar o presente final. Escolha a resposta correta para receber a localização do QR Code com a próxima pista. <span class="heart">&#x2764;</span></p>
    
    <div id="pista-container">
        <h2 id="pista"></h2>
        <div id="opcoes"></div>
        <p id="mensagem"></p>
        <div id="mapa"></div>
    </div>
    
    <button onclick="iniciarJogo()">Iniciar Jogo</button>

    <!-- Leitor de QR Code -->
    <div id="qr-reader" style="width: 500px; margin: 20px auto;"></div>
    <p id="qr-reader-results"></p>
    
    <script src="https://unpkg.com/html5-qrcode/minified/html5-qrcode.min.js"></script>
    <script>
        function iniciarJogo() {
            document.getElementById("pista-container").style.display = "block";
            criarCorações();
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
        function onScanSuccess(decodedText, decodedResult) {
            document.getElementById('qr-reader-results').innerHTML = `Código QR Lido: ${decodedText}`;
        }
        var html5QrcodeScanner = new Html5QrcodeScanner(
            "qr-reader", { fps: 10, qrbox: 250 });
        html5QrcodeScanner.render(onScanSuccess);
    </script>
</body>
</html>
