# 📘 Aula: React + Vite + Tailwind – Explorando Math e Date

## Estrutura de Pastas

```bash
/projeto-math-date
```

## 🚀 1. Frontend (React + Vite)

### Passo 1: Inicialização do Projeto

No terminal:

```bash
npm create vite@latest projeto-math-date
cd projeto-math-date
npm install
npm install tailwindcss @tailwindcss/vite
```

### Passo 2: Configure o Tailwind

Edite `vite.config.js`:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

No `src/index.css` (ou `App.css`):

```css
@import "tailwindcss";
```

### Passo 3: Código do App.jsx

Substitua o `src/App.jsx`:

```js
import { useState, useEffect } from "react";

function App() {
  // Estados
  const [randomNumber, setRandomNumber] = useState(null);
  const [currentTime, setCurrentTime] = useState(new Date());

  // Atualiza o relógio a cada segundo
  useEffect(() => {
    const timer = setInterval(() => {
      setCurrentTime(new Date());
    }, 1000);

    return () => clearInterval(timer);
  }, []);

  // Gera número aleatório entre 1 e 100
  const generateNumber = () => {
    const num = Math.floor(Math.random() * 100) + 1;
    setRandomNumber(num);
  };

  // Formatação de hora
  const formattedTime = currentTime.toLocaleTimeString();

  return (
    <div className="flex items-center justify-center min-h-screen bg-gradient-to-br from-blue-100 to-purple-200">
      <div className="bg-white p-8 rounded-xl shadow-lg w-full max-w-md text-center">
        <h1 className="text-2xl font-bold mb-6">Explorando Math e Date</h1>

        {/* Relógio */}
        <div className="mb-6">
          <h2 className="text-lg font-semibold text-gray-700">⏰ Relógio Atual</h2>
          <p className="text-3xl font-mono mt-2">{formattedTime}</p>
        </div>

        {/* Gerador de Números */}
        <div className="mb-6">
          <h2 className="text-lg font-semibold text-gray-700">🎲 Número Aleatório</h2>
          {randomNumber !== null && (
            <p className="text-3xl font-bold text-green-600 mt-2">{randomNumber}</p>
          )}
          <button
            onClick={generateNumber}
            className="mt-4 bg-blue-500 text-white py-2 px-4 rounded-md hover:bg-blue-600 transition-colors"
          >
            Gerar Número
          </button>
        </div>
      </div>
    </div>
  );
}

export default App;
```

### Passo 4: Rodar o Projeto

```bash
npm run dev
```
# 📝 Exercício Prático – Dashboard Aleatório e Temporal

Este projeto reúne exemplos de **Math** e **Date** em um único `App.jsx`, formando um **dashboard** com três cards:
- Relógio digital em tempo real
- Sorteio de números (estilo Mega-Sena)
- Contagem regressiva até uma data

---

## Código Completo (App.jsx)

```jsx
import { useState, useEffect } from "react";

function App() {
  // -----------------------------
  // 1. ESTADOS DO COMPONENTE
  // -----------------------------
  const [currentTime, setCurrentTime] = useState(new Date()); // relógio
  const [numeros, setNumeros] = useState([]); // sorteio
  const [tempoRestante, setTempoRestante] = useState({}); // contagem regressiva
  const [dataInput, setDataInput] = useState(""); // data digitada pelo usuário

  // -----------------------------
  // 2. RELÓGIO – atualiza a cada segundo
  // -----------------------------
  useEffect(() => {
    const timer = setInterval(() => {
      setCurrentTime(new Date());
    }, 1000);

    return () => clearInterval(timer);
  }, []);

  const formattedTime = currentTime.toLocaleTimeString();

  // -----------------------------
  // 3. SORTEIO DE NÚMEROS (6 entre 1 e 60, sem repetir)
  // -----------------------------
  const gerarSorteio = () => {
    const numerosGerados = new Set();
    while (numerosGerados.size < 6) {
      numerosGerados.add(Math.floor(Math.random() * 60) + 1);
    }
    setNumeros([...numerosGerados]);
  };

  // -----------------------------
  // 4. CONTAGEM REGRESSIVA
  // -----------------------------
  useEffect(() => {
    if (!dataInput) return;

    const intervalo = setInterval(() => {
      const dataFinal = new Date(dataInput);
      const agora = new Date();
      const diff = dataFinal - agora;

      if (diff <= 0) {
        clearInterval(intervalo);
        setTempoRestante({ mensagem: "🎉 Chegou o grande dia!" });
        return;
      }

      const dias = Math.floor(diff / (1000 * 60 * 60 * 24));
      const horas = Math.floor((diff / (1000 * 60 * 60)) % 24);
      const minutos = Math.floor((diff / (1000 * 60)) % 60);
      const segundos = Math.floor((diff / 1000) % 60);

      setTempoRestante({ dias, horas, minutos, segundos });
    }, 1000);

    return () => clearInterval(intervalo);
  }, [dataInput]);

  // -----------------------------
  // 5. RESETAR TUDO
  // -----------------------------
  const resetar = () => {
    setNumeros([]);
    setTempoRestante({});
    setDataInput("");
  };

  // -----------------------------
  // 6. INTERFACE (UI)
  // -----------------------------
  return (
    <div className="flex items-center justify-center min-h-screen bg-gradient-to-br from-blue-100 to-purple-200 p-6">
      <div className="grid gap-6 md:grid-cols-3 w-full max-w-6xl">
        
        {/* CARD 1 - RELÓGIO */}
        <div className="bg-white p-6 rounded-lg shadow-md text-center">
          <h2 className="text-xl font-bold">⏰ Relógio Digital</h2>
          <p className="text-3xl font-mono mt-4">{formattedTime}</p>
        </div>

        {/* CARD 2 - SORTEIO */}
        <div className="bg-white p-6 rounded-lg shadow-md text-center">
          <h2 className="text-xl font-bold">🎰 Sorteio de Números</h2>
          <div className="flex justify-center gap-2 my-4 flex-wrap">
            {numeros.map((n, i) => (
              <span
                key={i}
                className="w-10 h-10 flex items-center justify-center bg-green-500 text-white rounded-full"
              >
                {n}
              </span>
            ))}
          </div>
          <button
            onClick={gerarSorteio}
            className="bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700 transition"
          >
            Gerar Sorteio
          </button>
        </div>

        {/* CARD 3 - CONTAGEM REGRESSIVA */}
        <div className="bg-white p-6 rounded-lg shadow-md text-center">
          <h2 className="text-xl font-bold">⏳ Contagem Regressiva</h2>

          {/* Input de data */}
          <input
            type="datetime-local"
            value={dataInput}
            onChange={(e) => setDataInput(e.target.value)}
            className="border px-2 py-1 rounded-md mt-2 w-full"
          />

          {/* Exibição da contagem */}
          <div className="mt-4">
            {tempoRestante.mensagem ? (
              <p className="text-2xl text-green-600 font-bold">
                {tempoRestante.mensagem}
              </p>
            ) : tempoRestante.dias !== undefined ? (
              <p className="text-lg font-mono">
                {tempoRestante.dias}d {tempoRestante.horas}h {tempoRestante.minutos}m {tempoRestante.segundos}s
              </p>
            ) : (
              <p className="text-gray-500">Escolha uma data para começar</p>
            )}
          </div>
        </div>
      </div>

      {/* BOTÃO RESET */}
      <div className="absolute bottom-6 right-6">
        <button
          onClick={resetar}
          className="bg-red-500 text-white px-4 py-2 rounded-md hover:bg-red-600 transition"
        >
          Resetar Tudo
        </button>
      </div>
    </div>
  );
}

export default App;
```

## 🎯 Atividade

1. Replique o código acima em um projeto React + Vite + Tailwind.

2. Analise como Math e Date foram usados em cada card.

3. Faça modificações criativas, como:

- Mostrar o dia da semana no card do relógio.

- Adicionar opção de quantidade de números no sorteio.

- Personalizar o estilo da contagem regressiva.

💡 Dica: explore outras funções do Math (`ceil`, `round`, `max`, `min`) e do objeto Date (`getDay`, `getMonth`, `getFullYear`).
