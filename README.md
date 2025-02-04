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
            color: #fff;
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

    <!-- Adicionando o leitor de QR Code -->
    <div id="qr-reader" style="width: 500px; margin: 20px auto;"></div>
    <p id="qr-reader-results"></p>

    <!-- Adicionando sons -->
    <audio id="somCorreto" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="somIncorreto" src="https://www.soundjay.com/button/beep-10.wav"></audio>

    <!-- Adicionando o assistente virtual -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/annyang/2.6.1/annyang.min.js"></script>
    
    <script src="https://unpkg.com/html5-qrcode/minified/html5-qrcode.min.js"></script>
    <script>
        const pistas = [
            {
                charada: "Ele foi o homem que sonhou e construiu. No coração da cidade, sua presença ainda se faz sentir. Seu olhar repousa sobre aqueles que passam. Onde ele está?",
                opcoes: ["Praça Cívica", "Parque Flamboyant", "Bosque dos Buritis"],
                resposta: "Praça Cívica",
                proximaPista: "Parabéns! Vá até a Praça Cívica e procure um QR Code próximo à estátua de Pedro Ludovico para a próxima charada!",
                qrCode: "https://www.exemplo.com/qr1.png"
            },
            {
                charada: "Um espaço de lazer no coração da cidade, onde vacas pastavam antigamente. Agora, é um lugar para caminhar, correr e relaxar junto ao lago. Onde estou?",
                opcoes: ["Parque Areião", "Parque Vaca Brava", "Parque Flamboyant"],
                resposta: "Parque Vaca Brava",
                proximaPista: "Muito bem! Vá até o Parque Vaca Brava e encontre um QR Code próximo ao lago!",
                qrCode: "https://www.exemplo.com/qr2.png"
            },
            {
                charada: "Um mercado histórico onde se encontram sabores e aromas regionais. De frutas frescas a artesanatos, este lugar é um verdadeiro tesouro. Onde estou?",
                opcoes: ["Mercado Municipal", "Mercado Central", "Mercado da 74"],
                resposta: "Mercado Central",
                proximaPista: "Ótimo! Vá até o Mercado Central e procure um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr3.png"
            },
            {
                charada: "Um parque onde a natureza e os animais convivem em harmonia. Caminhe pelas trilhas e observe os saguis brincando nas árvores. Onde estou?",
                opcoes: ["Parque Areião", "Parque Flamboyant", "Bosque dos Buritis"],
                resposta: "Parque Areião",
                proximaPista: "Você acertou! Vá até o Parque Areião e encontre um QR Code próximo ao playground!",
                qrCode: "https://www.exemplo.com/qr4.png"
            },
            {
                charada: "Um teatro que é um marco da arquitetura art déco na cidade. Aqui, a cultura se encontra com a história em cada apresentação. Onde estou?",
                opcoes: ["Teatro Goiânia", "Teatro Basileu França", "Centro Cultural UFG"],
                resposta: "Teatro Goiânia",
                proximaPista: "Muito bom! Vá até o Teatro Goiânia e encontre um QR Code próximo à bilheteria!",
                qrCode: "https://www.exemplo.com/qr5.png"
            },
            // Adicione mais 15 charadas baseadas na história de Goiânia
            {
                charada: "Este parque é conhecido por ser um local de eventos e tem um lago muito visitado. Onde ele está?",
                opcoes: ["Parque Flamboyant", "Jardim Botânico", "Parque Mutirama"],
                resposta: "Parque Flamboyant",
                proximaPista: "Ótimo! Vá até o Parque Flamboyant e encontre um QR Code próximo ao lago!",
                qrCode: "https://www.exemplo.com/qr6.png"
            },
            {
                charada: "Este monumento homenageia os pioneiros que fundaram a cidade de Goiânia. Onde ele está?",
                opcoes: ["Monumento dos Três Marcos", "Monumento às Nações Indígenas", "Monumento à Paz Mundial"],
                resposta: "Monumento dos Três Marcos",
                proximaPista: "Muito bem! Vá até o Monumento dos Três Marcos e encontre um QR Code próximo ao monumento!",
                qrCode: "https://www.exemplo.com/qr7.png"
            },
            {
                charada: "Este estádio é a casa do Goiás Esporte Clube. Onde ele está?",
                opcoes: ["Estádio Serra Dourada", "Estádio Olímpico Pedro Ludovico", "Estádio Hailé Pinheiro"],
                resposta: "Estádio Hailé Pinheiro",
                proximaPista: "Ótimo! Vá até o Estádio Hailé Pinheiro e procure um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr8.png"
            },
            {
                charada: "Este museu abriga a história de Goiás e fica no centro da cidade. Onde ele está?",
                opcoes: ["Museu Pedro Ludovico", "Museu de Arte de Goiânia", "Museu Antropológico"],
                resposta: "Museu Pedro Ludovico",
                proximaPista: "Muito bem! Vá até o Museu Pedro Ludovico e encontre um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr9.png"
            },
            {
                charada: "Este centro cultural é conhecido por suas exposições de arte contemporânea. Onde ele está?",
                opcoes: ["Centro Cultural Oscar Niemeyer", "Centro Cultural UFG", "Centro Cultural Martim Cererê"],
                resposta: "Centro Cultural Oscar Niemeyer",
                proximaPista: "Ótimo! Vá até o Centro Cultural Oscar Niemeyer e encontre um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr10.png"
            },
            {
                charada: "Este parque é conhecido por suas trilhas e pela presença de animais. Onde ele está?",
                opcoes: ["Parque Areião", "Parque Flamboyant", "Bosque dos Buritis"],
                resposta: "Parque Areião",
                proximaPista: "Você acertou! Vá até o Parque Areião e encontre um QR Code próximo ao playground!",
                qrCode: "https://www.exemplo.com/qr11.png"
            },
            {
                charada: "Este monumento é uma das principais atrações turísticas de Goiânia e fica na Praça Cívica. Onde ele está?",
                opcoes: ["Monumento às Três Raças", "Monumento à Paz Mundial", "Monumento ao Bandeirante"],
                resposta: "Monumento às Três Raças",
                proximaPista: "Muito bem! Vá até o Monumento às Três Raças e encontre um QR Code próximo ao monumento!",
                qrCode: "https://www.exemplo.com/qr12.png"
            },
            {
                charada: "Este parque tem um grande lago e é um ótimo lugar para passeios de barco. Onde ele está?",
                opcoes: ["Parque Flamboyant", "Parque Vaca Brava", "Parque Areião"],
                resposta: "Parque Flamboyant",
                proximaPista: "Ótimo! Vá até o Parque Flamboyant e encontre um QR Code próximo ao lago!",
                qrCode: "https://www.exemplo.com/qr13.png"
            },
            {
                charada: "Este mercado é famoso por suas frutas frescas e artesanatos. Onde ele está?",
                opcoes: ["Mercado Central", "Mercado Municipal", "Mercado da 74"],
                resposta: "Mercado Central",
                proximaPista: "Ótimo! Vá até o Mercado Central e procure um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr14.png"
            },
            {
                charada: "Este espaço cultural é conhecido por suas apresentações teatrais e musicais. Onde ele está?",
                opcoes: ["Teatro Goiânia", "Centro Cultural UFG", "Teatro Basileu França"],
                resposta: "Teatro Goiânia",
                proximaPista: "Muito bom! Vá até o Teatro Goiânia e encontre um QR Code próximo à bilheteria!",
                qrCode: "https://www.exemplo.com/qr15.png"
            },
            {
                charada: "Este parque é conhecido por suas esculturas e jardins bem cuidados. Onde ele está?",
                opcoes: ["Bosque dos Buritis", "Parque Areião", "Parque Flamboyant"],
                resposta: "Bosque dos Buritis",
                proximaPista: "Ótimo! Vá até o Bosque dos Buritis e encontre um QR Code próximo às esculturas!",
                qrCode: "https://www.exemplo.com/qr16.png"
            },
            {
                charada: "Este monumento foi construído em homenagem ao fundador de Goiânia. Onde ele está?",
                opcoes: ["Monumento a Pedro Ludovico", "Monumento às Três Raças", "Monumento ao Bandeirante"],
                resposta: "Monumento a Pedro Ludovico",
                proximaPista: "Muito bem! Vá até o Monumento a Pedro Ludovico e encontre um QR Code próximo ao monumento!",
                qrCode: "https://www.exemplo.com/qr17.png"
            },
            {
                charada: "Este parque é um dos maiores de Goiânia e é conhecido por sua natureza preservada. Onde ele está?",
                opcoes: ["Parque Areião", "Parque Flamboyant", "Jardim Botânico"],
                resposta: "Jardim Botânico",
                proximaPista: "Ótimo! Vá até o Jardim Botânico e encontre um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr18.png"
            },
            {
                charada: "Este museu é dedicado à arte contemporânea e está localizado no centro de Goiânia. Onde ele está?",
                opcoes: ["Museu de Arte de Goiânia", "Museu Pedro Ludovico", "Museu Antropológico"],
                resposta: "Museu de Arte de Goiânia",
                proximaPista: "Muito bem! Vá até o Museu de Arte de Goiânia e encontre um QR Code próximo à entrada principal!",
                qrCode: "https://www.exemplo.com/qr19.png"
            },
            {
                charada: "Este parque é ideal para quem gosta de caminhadas e corridas. Onde ele está?",
                opcoes: ["Parque Areião", "Parque Vaca Brava", "Parque Flamboyant"],
                resposta: "Parque Vaca Brava",
                proximaPista: "Ótimo! Vá até o Parque Vaca Brava e encontre um QR Code próximo ao lago!",
                qrCode: "https://www.exemplo.com/qr20.png"
            }
        ];
        
        let indiceAtual = 0;
        
        function iniciarJogo() {
            document.getElementById("pista-container").style.display = "block";
            mostrarCharada(indiceAtual);
            criarCorações();
        }
        
        function mostrarCharada(indice) {
            const pista = pistas[indice];
            document.getElementById("pista").textContent = pista.charada;
            const opcoesContainer = document.getElementById("opcoes");
            opcoesContainer.innerHTML = "";
            pista.opcoes.forEach((opcao, index) => {
                const botao = document.createElement("button");
                botao.textContent = opcao;
                botao.onclick = () => verificarResposta(opcao);
                opcoesContainer.appendChild(botao);
            });
        }
        
        function verificarResposta(resposta) {
            const somCorreto = document.getElementById("somCorreto");
            const somIncorreto = document.getElementById("somIncorreto");
            if (resposta === pistas[indiceAtual].resposta) {
                somCorreto.play();
                document.getElementById("mensagem").textContent = pistas[indiceAtual].proximaPista;
                const qrCodeImage = document.createElement('img');
                qrCodeImage.src = pistas[indiceAtual].qrCode;
                qrCodeImage.alt = "QR Code";
                qrCodeImage.style.maxWidth = "200px";
                document.getElementById("mensagem").appendChild(qrCodeImage);
                mostrarMapa(pistas[indiceAtual].proximaPista);
                indiceAtual++;
                if (indiceAtual < pistas.length) {
                    setTimeout(() => {
                        mostrarCharada(indiceAtual);
                        document.getElementById("mensagem").textContent = "";
                    }, 3000);
                } else {
                    document.getElementById("mensagem").textContent = "Parabéns! Você completou a caça ao tesouro e encontrou seu presente!";
                }
            } else {
                somIncorreto.play();
                document.getElementById("mensagem").textContent = "Resposta incorreta. Tente novamente!";
            }
        }

        function mostrarMapa(local) {
            let mapa = document.getElementById("mapa");
            mapa.innerHTML = `<iframe width="600" height="450" style="border:0" loading="lazy" allowfullscreen
                src="https://www.google.com/maps/embed/v1/place?key=YOUR_API_KEY&q=${encodeURIComponent(local)}"></iframe>`;
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
