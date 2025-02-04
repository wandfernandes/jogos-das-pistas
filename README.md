const pistasOriginais = [
    { charada: "🌊 Um espelho d’água cercado por dunas e natureza. Casais adoram remar aqui. Onde estou?", latitude: -27.5969, longitude: -48.4846, nome: "Lagoa da Conceição", curiosidade: "A Lagoa da Conceição é um dos cartões-postais mais conhecidos de Florianópolis, famosa por suas águas calmas e esportes aquáticos." },
    { charada: "🌉 Uma ponte que une passado e presente, iluminando noites românticas. Onde estou?", latitude: -27.5973, longitude: -48.5515, nome: "Ponte Hercílio Luz", curiosidade: "A Ponte Hercílio Luz é a maior ponte pênsil do Brasil e foi inaugurada em 1926. É um símbolo de Florianópolis." },
    { charada: "🏄‍♂️ Dunas douradas onde aventureiros deslizam ao vento. Um encontro perfeito. Onde estou?", latitude: -27.6206, longitude: -48.4354, nome: "Dunas da Joaquina", curiosidade: "As Dunas da Joaquina são perfeitas para a prática de sandboard, um esporte que consiste em descer as dunas em uma prancha." },
    { charada: "🏖️ Um paraíso de luxo e diversão onde o pôr do sol é digno de aplausos. Onde estou?", latitude: -27.4368, longitude: -48.4916, nome: "Praia de Jurerê", curiosidade: "Jurerê Internacional é conhecida por suas festas luxuosas, casas noturnas e um público seleto. Um lugar onde luxo e natureza se encontram." },
    { charada: "🍽️ Frutos do mar, cultura e encontros românticos entre as mesas. Onde estou?", latitude: -27.5951, longitude: -48.5480, nome: "Mercado Público", curiosidade: "O Mercado Público de Florianópolis é um ponto tradicional para comprar frutos do mar frescos e aproveitar a culinária local." },
    { charada: "🌅 No alto da ilha, uma vista que revela toda a beleza de Floripa. Onde estou?", latitude: -27.5888, longitude: -48.5350, nome: "Mirante do Morro da Cruz", curiosidade: "O Mirante do Morro da Cruz oferece uma vista panorâmica de Florianópolis, abrangendo o centro da cidade, a baía sul e a baía norte." }
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
    document.getElementById("curiosidade").textContent = pistas[indiceAtual].curiosidade;
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
        document.getElementById('pista-container').classList.remove('active');
        document.getElementById('roleta-container').classList.add('active');
    }
}

function girarRoleta() {
    const roleta = document.querySelector('.roleta');
    const deg = Math.floor(1000 + Math.random() * 1000);
    roleta.style.transition = 'all 5s ease-out';
    roleta.style.transform = `rotate(${deg}deg)`;
    roleta.addEventListener('transitionend', () => {
        const actualDeg = deg % 360;
        const resultado = obterResultadoRoleta(actualDeg);
        document.getElementById('resultado-roleta').textContent = resultado;
    });
}

function obterResultadoRoleta(angulo) {
    if (angulo >= 0 && angulo < 90) return "Você ganhou um jantar romântico!";
    if (angulo >= 90 && angulo < 180) return "Você ganhou um passeio de barco!";
    if (angulo >= 180 && angulo < 270) return "Você ganhou um buquê de flores!";
    return "Você ganhou uma caixa de chocolates!";
}
