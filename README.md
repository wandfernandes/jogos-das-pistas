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

    <div class="roleta-container" id="roleta-container" style="display:none;">
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
    flex-direction: column;
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

.pista-container, .roleta-container {
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

.form-container.active, .inicio-container.active, .pista-container.active, .roleta-container.active {
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

/* Estilos da roleta */
.roleta {
    width: 300px;
    height: 300px;
    border-radius: 50%;
    border: 10px solid #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    position: relative;
    margin: 20px auto;
}

.roleta-segmento {
    width: 50%;
    height: 50%;
    background: conic-gradient(#ff4081 0% 25%, #e91e63 25% 50%, #6200ea 50% 75%, #3f51b5 75% 100%);
    position: absolute;
    top: 0;
    left: 0;
    clip-path: polygon(0 0, 100% 0, 50% 50%);
    transform-origin: 100% 100%;
}

.roleta-segmento:nth-child(2) {
    transform: rotate(90deg);
}

.roleta-segmento:nth-child(3) {
    transform: rotate(180deg);
}

.roleta-segmento:nth-child(4) {
    transform: rotate(270deg);
}

.roleta-agulha {
    width: 0;
    height: 0;
    border-left: 20px solid transparent;
    border-right: 20px solid transparent;
    border-bottom: 40px solid #fff;
    position: absolute;
    top: -40px;
    left: 50%;
    transform: translateX(-50%);
}
const pistasOriginais = [
    { charada: "🌆 Noite: Passeio leve na Beira-Mar Norte ou na Lagoa da Conceição. Onde estou?", latitude: -27.5969, longitude: -48.4846, nome: "Lagoa da Conceição", curiosidade: "A Lagoa da Conceição é um dos cartões-postais mais conhecidos de Florianópolis, famosa por suas águas calmas e esportes aquáticos.", foto: "https://example.com/lagoa.jpg" },
    { charada: "🏖️ Manhã: Praia de Jurerê Internacional e Jurerê Tradicional. Onde estou?", latitude: -27.4368, longitude: -48.4916, nome: "Praia de Jurerê", curiosidade: "Jurerê Internacional é conhecida por suas festas luxuosas, casas noturnas e um público seleto. Um lugar onde luxo e natureza se encontram.", foto: "https://example.com/jurere.jpg" },
    { charada: "🏰 Tarde: Praia do Forte e visita à Fortaleza de São José da Ponta Grossa. Onde estou?", latitude: -27.4284, longitude: -48.5005, nome: "Praia do Forte", curiosidade: "A Fortaleza de São José da Ponta Grossa é uma fortificação histórica construída no século XVIII para proteger a ilha de invasões.", foto: "https://example.com/forte.jpg" },
    { charada: "🍽️ Noite: Jantar em Santo Antônio de Lisboa. Onde estou?", latitude: -27.5063, longitude: -48.5496, nome: "Santo Antônio de Lisboa", curiosidade: "Santo Antônio de Lisboa é um bairro histórico com forte influência da cultura açoriana, famoso por seus restaurantes de frutos do mar.", foto: "https://example.com/santo_antonio.jpg" },
    { charada: "🏄‍♂️ Manhã: Praia da Joaquina e Dunas. Onde estou?", latitude: -27.6206, longitude: -48.4354, nome: "Praia da Joaquina", curiosidade: "As Dunas da Joaquina são perfeitas para a prática de sandboard, um esporte que consiste em descer as dunas em uma prancha.", foto: "https://example.com/joaquina.jpg" },
    { charada: "🌊 Tarde: Praia Mole e trilha para Galheta. Onde estou?", latitude: -27.6048, longitude: -48.4223, nome: "Praia Mole", curiosidade: "A Praia Mole é famosa por suas ondas fortes, sendo um ponto de encontro para surfistas e amantes da natureza.", foto: "https://example.com/mole.jpg" },
    { charada: "🛍️ Noite: Centro Histórico e Mercado Público. Onde estou?", latitude: -27.5951, longitude: -48.5480, nome: "Mercado Público", curiosidade: "O Mercado Público de Florianópolis é um ponto tradicional para comprar frutos do mar frescos e aproveitar a culinária local.", foto: "https://example.com/mercado.jpg" },
    { charada: "🎡 Passeio a Balneário Camboriú: Parque Unipraias, Praia Central e Roda Gigante FG Big Wheel. Onde estou?", latitude: -26.9911, longitude: -48.6337, nome: "Balneário Camboriú", curiosidade: "Balneário Camboriú é conhecida como a 'Dubai Brasileira' devido aos seus arranha-céus e atrações turísticas modernas.", foto: "https://example.com/camboriu.jpg" },
    { charada: "🏝️ Manhã: Ilha do Campeche. Onde estou?", latitude: -27.6878, longitude: -48.4827, nome: "Ilha do Campeche", curiosidade: "A Ilha do Campeche é conhecida por suas águas cristalinas e trilhas arqueológicas, sendo um verdadeiro paraíso natural.", foto: "https://example.com/campeche.jpg" },
    { charada: "🏖️ Tarde: Praia da Armação e Matadeiro. Onde estou?", latitude: -27.7252, longitude: -48.5007, nome: "Praia da Armação", curiosidade: "A Praia da Armação é conhecida por sua beleza natural e tranquilidade, sendo um ótimo local para relaxar e aproveitar a natureza.", foto: "https://example.com/armacao.jpg" },
    { charada: "🌅 Noite: Relaxamento ou passeio livre. Onde estou?", latitude: -27.5969, longitude: -48.4846, nome: "Lagoa da Conceição", curiosidade: "A Lagoa da Conceição é um dos cartões-postais mais conhecidos de Florianópolis, famosa por suas águas calmas e esportes aquáticos.", foto: "https://example.com/lagoa.jpg" },
    { charada: "🏞️ Manhã: Trilha para Lagoinha do Leste. Onde estou?", latitude: -27.7516, longitude: -48.4877, nome: "Lagoinha do Leste", curiosidade: "A Lagoinha do Leste é uma das praias mais selvagens e bonitas de Florianópolis, acessível apenas por trilhas ou barco.", foto: "https://example.com/lagoinha.jpg" },
    { charada: "🏖️ Tarde: Praia da Solidão e Saquinho. Onde estou?", latitude: -27.7615, longitude: -48.5106, nome: "Praia da Solidão", curiosidade: "A Praia da Solidão é um refúgio tranquilo e paradisíaco, ideal para quem busca sossego e contato com a natureza.", foto: "https://example.com/solidao.jpg" },
    { charada: "🌆 Noite: Passeio de despedida na Lagoa da Conceição. Onde estou?", latitude: -27.5969, longitude: -48.4846, nome: "Lagoa da Conceição", curiosidade: "A Lagoa da Conceição é um dos cartões-postais mais conhecidos de Florianópolis, famosa por suas águas calmas e esportes aquáticos.", foto: "https://example.com/lagoa.jpg" },
    { charada: "🛍️ Manhã: Compras ou passeio leve em praias próximas. Onde estou?", latitude: -27.5951, longitude: -48.5480, nome: "Mercado Público", curiosidade: "O Mercado Público de Florianópolis é um ponto tradicional para comprar frutos do mar frescos e aproveitar a culinária local.", foto: "https://example.com/mercado.jpg" },
    { charada: "✈️ Noite: Saída para Goiânia. Onde estou?", latitude: -27.6706, longitude: -48.5525, nome: "Aeroporto Internacional Hercílio Luz", curiosidade: "O Aeroporto Internacional Hercílio Luz é o principal aeroporto de Florianópolis, conectando a cidade a diversos destinos nacionais e internacionais.", foto: "https://example.com/aeroporto.jpg" }
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
    document.getElementBy
