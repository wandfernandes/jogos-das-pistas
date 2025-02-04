import React, { useState } from "react";
import QRCode from "qrcode.react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { MapPin, CheckCircle, XCircle } from "lucide-react";

const pistas = [
  {
    id: 1,
    pergunta: "Onde começa nossa jornada?",
    opcoes: ["Praça Central", "Shopping Downtown", "Parque da Cidade"],
    respostaCorreta: "Praça Central",
    localizacao: "https://maps.google.com/?q=-15.7801,-47.9292",
  },
  {
    id: 2,
    pergunta: "Qual é o símbolo do nosso primeiro encontro?",
    opcoes: ["Uma rosa vermelha", "Um café especial", "Uma estrela no céu"],
    respostaCorreta: "Um café especial",
    localizacao: "https://maps.google.com/?q=-15.7815,-47.9256",
  },
  // Adicione mais pistas conforme necessário
];

export default function CacaAoTesouro() {
  const [pistaAtual, setPistaAtual] = useState(0);
  const [respostaSelecionada, setRespostaSelecionada] = useState(null);
  const [respostaCorreta, setRespostaCorreta] = useState(false);

  const verificarResposta = (opcao) => {
    const correta = opcao === pistas[pistaAtual].respostaCorreta;
    setRespostaSelecionada(opcao);
    setRespostaCorreta(correta);
    if (correta) {
      setTimeout(() => {
        if (pistaAtual < pistas.length - 1) {
          setPistaAtual(pistaAtual + 1);
          setRespostaSelecionada(null);
        }
      }, 2000);
    }
  };

  return (
    <div className="flex flex-col items-center p-6 space-y-6">
      <Card className="w-full max-w-lg p-6 text-center">
        <CardContent>
          <h2 className="text-xl font-bold">{pistas[pistaAtual].pergunta}</h2>
          <div className="mt-4 space-y-2">
            {pistas[pistaAtual].opcoes.map((opcao, index) => (
              <Button
                key={index}
                onClick={() => verificarResposta(opcao)}
                variant={respostaSelecionada === opcao ? "secondary" : "default"}
                className="w-full"
                aria-pressed={respostaSelecionada === opcao}
              >
                {opcao}
              </Button>
            ))}
          </div>
          {respostaSelecionada && (
            <div className="mt-4 flex items-center justify-center space-x-2">
              {respostaCorreta ? (
                <CheckCircle className="text-green-500" size={24} />
              ) : (
                <XCircle className="text-red-500" size={24} />
              )}
              <p className={respostaCorreta ? "text-green-500" : "text-red-500"}>
                {respostaCorreta ? "Correto!" : "Errado! Tente novamente."}
              </p>
            </div>
          )}
        </CardContent>
      </Card>

      <Card className="w-full max-w-lg p-6 text-center">
        <CardContent>
          <h2 className="text-lg font-semibold">Próxima Pista</h2>
          <p className="text-gray-600">Escaneie o QR Code para a localização da próxima pista.</p>
          <div className="mt-4">
            <QRCode value={pistas[pistaAtual].localizacao} size={128} />
          </div>
          <a
            href={pistas[pistaAtual].localizacao}
            target="_blank"
            rel="noopener noreferrer"
            className="mt-4 flex items-center justify-center text-blue-600"
          >
            <MapPin className="mr-2" /> Abrir no Google Maps
          </a>
        </CardContent>
      </Card>
    </div>
  );
}
