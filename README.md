<div align="center">

# 🌿 MicroClima • Observatório

### A floresta tem muita história para contar. A gente só colocou os dados para conversar. 📈

**Um dashboard interativo para explorar o microclima de ambientes amazônicos — sem fórmulas na tela e sem complicação.**

[![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=white)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-6-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vite.dev/)
[![Vercel](https://img.shields.io/badge/Vercel-ready-black?style=flat-square&logo=vercel)](https://vercel.com/)

**Pasto · Floresta secundária · Floresta primária**

</div>

---

## 👋 E aí, o que é isso?

Sabe aquela pergunta *“será que a temperatura muda tanto assim de um pasto para uma floresta?”* Então. Em vez de ficar só na curiosidade, aqui dá para passear pelos **dados reais coletados por sensores ambientais** e ver os padrões acontecendo.

O **MicroClima Observatório** nasceu a partir da interface do **MicroClima 360°**, mas ganhou vida própria: agora o foco é **visualização de dados ambientais**, sem laboratório matricial, quiz de Álgebra Linear ou apresentação guiada.

## ✨ O que dá para explorar?

| Painel | O que você encontra |
| --- | --- |
| 🌡️ **Atmosfera** | Temperatura do ar registrada pelos sensores HOBO nos três ambientes |
| 💧 **Umidade relativa** | Como a umidade muda ao longo do tempo |
| 💨 **VPD** | Déficit de pressão de vapor estimado a partir da temperatura e umidade relativa |
| 📅 **Navegação temporal** | Escolha um dia, veja a sequência de medições de cinco em cinco minutos ou explore todo o período |
| 🌱 **Solo & TMS** | Três sondas TOMST TMS do pasto, canais de temperatura T1, T2, T3 e sinal bruto relacionado à umidade do solo |
| 📥 **Exportação** | Baixe a série TMS atualmente selecionada em CSV |

O visual é escuro, inspirado na floresta, com gráficos interativos, cartões-resumo e filtros simples.

## 🗂️ De onde vêm os dados?

### 1. HOBO: atmosfera em três ambientes

Usamos o CSV histórico já preparado no projeto original, com **8.340 horários simultâneos**, entre **12/08/2026, 08:25** e **10/09/2026, 07:20**, sem interpolação. São dados de temperatura (°C) e umidade relativa (%), para:

- Pasto;
- Floresta secundária;
- Floresta primária.

Arquivo: [`public/data/hobo-alinhado.csv`](public/data/hobo-alinhado.csv).

**VPD:** calculado com a equação de Tetens, a partir de temperatura e umidade relativa. É uma variável **derivada**, não uma leitura direta do HOBO.

### 2. TOMST TMS: outro jeito de enxergar o ambiente

Também incluímos dados reais de **três sensores TMS instalados no pasto**, localizados no Google Drive, com arquivos de origem:

- `01_PASTO_10_03_2026.csv`
- `02_PASTO_10_03_2026.csv`
- `03_PASTO_10_03_2026.csv`

Para manter esta versão leve e rápida no navegador, a aba TMS contém **médias diárias de datas selecionadas com espaçamento de sete dias**, entre **15/03/2026** e **06/09/2026**. **Não são médias semanais**: cada ponto representa a média das observações de **um dia específico**.

Os três canais de temperatura são mostrados como **T1, T2 e T3**, sem atribuir profundidades que não foram confirmadas para cada arquivo. O sinal associado à umidade do solo permanece **na escala bruta do sensor**, pois não foi aplicada uma calibração para converter a medição em umidade volumétrica (%).

Arquivo: [`public/data/tms-pasto-amostra-semanal.csv`](public/data/tms-pasto-amostra-semanal.csv).

> ⚠️ **Importante:** o app não apresenta dados ao vivo, nem transforma automaticamente o sinal bruto do TMS em umidade volumétrica. Estamos mostrando registros históricos com suas limitações explicitadas.

## 🛠️ Tecnologias

- **React + TypeScript:** interface.
- **Vite:** desenvolvimento e compilação.
- **Recharts:** gráficos responsivos e interativos.
- **Lucide React:** ícones.
- **CSS:** visual feito sob medida, sem biblioteca pesada de componentes.
- **CSV:** dados históricos incluídos no próprio projeto.

**Sem backend Python nesta versão.** Tudo funciona como um site estático — ótimo para publicar no Vercel e evitar dores de cabeça com múltiplos serviços. 😅

## 🚀 Rodando no seu computador

É só ter **Node.js** e **npm** instalados:

```bash
git clone https://github.com/SEU_USUARIO/MicroClima-Observatorio.git
cd MicroClima-Observatorio
npm install
npm run dev
```

Depois abra o endereço informado no terminal, normalmente [`http://localhost:5173`](http://localhost:5173).

Para testar a versão final:

```bash
npm run build
npm run preview
```

## ☁️ Publicando no Vercel

1. Crie um repositório no GitHub e envie o conteúdo **desta pasta diretamente para a raiz do repositório**.
2. Acesse [vercel.com/new](https://vercel.com/new) e importe o novo repositório.
3. Selecione **Vite** (geralmente detectado automaticamente).
4. Use **Root Directory: `./`**, **Build Command: `npm run build`** e **Output Directory: `dist`**.
5. Clique em **Deploy**. 🌎

O [`vercel.json`](vercel.json) já está configurado para a implantação estática. Não precisa configurar serviços FastAPI, funções Python nem variáveis de ambiente.

## 📁 Organização dos arquivos

```text
MicroClima-Observatorio/
├── public/
│   └── data/
│       ├── hobo-alinhado.csv
│       └── tms-pasto-amostra-semanal.csv
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   └── styles.css
├── .gitignore
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
├── vercel.json
└── README.md
```

## 🌍 E o que vem depois?

Tem muita coisa legal que ainda dá para colocar aqui: dados completos dos TMS, outros ambientes, precipitação, sensores de nível d'água, filtros mais avançados e exportação dos gráficos. Mas cada nova variável entra **só depois de conferirmos unidades, metadados e qualidade dos registros**.

## 💚 Por que fazer isso?

Porque dados ambientais não precisam ficar escondidos em planilhas gigantes. Quando a gente transforma séries temporais em gráficos fáceis de explorar, fica muito mais simples enxergar diferenças, fazer perguntas melhores e compartilhar o que os sensores estão contando.

**Feito com dados reais, curiosidade científica e muito café. ☕🌱**

*Projeto de visualização ambiental no contexto de atividades acadêmicas da UFRA, na Amazônia Oriental.*
