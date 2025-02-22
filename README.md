# Prática: Git com Express

Este roteiro irá guiá-los na criação de um repositório Git local, desenvolvimento de branches para features distintas, execução de merges e resolução de conflitos, além de trabalhar com rebase. Ao final, vocês irão associar o repositório local a um repositório remoto no GitHub e realizar o push.

> [!WARNING]
Os comandos presentes neste roteiro são para serem executados em um terminal de linha de comando. Caso você esteja utilizando o Windows, é recomendado o uso do Git Bash, que é uma ferramenta que já vem com o Git e simula um ambiente Linux no Windows.
> 
> Você também pode utilizar o WSL (Windows Subsystem for Linux) para ter acesso ao bash.

## Pré-requisitos

- Conhecimento básico de JavaScript
- Node.js instalado
- Git instalado
- Conta no GitHub

## Acessar um diretório
Abra seu terminal (ou prompt de comando) e navegue até o diretório onde deseja criar o projeto. Você pode usar o comando `cd` para isso.

```bash
cd /caminho/para/seu/diretorio
```

> [!NOTE]
> É possível que você precise criar um diretório para o projeto. Use o comando `mkdir` para isso, após navegar até o local desejado.
>
> ```bash
> mkdir meu-projeto
> cd meu-projeto
> ```

## Criar um repositório local

Inicialize o repositório Git dentro do diretório selecionado:

```bash
git init
```
> [!NOTE]
> Isso criará uma pasta oculta `.git` que contém todo o histórico e as configurações do seu repositório local.

## Configurando seus dados no repositorio

Antes de começar a trabalhar, é importante configurar seu nome de usuário e e-mail no Git. Isso é importante para que os commits feitos tenham a informação correta do autor.

```bash
git config user.name "Seu Nome"
git config user.email "Seu Email"
```

> [!NOTE]
> Você pode verificar as configurações do Git a qualquer momento com o comando `git config --list`.
> ```bash
> git config --list
> ```
> Se precisar alterar alguma configuração, basta rodar o comando `git config` novamente.

> [!TIP]
> Se você quiser que essas configurações sejam globais (ou seja, válidas para todos os seus repositórios), adicione a flag `--global`:
> ```bash
> git config --global user.name "Seu Nome"
> git config --global user.email "Seu Email"
> ```

> [!IMPORTANT]
> Se você não configurar seu nome e e-mail, o Git pode reclamar e não permitir que você faça commits. Se isso acontecer, verifique se você configurou corretamente.

> [!WARNING]
> Se você estiver trabalhando em um ambiente de trabalho ou em um repositório compartilhado, é importante que você use o mesmo e-mail que está associado à sua conta no GitHub. Isso é importante para que os commits feitos por você sejam corretamente associados ao seu perfil.

> [!CAUTION]
> Se você estiver trabalhando em um **computador de uso compartilhado** (como num laboratório de uma escola ou de uma biblioteca), é importante que você configure seu nome e e-mail a cada vez que for trabalhar em um projeto diferente. Isso evita que commits feitos por você sejam associados ao nome e e-mail de outra pessoa.
> 
> Nesse cenário, evite usar a flag `--global` para que as configurações sejam específicas para o projeto em questão e não afetem outros projetos.

## Criar um projeto básico em Express (Skeleton)

Nesta etapa, criaremos um esqueleto mínimo de projeto em Express. Há várias formas de se fazer isso. Aqui, faremos manualmente, sem a ferramenta express-generator, para manter o exemplo bem simples.

### Estrutura de Arquivos

Crie a seguinte estrutura de diretórios e arquivos:
```plaintext
.
├── package.json
├── index.js
├── README.md
└── .gitignore
```

> [!NOTE]
> Para criar os arquivos diretamente pelo terminal, você pode usar o comando `touch`:
> ```bash
> touch package.json index.js README.md .gitignore
> ```
> Ou, se preferir, pode criar e editar os arquivos manualmente em um editor de texto - como o **VS Code**, por exemplo.

### Conteúdo dos arquivos

#### `package.json`

Crie (ou edite) o arquivo `package.json` com o conteúdo abaixo:

```json
{
  "name": "express-skeleton",
  "version": "1.0.0",
  "description": "Projeto básico para demonstrar Git com Express",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "keywords": [],
  "author": "Seu Nome",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```


#### `index.js`

Crie o arquivo `index.js` com o conteúdo abaixo:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

#### `README.md`

Crie ou edite o arquivo `README.md` com o conteúdo abaixo:

```markdown
# Express Skeleton

Este projeto é um exemplo básico de um servidor em Express.
```

#### `.gitignore`

Crie ou edite o arquivo `.gitignore` com o conteúdo abaixo:

```plaintext
node_modules
```

### Instalar dependências

Em seguida, volte ao terminal e instale as dependências:

```bash
npm install
```

## Fazer o commit inicial

Adicione todos os arquivos e faça o primeiro commit:

```bash
git add . # Adiciona todos os arquivos
git status # Verifica o status do repositório
git commit -m "chore: commit inicial com skeleton express"
```

> [!TIP]
> O comando `git add .` adiciona todos os arquivos e pastas do diretório atual. Se você quiser adicionar apenas um arquivo específico, você pode fazer assim:
> ```bash
> git add nome-do-arquivo
> ```
>
> Você pode ver o status do repositório a qualquer momento com o comando `git status`:
> ```bash
> git status
> ```

> [!NOTE]
> Note que ao executar o comando `git status` durante a execução dos comandos acima, você não verá o diretório `node_modules` listado. Isso ocorre porque o arquivo `.gitignore` foi criado e seu conteúdo especifica que o diretório `node_modules` deve ser ignorado.

## Criar uma branch para a primeira feature

Crie a branch `feature-1` a partir da main:

```bash
git checkout -b feature-1
```

## Escrever uma feature simples

Vamos supor que a primeira feature é adicionar uma rota nova que retorna uma mensagem de boas-vindas.

Edite o arquivo `index.js` para adicionar uma rota `/welcome`. No final do arquivo, acima de `app.listen`, adicione:

```js
app.get('/welcome', (req, res) => {
    res.send('Bem-vindo(a) à primeira feature!');
});
```

Assim, o arquivo `index.js` ficará assim:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/welcome', (req, res) => {
  res.send('Bem-vindo(a) à primeira feature!');
});

app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

## Fazer o commit (*semantic git*)

De volta ao terminal, adicione o staging e faça commit das mudanças, da mesma forma de antes.

Nesta etapa, execute cada comando separadamente para ver o resultado:

```bash
git status
```

```bash
git add .
```

```bash
git status
```

```bash
gi
```

## De volta à branch `main`

Agora retornaremos para a branch principal (main):

```bash
git checkout main
```

> [!NOTE]
> Note que ao voltar para a branch `main`, o conteúdo do arquivo `index.js` volta ao estado anterior, sem a rota `/welcome`.
>
> Isso acontece porque a branch `feature-1` ainda não foi mesclada na branch `main`, então aquelas mudanças não estão presentes na main - elas fazem parte da "foto" da branch `feature-1`.

## Atualizar o `README.md`

Edite o arquivo `README.md` para incluir um tópico adicional sobre o projeto e a nova rota que virá (nesse momento, ainda não existe a rota oficialmente na main, mas vamos adicionar uma referência simples).

Exemplo de conteúdo:

```md
# Express Skeleton

Este projeto é um exemplo básico de um servidor em Express.

## Funcionalidades Planejadas
- Rota `/welcome`
- Rota `/info`
```

## Fazer o commit (*semantic git*)

De volta ao terminal, adicione e faça o commit da mudança no `README.md`:

```bash
git add README.md
git commit -m "docs: adiciona sessão de funcionalidades planejadas"
```

> [!TIP]
> Você pode usar o comando `git log` para ver o histórico de commits do repositório:
> ```bash
> git log
> ```
> Para sair do `git log`, pressione `q`.

> [!TIP]
> Eu juro que é a última vez que vou falar isso, mas você pode usar o comando `git status` para ver o status do repositório a qualquer momento:
> ```bash
> git status
> ```

> [!TIP]
> E, claro, você pode usar o comando `git diff` para ver as diferenças entre o seu código atual e o último commit:
> ```bash
> git diff
> ```
> Para sair do `git diff`, pressione `q`.
>
> Estes comandos são **extremamente importantes** para entender o que está acontecendo no seu repositório, e você pode (e **deve**) usá-los sempre que precisar.

## Criar nova branch para a segunda feature

Vamos criar agora a branch para a segunda feature:

```bash
git checkout -b feature-2
```

> [!TIP]
> Se você precisar voltar para outro branch e depois quiser voltar para a branch atual, você pode usar o comando `git checkout nome-da-branch` ou `git switch nome-da-branch`:
> ```bash
> git checkout main
> git switch feature-2
> ```
> Neste cenário os comandos acima são equivalentes.
> 
> Isso é útil para navegar entre as branches do seu repositório.
>
> Você também pode usar o comando `git branch` para listar todas as branches do repositório:
> ```bash
> git branch
> ```
> A branch atual estará marcada com um asterisco `*`.
> Para sair do `git branch`, pressione `q`.


## Escrever outra feature que altera a mesma parte do código

Vamos supor que a segunda feature adiciona uma rota `/info` acima da linha de finalização no index.js. Isso vai se sobrepor à mesma área onde a rota `/welcome` foi adicionada.

Edite `index.js` (no mesmo local de rotas que a rota `/welcome`) e adicione:

```js
app.get('/info', (req, res) => {
  res.send('Informações sobre o projeto');
});
```

Assim, o arquivo `index.js` ficará assim:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/info', (req, res) => {
  res.send('Informações sobre o projeto');
});

app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

> [!WARNING]
> Note que a rota `/info` foi adicionada acima da rota `/welcome`. Isso é proposital para gerar um conflito de merge mais adiante.

## Fazer o commit (*semantic git*)

```bash
git add .
git commit -m "feat: adiciona rota /info"
```

## Fazer o merge da primeira feature no main

Volte para a branch main:

```bash
git switch main
```

Faça o merge da branch `feature-1` na main:

```bash
git merge feature-1
```

O comando `git merge` irá criar um novo commit de merge, que combina as mudanças da branch `feature-1` com a branch `main`.

Assim, provavelmente será aberto um editor de texto para você confirmar a mensagem de merge. Por padrão, o Git usa o editor configurado na variável `EDITOR` do seu sistema. Se você não tiver configurado um editor, o Git usará o editor padrão do sistema. Em ambiente Ubuntu, por exemplo, o editor padrão é o `nano`; já no Debian, é o `vim`.

> [!TIP]
> Se você não quiser abrir o editor, você pode usar a flag `-m` para adicionar a mensagem diretamente no comando:
> ```bash
> git merge feature-1 -m "Merge da feature-1 na main"
> ```
> Isso é útil para mensagens de merge simples.

> [!TIP]
> Para sair do `nano`, pressione `Ctrl + X` e, em seguida, `Y` para confirmar a saída.
>
> Para sair do `vim`, pressione `Esc` e digite `:wq` e, em seguida, pressione `Enter`.

Se tudo correr bem, sem conflitos (o que deve acontecer), veremos algo como:

```plaintext
Updating xxxxxxx..yyyyyyy
Fast-forward
 index.js | 4 ++++
 1 file changed, 4 insertions(+)
```

> [!NOTE]
> O termo `Fast-forward` indica que o merge foi feito sem problemas, pois a branch `main` estava atualizada com as mudanças da branch `feature-1`.
>
> Essa saída é um exemplo, e o número de commits e arquivos pode variar dependendo do seu projeto.
>
> A saída também pode ser diferente se você estiver usando um editor de texto diferente do `nano` ou `vim`, e também se você estiver usando um sistema operacional diferente.


## Fazer o merge da segunda feature no main (deve gerar conflito)
![image](https://media1.tenor.com/m/z_KoI0-y7rEAAAAC/chaos.gif)

Agora faremos o merge da branch `feature-2`:

```bash
git merge feature-2
```

Aqui, pode ocorrer um conflito de merge no index.js, pois você adicionou a rota `/info` e a rota `/welcome` em linhas próximas. O Git pode não conseguir mesclar sozinho.

Se tudo correr bem (ou seja, com o conflito planejado), você verá algo como:

```plaintext
Auto-merging index.js
CONFLICT (content): Merge conflict in index.js
Automatic merge failed; fix conflicts and then commit the result.
```

Isso indica que houve um conflito de merge no arquivo `index.js`. Vamos resolver isso.

## Avaliando a situação

Este é um bom momento para avaliar a situação. Você pode fazer isso com os comandos `git status` e `git diff`, conforme já vimos.

```bash
git status
```

```bash
git diff
```

## Resolver o merge
Abra o `index.js` e procure pela(s) marcações de conflito. Algo como:

```js
<<<<<<< HEAD
app.get('/welcome', (req, res) => {
  res.send('Bem-vindo(a) à primeira feature!');
});
=======
app.get('/info', (req, res) => {
  res.send('Informações sobre o projeto');
});
>>>>>>> feature-2
```

Decida como quer combinar as duas rotas. Por exemplo, você pode incluir as duas rotas:

```js
app.get('/welcome', (req, res) => {
  res.send('Bem-vindo(a) à primeira feature!');
});

app.get('/info', (req, res) => {
  res.send('Informações sobre o projeto');
});
```

> [!NOTE]
> Em situações de conflito, as marcações `<<<<<<<`, `=======` e `>>>>>>>` são usadas para indicar as diferenças entre as branches que estão sendo mescladas.
>
> Você deve remover essas marcações e decidir como quer combinar as mudanças. Em alguns casos, você pode querer manter apenas uma parte, ou pode querer manter ambas, ou pode querer fazer uma combinação das duas.

> [!NOTE]
> As marcações delimitam as mudanças de cada branch:
> - `<<<<<<< HEAD` indica o início das mudanças da branch atual (no caso, a branch `main`).
> - `=======` indica o fim das mudanças da branch atual e o início das mudanças da outra branch (no caso, a branch `feature-2`).
> - `>>>>>>> feature-2` indica o fim das mudanças da outra branch.

## Resolver o conflito

Para fins didáticos, vamos manter as duas rotas. Remova todas as marcações `<<<<<<<`, `=======` e `>>>>>>>` e deixe o arquivo assim:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/welcome', (req, res) => {
  res.send('Bem-vindo(a) à primeira feature!');
});

app.get('/info', (req, res) => {
  res.send('Informações sobre o projeto');
});

app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

Agora é outro bom momento para avaliar a situação:

```bash
git status
```

```bash
git diff
```

Neste ponto, você pode ver que o conflito foi resolvido e o arquivo `index.js` está pronto para ser commitado.

Também podemos ver que o repositório está em um estado de merge, pois o Git está esperando que você confirme o merge:

```plaintext
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)
```

**Como a mensagem sugere, podemos fazer o commit para finalizar o merge ou abortar o merge se algo deu errado, ou você decidiu que não quer mais fazer o merge (ou talvez você só não tenha certeza do que fazer e queira abortar para pensar melhor / pedir ajuda).**

Como queremos continuar e terminar esta prática, adicione o arquivo ao staging e faça o commit:

```bash
git add index.js
git commit # sem a flag -m para abrir o editor de texto
```

O editor padrão abrirá para que você confirme/edite a mensagem de merge, que deve ser algo como "Merge da feature-2 na main".

> [!NOTE]
> Quando o Git está em um estado de merge, o commit que você faz é um commit de merge, que é um tipo especial de commit que registra o merge de duas branches.

> [!NOTE]
> Observe que, quando o editor de texto abrir, você verá uma mensagem de merge padrão, possivelmente seguida por várias linhas de comentários (iniciadas por `#`). Você pode manter a mensagem padrão ou alterá-la conforme necessário.
>
> Os comentários são úteis para entender o que está acontecendo no commit de merge, mas você pode removê-los se quiser. Esses comentários não são parte da mensagem de commit e não são exibidos no histórico de commits.

## Visualizar o histórico de commits

Agora que fizemos o merge da branch `feature-2` na branch `main`, podemos ver o histórico de commits:

```bash
git log
```

Você verá uma lista de commits, com o commit de merge mais recente no topo. Cada commit tem um hash (uma sequência de letras e números que identifica unicamente o commit), uma mensagem de commit e outras informações.

Nas informações dos commits de merge, você verá que eles têm dois pais (parentes), que são os commits das branches que estão sendo mescladas (referenciados por `Merge: xxxxxx yyyyyy`).

Outra informação importante a ser observada neste ponto são as referências das branches. Você verá que a branch `main` agora aponta para o commit de merge mais recente, e que as branches `feature-1` e `feature-2` ainda apontam para os commits originais de cada branch. Além disso, a "branch" `HEAD` aponta para a branch `main`, indicando que estamos na branch `main`.

> [!TIP]
> Outra forma muito legal de visualizar o histórico de commits, das branches e merges que foram feitos é utilizando a flag `--graph` com o comando `git log`:
>
> ```bash
> git log --graph --oneline --all
> ```
> A flag `--oneline` exibe cada commit em uma única linha, e a flag `--all` exibe todos os commits de todas as branches.
>
> Você verá uma representação gráfica do histórico de commits, com as branches e os merges. Isso é muito útil para entender a estrutura do seu repositório.

## Criar uma terceira branch

Vamos criar uma terceira branch para mais uma feature:

```
git checkout -b feature-3
```

## Escrever outra feature simples

Desta vez, vamos criar uma rota `/about`:

1. No `index.js`, acima de `app.listen`, adicione:

```js
app.get('/about', (req, res) => {
  res.send('Sobre este projeto...');
});
```

Assim, o arquivo `index.js` ficará assim:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/welcome', (req, res) => {
  res.send('Bem-vindo(a) à primeira feature!');
});

app.get('/info', (req, res) => {
  res.send('Informações sobre o projeto');
});

app.get('/about', (req, res) => {
  res.send('Sobre este projeto...');
});

app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

## Fazer o commit (*semantic git*)

```bash
git add .
git commit -m "feat: adiciona rota /about"
```

## Adicionar uma nova rota no README na branch `main`

Agora que nossa nova rota está pronta e commitada na devida branch, vamos novamente voltar para a branch `main` e adicionar a nova rota no `README.md`.

```bash
git switch main
```

Vamos adicionar a rota `/about` no `README.md`:

```md
# Express Skeleton

Este projeto é um exemplo básico de um servidor em Express.

## Funcionalidades Planejadas
- Rota `/welcome`
- Rota `/info`
- Rota `/about`
```

> [!TIP]
> Se você quiser fazer esta alteração de forma mais rápida, diretamente pelo terminal, você pode usar o comando `echo`:
> ```bash
> echo "- Rota /about" >> README.md
> ```

Agora, adicione e faça o commit da mudança no `README.md`:

```bash
git add README.md
git commit -m "docs: adiciona rota /about no README"
```

> [!TIP]
> Se você analisar o histórico de commits agora, verá que o commit mais recente é o que adiciona a rota `/about` no `README.md`.
>
> Utliizando o comando `git log --graph --oneline --all`, você verá a representação gráfica do histórico de commits, com as branches e os merges, e poderá ver que a branch `main` agora tem um commit a mais que a branch `feature-3`, ou seja a branch `feature-3` está atrasada em relação à branch `main`.

## Rebase com a branch `main`

### Contextualização

O que fizemos no passo anterior foi adicionar uma nova rota no `README.md` na branch `main`. Isso simula uma situação em que alguém fez uma alteração na branch `main` (ou seja lá qual for a branch principal do seu projeto) enquanto você estava trabalhando em uma branch para uma nova feature.

Nesse cenário, é sempre possível fazer um merge da branch principal na sua branch de feature, mas isso pode gerar um histórico de commits não linear e mais caótico, com vários commits de merge.

Uma alternativa é fazer um **rebase** da branch principal na sua branch de feature. Isso mantém o histórico linear e mais limpo, mas pode gerar conflitos que precisam ser resolvidos. O rebase é uma técnica mais avançada e pode ser mais complexa, mas é uma boa prática para manter um histórico de commits mais limpo. O que ele faz é "recolocar" os commits da branch de feature (que você está trabalhando) no topo da branch principal, ou seja, ele muda o ponto de origem da branch de feature.

No nosso cenário, não haverá conflitos, pois a única mudança foi no `README.md`, que não afeta o código do `index.js`.

### Salvando o histórico de commits antes do rebase

Para acompanhar o rebase e comparar as mudanças, você pode usar o comando `git log --graph --oneline --all` para ver a representação gráfica do histórico de commits. Vamos fazer isso através de uma série de comandos:

```bash
git switch main
mkdir tmp
echo tmp >> .gitignore
git add .gitignore
git commit -m "chore: adiciona diretório temporário"
git log --graph --oneline --all > tmp/before-rebase.txt
```

> [!NOTE]
> Estes comandos criam um diretório temporário chamado `tmp`, adicionam ao `.gitignore`, fazem um commit para atualizar o `.gitignore` e salvam o histórico de commits em um arquivo `before-rebase.txt` dentro do diretório `tmp`.

### Rebase da branch `main` na branch `feature-3`

Agora, vamos fazer o rebase da branch `main` na branch `feature-3`:

```bash
git switch feature-3
git rebase main
```

Se tudo correr bem, você verá algo como:

```plaintext
First, rewinding head to replay your work on top of it...
Applying: feat: adiciona rota /about
```
ou algo como:

```plaintext
Successfully rebased and updated refs/heads/feature-3.
```
ou algo parecido, dependendo do seu sistema operacional e editor de texto. Isso indica que o rebase foi bem-sucedido.

> [!NOTE]
> Se houver conflitos durante o rebase, você precisará resolvê-los da mesma forma que fizemos anteriormente com o merge, porém utilizando o `git add` e o `git rebase --continue` conforme explicado no resultado do comando `git status` - daí sua grande importância.

### Salvando o histórico de commits após o rebase

Para comparar o histórico de commits antes e depois do rebase, você pode usar o comando `git log --graph --oneline --all` novamente. Vamos fazer isso através de uma série de comandos:

```bash
git log --graph --oneline --all > tmp/after-rebase.txt
```

### Visualizando o histórico de commits

Agora, você pode comparar os arquivos `before-rebase.txt` e `after-rebase.txt` para ver as diferenças no histórico de commits antes e depois do rebase.

```bash
cat tmp/before-rebase.txt
```

```bash
cat tmp/after-rebase.txt
```

> [!TIP]
> Você pode usar o comando `cat` para visualizar o conteúdo de arquivos de texto diretamente no terminal.
>
> Nesse caso, você pode alinhar as duas janelas do terminal lado a lado para comparar os arquivos.

### Removendo o diretório temporário

Por fim, remova o diretório temporário:

```bash
rm -rf tmp
```

> [!NOTE]
> Perceba que apagar o diretório `tmp` não causou nenhum impacto ao repositório, pois ele foi adicionado ao `.gitignore` e portanto é ignorado pelo Git.

> [!WARNING]
> Note que isso apagará o diretório `tmp` e todos os arquivos e subdiretórios dentro dele. Tenha cuidado ao usar o comando `rm -rf`, pois ele apaga tudo sem perguntar.

1. Certifique-se de estar na branch `feature-3`.

```bash
git switch feature-3
```

## Integrando a branch `feature-3` na branch `main`

Agora que a branch `feature-3` está atualizada com as mudanças da branch `main`, podemos fazer o merge da branch `feature-3` na branch `main`.

```bash
git switch main
git merge feature-3
```

Se tudo correr bem, você verá algo como:

```plaintext
Updating xxxxxxx..yyyyyyy
Fast-forward
 index.js | 4 ++++
 1 file changed, 4 insertions(+)
```

Isso indica que o merge foi feito sem problemas e que não foi preciso criar um commit de merge, pois a branch `feature-3` estava atualizada com as mudanças da branch `main` e não havia nenhum commit adicional na branch `main`. O cenário era algo como:

```plaintext
A - B - C <- main
         \
          D <- feature-3
```

## Associar o repositório local a um repositório remoto do GitHub

Agora que terminamos de trabalhar no projeto, vamos associar o repositório local a um repositório remoto no GitHub.

### Criar um repositório no GitHub

1. Acesse o GitHub e faça login.
2. No canto superior direito, clique no botão `+` e selecione `New repository`.
3. Preencha o nome do repositório, a descrição (opcional), selecione se será público ou privado, e clique em `Create repository`.

### Adicionar o repositório remoto

Agora, adicione o repositório remoto no seu repositório local:

```bash
git remote add origin <endereco-do-repositorio>
```

Aqui, `<endereco-do-repositorio>` é o endereço do repositório remoto no GitHub. Você pode copiar o endereço do repositório no GitHub e colar no comando acima.

Por exemplo:

```bash
git remote add origin https://github.com/ranierivalenca/atividade-git.git
```

## Fazer o push

Agora você pode fazer o push da branch `main` e das branches de feature (caso deseje) para o repositório remoto:

```bash
git push -u origin main
```

Considerando que você adicionou o endereço do repositório remoto similar ao exemplo acima, vinculando um endereço remoto com o protocolo `https`, você será solicitado a inserir suas credenciais do GitHub.

Se você configurou o Git para usar o protocolo `ssh`, você não será solicitado a inserir suas credenciais.

> [!CAUTION]
> **Nunca compartilhe suas credenciais de acesso ao GitHub.** Se você estiver trabalhando em um ambiente compartilhado, é recomendado que você use um token de acesso pessoal para autenticação.
>
> Você pode criar um token de acesso pessoal no GitHub clicando no seu avatar, selecionando `Settings`, `Developer settings` (uma das últimas opções no menu à esquerda) e, em seguida, `Personal access tokens`. Clique em `Tokens (classic)` e, em seguida, `Generate new token`. Caso apareça outro submenu, selecione `Generate new token (classic)`. Preencha o nome e marque as permissões necessárias (para esta atividade, você pode marcar apenas `repo` para ter acesso a repositórios). Clique em `Generate token` e copie o token gerado. Use esse token no lugar da senha ao fazer o push.
