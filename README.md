# Controle Financeiro

App de controle financeiro compartilhado (Fazenda, Construção, Moradia e Faturamento) com
gráficos, previsões e login. O **app** roda no GitHub Pages; o **banco de dados** fica online
no Supabase e é acessado por vários usuários mediante login.

## Passo a passo

### 1. Criar o banco no Supabase
1. Crie uma conta grátis em https://supabase.com e clique em **New project**.
2. Dê um nome, defina uma senha de banco e escolha a região mais próxima. Aguarde criar (~1 min).
3. No menu lateral, abra **SQL Editor → New query**, cole todo o conteúdo do arquivo
   `supabase-setup.sql` e clique em **Run**. Isso cria a tabela, as regras de acesso e o tempo real.

### 2. Pegar as credenciais
1. No painel, vá em **Project Settings → API** (ou **Data API**).
2. Copie o **Project URL** e a chave **anon / publishable** (a pública).
3. Abra o arquivo `index.html`, e no topo substitua:
   ```js
   const SUPABASE_URL      = "COLE_AQUI_A_URL_DO_PROJETO";
   const SUPABASE_ANON_KEY = "COLE_AQUI_A_CHAVE_ANON";
   ```
   A chave anon é pública por natureza — pode ficar no código. O que protege os dados é o
   login + as regras (RLS) do passo 1.

### 3. Criar os usuários
O app **não tem mais cadastro público** — só entra quem você criar manualmente. Isso é
proposital: como a chave anon fica visível no código e o banco é compartilhado (qualquer
usuário logado lê e edita tudo), um cadastro aberto deixaria os dados acessíveis a qualquer
pessoa que encontrasse o link do site.

Para cada pessoa da equipe: painel do Supabase → **Authentication → Users → Add user**,
informe e-mail e senha e entregue essas credenciais a ela.

> Dica: em **Authentication → Providers → Email**, confira se **"Allow new users to sign up"**
> está desligado (o script `supabase-setup.sql` já deixa esse lembrete). Com um grupo pequeno
> e confiável, também dá pra desligar **"Confirm email"**, já que quem cria a conta é você.

### 4. Publicar no GitHub Pages
1. Crie um repositório no seu GitHub e envie **todos** os arquivos desta pasta: `index.html`,
   `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`,
   `apple-touch-icon.png` e `README.md` (o `supabase-setup.sql` já cumpriu a função; pode
   manter no repo como referência). Os arquivos de ícone/manifest são o que permite
   "instalar" o app — sem eles o site funciona normalmente, só não aparece a opção de instalar.
2. No repositório: **Settings → Pages**.
3. Em **Source**, escolha **Deploy from a branch**, selecione a branch `main` e a pasta `/ (root)`.
4. Salve. Em ~1 min o GitHub mostra o endereço público (algo como
   `https://SEU-USUARIO.github.io/NOME-DO-REPO/`). Esse é o link que você compartilha.

Pronto: qualquer pessoa com uma conta acessa o link, faz login e vê/edita o mesmo banco.

### 5. Instalar como aplicativo (celular e computador)
Depois de publicado no GitHub Pages, o site pode ser "instalado" e passa a abrir com ícone
próprio, em janela cheia, sem barra de endereço — igual ao software de controle de rebanho.

- **Android (Chrome)**: abra o link do app, toque no menu (⋮) no canto superior direito e
  escolha **"Instalar aplicativo"** (ou **"Adicionar à tela inicial"**).
- **iPhone/iPad (Safari)**: abra o link, toque no ícone de compartilhar (□↑) e escolha
  **"Adicionar à Tela de Início"**.
- **Computador (Chrome ou Edge)**: abra o link; um ícone de instalação (⊕ ou um monitor com
  seta) aparece do lado direito da barra de endereço — clique nele e depois em **Instalar**.
  Também dá pra achar em **⋮ → Instalar Controle Financeiro…**.

Cada pessoa da equipe instala no próprio celular/computador com a própria conta — o app
instalado continua sendo o mesmo site, só com atalho e visual de aplicativo; os dados
continuam os mesmos para todo mundo.

## Observações importantes

- **Banco compartilhado**: todos que fazem login enxergam e editam os mesmos lançamentos
  (modelo de livro-caixa da equipe). Não há dado privado por usuário.
- **Backup**: o plano gratuito do Supabase não faz backup automático. Use o botão
  **⬇ Backup (Excel)** dentro do app de tempos em tempos para guardar uma cópia.
- **Pausa por inatividade**: projetos gratuitos do Supabase pausam após ~7 dias sem nenhum
  acesso; basta reabrir o painel do Supabase e reativar. Uso frequente evita isso.
- **Tempo real**: quando uma pessoa lança algo, as demais que estão com o app aberto veem a
  atualização automaticamente.
- **Tipo (item)**: cada lançamento tem um campo "Tipo" (ex.: "Ração e suplementação", "Venda
  de bezerros") além da categoria. Já vem com uma lista pronta por aba; escolhendo
  "+ Cadastrar novo tipo…" no formulário, dá pra criar outros na hora — ficam disponíveis
  para todo mundo a partir do próximo lançamento.
- **Atualizando de uma versão anterior**: se você já tinha rodado o `supabase-setup.sql` antes
  e só substituiu esse arquivo agora, abra o SQL Editor e rode os blocos comentados das seções
  1b (trava de categoria) e 1c (coluna do campo "Tipo") — o restante do script pode rodar de
  novo sem problema.
