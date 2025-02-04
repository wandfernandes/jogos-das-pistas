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
        #form-container, #inicio-container {
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
        #form-container.active, #inicio-container.active {
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
        <img src=(https://drive.google.com/file/d/1yG53CU4XX6-Aap6FJBHl_PLKvccgXg0j/view?usp=drive_link) alt="Imagem Enigmática">
        <button onclick="iniciarJogo()">Começar o Jogo</button>
    </div>

    <script>
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

        function iniciarJogo() {
            alert("O jogo vai começar! Boa sorte!");
        }
    </script>
</body>
</html>
