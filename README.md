<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roleta do Tesouro Romântico</title>
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #ff4081;
            color: white;
            text-align: center;
            padding-top: 50px;
        }
        .roleta {
            width: 300px;
            height: 300px;
            border-radius: 50%;
            border: 10px solid #fff;
            margin: 0 auto;
            position: relative;
            background: conic-gradient(#ff4081 0% 16%, #6200ea 16% 32%, #e91e63 32% 48%, #ff5722 48% 64%, #4caf50 64% 80%, #ffeb3b 80% 100%);
        }
        .seta {
            width: 0;
            height: 0;
            border-left: 30px solid transparent;
            border-right: 30px solid transparent;
            border-bottom: 30px solid white;
            position: absolute;
            top: -30px;
            left: 50%;
            transform: translateX(-50%);
        }
        button {
            margin-top: 20px;
            padding: 15px 30px;
            font-size: 1.2rem;
            background: #fff;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            color: #ff4081;
            font-weight: bold;
        }
        button:hover {
            background: #e91e63;
            color: white;
        }
        .resultado {
            font-size: 1.5rem;
            margin-top: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <h1>🎉 Roleta do Tesouro Romântico 🎉</h1>
    <p>Você encontrou o tesouro! Agora, vamos descobrir o que você ganhou.</p>

    <div class="roleta">
        <div class="seta"></div>
    </div>

    <button onclick="girarRoleta()">Girar a Roleta</button>
    <div class="resultado" id="resultado"></div>

    <script>
        const premios = [
            "Pulseira de Prata Personalizada",
            "Perfume Exclusivo",
            "Jantar Romântico",
            "Viagem a Dois",
            "Vale para Compras de Roupa",
            "Experiência Exclusiva (Passeio de Barco)"
        ];

        function girarRoleta() {
            const roleta = document.querySelector('.roleta');
            const resultado = document.getElementById('resultado');

            // Gira a roleta
            const anguloAleatorio = Math.floor(Math.random() * 360) + 1800; // Pelo menos 5 voltas
            roleta.style.transition = 'transform 4s ease-out';
            roleta.style.transform = `rotate(${anguloAleatorio}deg)`;

            // Após o giro, exibe o prêmio
            setTimeout(() => {
                const premioIndex = Math.floor((anguloAleatorio % 360) / 60); // Dividido em 6 partes de 60 graus
                resultado.textContent = `Você ganhou: ${premios[premioIndex]}`;
            }, 4000); // Espera o tempo do giro terminar
        }
    </script>

</body>
</html>
