<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lembranças para Recordar - Florianópolis</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="avatar"></div>

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
        <img id="foto-local" src="" alt="Foto do Local">
        <button onclick="desbloquearProximaPista()">Próxima Pista</button>
        <div id="mapa"></div>
        <p id="curiosidade"></p>
    </div>

    <div class="roleta-container" id="roleta-container">
        <h2>🎉 Parabéns! 🎉</h2>
        <p>Gire a roleta para ganhar um prêmio!</p>
        <div class="roleta">
            <div class="roleta-segmento"></div>
            <div class="roleta-segmento"></div>
            <div class="roleta-segmento"></div>
            <div class="roleta-segmento"></div>
            <div class="roleta-agulha"></div>
        </div>
        <button onclick="girarRoleta()">Girar Roleta</button>
        <p id="resultado-roleta"></p>
    </div>

    <audio id="somCorreto" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="somIncorreto" src="https://www.soundjay.com/button/beep-10.wav"></audio>
    <audio id="musicaFundo" src="https://www.soundjay.com/nature/sounds/rain-01.mp3" loop></audio>

    <script src="script.js"></script>
</body>
</html>
