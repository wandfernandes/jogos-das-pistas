<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caça ao Tesouro Romântica - Florianópolis</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background: linear-gradient(120deg, #ff758c, #ff7eb3);
            color: #fff;
            overflow-x: hidden;
        }
        h1 {
            margin-top: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        #avatar-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 20px 0;
        }
        .avatar {
            width: 100px;
            cursor: pointer;
            transition: transform 0.3s;
        }
        .avatar:hover {
            transform: scale(1.1);
        }
        #pista-container {
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
        button {
            padding: 15px 20px;
            cursor: pointer;
            border: none;
            background-color: #ff4081;
            color: #ffffff;
            border-radius: 8px;
            font-size: 18px;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #e91e63;
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
    </style>
</head>
<body>

    <h1>🌸 Caça ao Tesouro Romântica - Florianópolis 🌸</h1>
    <p>Escolha seu avatar para começar:</p>
    
    <div id="avatar-container">
        <img src="https://cdn-icons-png.flaticon.com/512/2829/2829816.png" class="avatar" onclick="iniciarJogo('avatar1')">
        <img src="https://cdn-icons-png.flaticon.com/512/2829/2829818.png" class="avatar" onclick="iniciarJogo('avatar2')">
        <img src="https://cdn-icons-png.flaticon.com/512/2829/2829819.png" class="avatar" onclick="iniciarJogo('avatar3')">
    </div>

    <div id="pista-container">
        <h2 id="pista"></h2>
        <p id="mensagem"></p>
        <button onclick="verificarLocalizacao()">Verificar Localização</button>
        <div id="mapa"></div>
    </div>

    <script>
        const pistas = [
            { charada: "🌊 Um espelho d’água cercado por dunas e natureza. Casais adoram remar aqui. Onde estou?", 
              latitude: -27.5969, longitude: -48.4846, nome: "Lagoa da Conceição" },

            { charada: "🌉 Uma ponte que une passado e presente, iluminando noites românticas. Onde estou?", 
              latitude: -27.5973, longitude: -48.5515, nome: "Ponte Hercílio Luz" },

            { charada: "🏄‍♂️ Dunas douradas onde aventureiros deslizam ao vento. Um encontro perfeito. Onde estou?", 
              latitude: -27.6206, longitude: -48.4354, nome: "Dunas da Joaquina" },

            { charada: "🏖️ Um paraíso de luxo e diversão onde o pôr do sol é digno de aplausos. Onde estou?", 
              latitude: -27.4368, longitude: -48.4916, nome: "Praia de Jurerê" },

            { charada: "🍽️ Frutos do mar, cultura e encontros românticos entre as mesas. Onde estou?", 
              latitude: -27.5951, longitude: -48.5480, nome: "Mercado Público" },

            { charada: "🌅 No alto da ilha, uma vista que revela toda a beleza de Floripa. Onde estou?", 
              latitude: -27.5888, longitude: -48.5350, nome: "Mirante do Morro da Cruz" }
        ];

        let indiceAtual = 0;

        function iniciarJogo(avatar) {
            document.getElementById("avatar-container").style.display = "none";
            document.getElementById("pista-container").style.display = "block";
            mostrarPista();
        }

        function mostrarPista() {
            document.getElementById("pista").textContent = pistas[indiceAtual].charada;
            document.getElementById("mensagem").textContent = Vá até ${pistas[indiceAtual].nome} e clique no botão abaixo!;
            mostrarMapa(pistas[indiceAtual].latitude, pistas[indiceAtual].longitude);
        }

        function verificarLocalizacao() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition((posicao) => {
                    const latUsuario = posicao.coords.latitude;
                    const longUsuario = posicao.coords.longitude;
                    const latPista = pistas[indiceAtual].latitude;
                    const longPista = pistas[indiceAtual].longitude;

                    const distancia = calcularDistancia(latUsuario, longUsuario, latPista, longPista);

                    if (distancia < 0.2) {
                        indiceAtual++;
                        if (indiceAtual < pistas.length) {
                            mostrarPista();
                        } else {
                            document.getElementById("pista").textContent = "🎉 Parabéns! Você encontrou o tesouro romântico! 🎁";
                            document.getElementById("mensagem").textContent = "";
                        }
                    } else {
                        document.getElementById("mensagem").textContent = "📍 Você ainda não chegou ao local certo! Continue procurando!";
                    }
                });
            } else {
                alert("Geolocalização não suportada no seu navegador.");
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
    </script>
</body>
</html>
