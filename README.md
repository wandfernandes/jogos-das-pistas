<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caça ao Tesouro Romântica - Florianópolis</title>
    <style>
        /* Template de design vibrante e moderno */
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(45deg, #ff4081, #6200ea);
            color: #fff;
            margin: 0;
            overflow: hidden;
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
            background: url('avatar.jpg') no-repeat center;
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
    </style>
</head>
<body>

    <div class="avatar"></div>

    <div id="inicio">
        <h1>🌸 Caça ao Tesouro Romântica 🌸</h1>
        <p>Descubra as belezas de Florianópolis e desvende os mistérios do amor!</p>
        <input type="text" id="chave" placeholder="Digite sua chave secreta">
        <button onclick="iniciarJogo()">Iniciar Jogo</button>
    </div>

    <div class="pista-container" id="pista-container">
        <h2 id="pista">🌊 Local da Primeira Pista</h2>
        <p id="mensagem">Descubra a pista nas belezas naturais de Florianópolis!</p>
        <button onclick="verificarLocalizacao()">Verificar Localização</button>
        <div id="mapa"></div>
    </div>

    <script>
        // Funções para o jogo interativo...
    </script>
</body>
</html>
