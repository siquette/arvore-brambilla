# Árvore Genealógica — Memorial dos Brambillas

Protótipo interativo em React com as 489 pessoas extraídas do memorial,
mais os dados fornecidos diretamente por Rodrigo Aroni Siquette (linha
Iracema → Shirley → Rodrigo/Leandro/Rogério).

## Como rodar

Pré-requisito: Node.js instalado (versão 18 ou superior).

```bash
# 1. Instalar dependências
npm install

# 2. Rodar em modo desenvolvimento
npm run dev
```

Isso abre um servidor local (normalmente em `http://localhost:5173`) —
abra essa URL no navegador.

Para gerar uma versão estática (pasta `dist/`) que pode ser hospedada
em qualquer serviço de arquivos estáticos (GitHub Pages, Netlify, etc.):

```bash
npm run build
```

## O que já funciona neste protótipo

- **Layout hierárquico vertical**: gerações A a G empilhadas de cima
  para baixo, calculado automaticamente a partir das relações
  pai/mãe/filho no JSON.
- **Busca por nome**: digite no campo de busca no topo; ao clicar num
  resultado, a árvore expande automaticamente todos os ancestrais
  necessários para revelar a pessoa e abre o painel de detalhes dela.
- **Colapsar/expandir ramos**: clique no botão "–"/"+" na base de
  qualquer nó com filhos, para esconder ou mostrar a descendência
  daquele ramo. Por padrão, tudo está recolhido exceto a linha direta
  de Rodrigo (Gaetano → Giovanni → Mário → Iracema → Shirley →
  Rodrigo), destacada com borda tracejada.
- **Zoom e pan**: scroll do mouse para zoom, clique e arraste para
  navegar.
- **Cores por status do dado**: cada nó tem um ponto colorido indicando
  se o dado está confirmado, estimado, com grafia variante, divergente
  (contradição real entre fontes) ou incompleto — ver legenda no canto
  inferior esquerdo.
- **Painel de detalhes**: clique em qualquer pessoa para ver
  nascimento, falecimento, pais, cônjuges, filhos, observações e
  fontes. Links para pai/mãe navegam diretamente até aquela pessoa.

## O que este protótipo NÃO faz ainda (é local, sem backend)

- **Comentários não são salvos** — o painel mostra a seção de
  comentários, mas ela é só visual por enquanto (útil pra você avaliar
  se o formato faz sentido antes de investir em backend).
- **Não há múltiplos usuários** — isso é um protótipo de uma pessoa só,
  rodando no seu navegador. Para a visão de "compartilhar com parentes
  e todos editarem", será necessário um backend real (banco de dados +
  autenticação), que é a próxima etapa depois de você validar esta
  interface.
- **Não há upload de documentos anexados** por pessoa ainda.

## Estrutura de arquivos

```
projeto-arvore/
├── package.json       # dependências (React + Vite)
├── vite.config.js      # configuração do bundler
├── index.html          # página HTML raiz
├── src/
│   ├── main.jsx         # ponto de entrada React
│   └── App.jsx          # componente principal — TODA a lógica da
│                         # árvore está aqui, incluindo os dados das
│                         # 489 pessoas embutidos como constante no
│                         # topo do arquivo (PESSOAS_RAW)
```

## Publicando no GitHub Pages (repositório arvore-brambilla)

Este projeto já está configurado para publicar em
`https://seu-usuario.github.io/arvore-brambilla/` (repositório próprio,
não a raiz da sua conta) — o arquivo `vite.config.js` já tem
`base: '/arvore-brambilla/'` ajustado para isso. **O nome do
repositório no GitHub precisa ser exatamente `arvore-brambilla`**
(minúsculas, com hífen) para o site funcionar — um nome diferente
exigiria mudar essa linha de novo.

Passo a passo:

1. **Crie o repositório no GitHub** com o nome `arvore-brambilla`
   (pode ser público ou privado — GitHub Pages funciona nos dois,
   desde que sua conta tenha o plano que permite Pages em repositório
   privado; se tiver dúvida, deixe público).

2. **Suba este projeto para esse repositório:**

   ```bash
   cd projeto-arvore
   git init
   git add .
   git commit -m "Árvore genealógica Brambilla"
   git branch -M main
   git remote add origin https://github.com/siquette/arvore-brambilla.git
   git push -u origin main
   ```

3. **Ative o GitHub Pages via Actions:**
   No repositório, vá em **Settings → Pages** e, em "Build and
   deployment" → "Source", selecione **GitHub Actions** (não
   "Deploy from a branch"). O workflow já incluído neste projeto
   (`.github/workflows/deploy.yml`) builda e publica automaticamente
   a cada push na branch `main`.

4. **Aguarde o deploy** (aparece em Settings → Pages e na aba
   "Actions" do repositório, geralmente leva 1–2 minutos). Depois
   disso o site fica em `https://seu-usuario.github.io/arvore-brambilla/`
   — **repare na barra final `/arvore-brambilla/`**, sem ela a página
   não carrega os arquivos corretamente.

Qualquer alteração que você fizer depois (corrigir uma data, adicionar
uma pessoa em `src/App.jsx`) — basta commitar e dar `git push` de novo;
o Actions rebuilda e republica sozinho.



## Editando os dados

Os dados estão embutidos diretamente em `src/App.jsx` (constante
`PESSOAS_RAW`, logo no topo do arquivo) para simplificar este
protótipo — não há chamada de rede nem arquivo externo. Se quiser
editar uma pessoa (corrigir uma data, adicionar uma observação),
localize o registro pelo campo `"id"` e edite o JSON diretamente ali.

Para uma iteração futura com múltiplos parentes editando ao mesmo
tempo, esses dados precisarão migrar para um banco de dados real
(Postgres, Firebase, etc.) em vez de ficarem hardcoded no código — isso
é o próximo passo depois de validar que a interface e a estrutura de
dados fazem sentido

