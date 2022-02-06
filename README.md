## Anotações de comandos e aprendizados
##### **Obs**: Alguns comandos aqui são exclusivos do Linux. Recomendo que use o SO ou WSL2

<details><summary>Git</summary>
<br>
  <details><summary><strong>Sincronizando repo local com o remoto</strong></summary>

  - Se você não tem a chave SSH configurada, é bem tranquilo, só seguir esses comandos:
    - **`ssh-keygen -t ed25519 -C "SEU-EMAIL"`**
    - Aperte *ENTER* nas próximas 3 perguntas
    - **`eval "$(ssh-agent -s)"`**
    - **`ssh-add ~/.ssh/id_ed25519`**
    - **`cat ~/.ssh/id_ed25519.pub`**
    - Copie o resultado do comando anterior, que apareceu no terminal. Essa é a sua chave SSH.
    - Vá até [essa](https://github.com/settings/keys) página, clique em *New SSH key*, coloque o título que quiser, e no campo *key* cole a sua chave. Clique em *Add SSH Key*, e pronto, sua máquina está com a chave SSH configurada.

  - Pra facilitar, crie o repositório no próprio Github. Depois na sua máquina entre na pasta onde vai guardar os repositórios. No terminal digite:  
    **`git clone UrlDoRepo`**  
    **`cd NomeDoRepo`**  
    **`git config user.name "SEU-USERNAME"`**  
    **`git config user.email "SEU-EMAIL"`**  
    **Crie ou edite algum arquivo**  
    **`git add .`**  
    **`git commit -m "Update"`**  
    **`git push -u origin main`**  

    *(sempre que mudar algo como username ou nome do repo, na sua máquina entre na pasta .git de cada repo e faça as alterações no arquivo config)*

  </details>

  <details><summary>Ciclo de vida dos arquivos</summary>

  - **Untracked:** estados em que todos arquivos iniciam. Quando não está rastreado, sincronizado no repo local, no Git.
  - **Tracked:** quando o arquivo está rastreado pelo Git, está sob o controle de versionamento.
  - **Modified:** quando modifica um arquivo já rastreado. O Git te avisa que precisa atualizar o rastreamento.
  - **Staged:** quando o arquivo está pronto pro commit.

  </details>

  <details><summary>Comandos Básicos</summary>

  - **`history -c`** --> Apagar histórico do terminal git/linux.
    - Apagar de forma mais completa: **`cat /dev/null > ~/.bash_history && history -c`**
  - **`git init`** --> Inicializar um repositório.

  - **`git status`** --> Checar o estado dos arquivos do repo.

  - **`.gitignore`** --> Bem auto explicativo, é um arquivo em que você coloca arquivos/diretórios/etc, que você quer que o git ignore. Normalmente usado pra banco de dados, lógica de negócios, autenticações, etc.
    - Para arquivos, coloque o arquivo e extensão, exemplo **`video.mp4`** **`db.sqlite`** etc
    - Para ignorar vários arquivos com a mesma extensão, use **\*** e a extensão, exemplo **`*.sqlite3`**
    - Para diretórios, coloque **\*\*** e o nome do diretório, exemplo **`**videos`** **`**database`**

  - **`git config user.name ""`** --> configurar seu nome de usuário.

  - **`git config user.email ""`** --> configurar email do usuário.
    - Se estiver numa máquina pessoal, de uso exclusivo, utilize **`--global`** depois do **`config`** para que todos projetos comecem com essa configuração padrão.

  - **`git add`** seguido do nome e extensão do arquivo, para adicionar arquivos ao monitoramento do git. **Também** é usado quando você modifica um arquivo.

  - **`git add .`** --> diz pro git tanto pra adicionar arquivos novos pro monitoramento, quanto pra monitorar os modificados.

  - **`git mv arquivo1.extensao arquivo2.extensao`** --> renomeia arquivos. Serve pra diretórios também. Certifique-se de estar no dir correto, e usar **`git mv ./pasta1/ ./pasta2/`**
    > Por que fazer isso pelo git e não pelo terminal normal? Porque quando você faz isso, na verdade o arquivo anterior é apagado, e é criado um nome arquivo, com mesmo conteúdo mas nome diferente. Você tem que adicionar novamente o arquivo, com o novo nome, ao rastreio do Git, e também tem que adicionar o arquivo deletado (??wtf??) com o nome antigo.

    > Renomeando pelo próprio git, o arquivo só muda o nome mesmo, continua rastreado, pronto pro commit. Muito menos dor de cabeça.

  - **`git rm arquivo.extensao`** --> deletar arquivo. **`git rm -rf pasta/`** --> deletar diretório
    > Mas preste atenção, só pode excluir um diretório ou arquivo que já esteja sendo tracked pelo Git, do contrário vai dar erro, pois pra ele "não existe". Ah, e diretórios vazios não são sequer enxergados pelo Git, ele nem dá algum aviso. E portanto não dá pra remover, são untracked.

  - **`git diff`** vem de difference, mostra as diferenças de um estado pro outro, de um commit pro que virá.
    - Você tem que adicionar algo amais, exemplo **`git diff --staged`** para verificar diferença do anterior pro atual.
    - **`git diff hash`** --> verificar a diferença com um commit especifico.
    - **`git diff hash..hash`** para ver a diferença de um commit **até** o outro.

  </details>

  <details><summary>Commit</summary>

  - Um commit é tipo um snapshot do arquivo/algoritmo que está desenvolvendo. É um "okay" pro repo local e informa que o arquivo está pronto para ir pro repo remoto.
    - **`git commit -m ""`** onde **-m** significa a mensagem que aparecerá no commit.

  - Sempre que você fizer um commit, irá gerar um hash id, um identificador, exemplo **`[main 9da4dd5]`**

  - Quando esquecer de mandar certas mudanças pro mesmo commit, ou esquecer arquivos, etc, **antes do push**, você pode usar **`git commit --amend -m "mensagem"`** para fazer essas adições ao último commit.

  - Quando você adiciona um arquivo, deixa ele tracked, mas se arrepende, quer remover do track do Git, **`git restore --staged <file>`**

  </details>

  <details><summary>Log/Histórico</summary>

  - **`git log`** mostra o log de commits, autor, email, timestamp e hash.
    - Quando tem muitos commits, ele reduz a visão no terminal.
    - Você pode usar **`/`** e digitar conteúdo da mensagem do commit para procurar. **`b`** para voltar. **`q`** para sair.
    - (se você quiser fazer com que ele pare de reduzir o log, use **`git config core.pager cat`**
    - (se quiser que volte ao normal, use **`git config core.pager less`**
    - (Lembrando que são configs locais, se quiser de forma global utilize **`--global`** depois do **`config`**)

  - Você pode usar **-** e um número, para informar os últimos commits que quer ver, Ex: **`git log -2`**

  - **`git log --oneline`** mostra as informações de forma reduzida, o hash e mensagem. Inclusive pode combinar isso com o de cima.

  - Você pode procurar por datas, exemplo: **`git log --before="2020-12-13" | git log --after="2020-12-10" | git log --after="2020-12-01" --before="2020-12-12" | git log --since="7 days ago"` |** (Lembrando que também pode mesclar com o ante anterior).

  - Pode pesquisar pelo autor do commit **`git log --author="Gustavo"`**

  </details>

  <details><summary>Checkout</summary>

  > Através do hash id, conseguimos desafazer mudanças. Lembre-se que um commit é um snapshot, uma foto do projeto, você pode entrar naquela foto e voltar pro momento, igual Life is Strange.

  - **`git checkout`** e o hash id, exemplo **`0e1b5fa`**

  - Se você só quiser checar algo e voltar pro futuro, ou se arrepender, pode usar **`git checkout main`**

  - Quando se arrepender de uma mudança em um arquivo, tiver feito merda, **antes dele estar add, monitorado**, pode usar **`git checkout <file>`** que o arquivo voltará ao estado do último commit feito.

  - Pra fazer isso com todo projeto: **`git reset HEAD --hard`**

  - Para fazer isso, depois de ter commitado, (você irá voltar todo projeto pro último commit) **`git reset HEAD^ --hard`**

  - Para voltar todos arquivos pro estado original, do último commit, antes de estarem tracked, **`git checkout -- .`**

  - Para fazer isso com apenas um arquivo **`git checkout -- <filename>`**

  - Para fazer isso depois do arquivos estarem tracked: **`git checkout HEAD -- .`**

  - Para fazer isso com apenas um arquivo **`git checkout HEAD -- <filename>`**

  </details>

  <details><summary>Revert e Reset</summary>

  - **Revert**: não desfaz um commit, ele reverte o que foi feito e criando um novo commit. Reverte. **`git revert <HashDoCommit>`**
    - Não esqueça de dar o **push** pro commit ir pro bare.

  - **Reset:** remove commits. **`git reset HEAD~1`**
    - **`git push -f -u origin main`**

  </details>

  <details><summary>Branchs</summary>

  > Quando você cria um projeto no git, você tem seu **branch main**, que seria o **tronco** da árvore. É perigoso ficar commitando no tronco, pois se fizer algo errado, vai estragar toda árvore. Por isso você tem o conceito de **branchs secundárias**, que seriam os **galhos**, as **ramificações**. Então você está lá desenvolvendo certa **feature** do projeto, se ela der errado, você simplesmente joga o galho fora, corta ele. Mas se der certo, você faz um **merge**, **junta** o galho ao tronco, junta a branch secundária com a feature para a branch main.

  - **`git branch`** retorna quantas branchs existem e em qual branch você está (em verde e com um asterisco *) 

  - Para criar uma branch é bem simples **`git branch NomeDaBranch`**

  - Alternar entre branchs --> **`git checkout NomeDaBranch`**
    - (Se você quiser economizar tempo, pode criar e já alternar pra branch, com um comando só: **`git checkout -b NomeDaBranch`**)

  - Excluir uma branch --> **`git branch -d NomeDaBranch`** 
    - Se a branch que vai ser excluída não foi fundida com outra em algum momento, o git vai perguntar se quer mesmo excluir, aí tem que rodar o mesmo comando, mas em caps o **`-D`**

  - Pra dar um **merge** você alterna pra branch que vai *absorver a outra* (normalmente a main) e digita **`git merge NomeDaBranchAbsorvida`**
    - (Lembrando que após o merge, a branch absorvida não desaparece, ela continua viva e independente). Ah, e quando tal branch recebe o merge, ela absorve também os commit feitos, todo log etc

  - **Rebase** faz quase a mesma coisa que **merge**, mas deixa os commits em ordem, reoorganiza a ordem de todos commits do projeto. **`git rebase NomeDaBranch`**
    - Não é super indicado, principalmente em pair programming e em empresa. É até legal para projetos pessoais, mas melhor não usar.

  </details>

  <details><summary>Clone, Push, Fetch, Pull e Tag</summary>

  - Pra clonar um repositório --> **`git clone urlDoRepo .`** (o ponto indica pra clonar dentro do repo que está)
    - Depois de clonar, entre no repo e configure seu usuário.

  - O **push** "empurra" pro repo remoto, o bare. **`git push -u origin main`** --> envia seus commits pro repo central

  - O **fetch** baixa os arquivos, mas sem trackear. **`git fetch`** aí depois tem que usar o git rebase pro arquivo organizar os arquivos e commits **`git rebase`**
    - Método menos utilizado.

  - O **pull** faz isso acima em uma tacada só **`git pull origin main`** (vai abrir um editor de código, só digitar ^O + enter + ^X)

  - A **tag** é um estado da aplicação, como se fosse um release, a versão. **`git tag versaoTal`**
    - Mas por enquanto isso só está no repo local. Para mandar pro repo remoto, para que todos users saibam da release **`git push origin versaoTal`**
    - Inclusive, você pode alternar para tags, para "dar uma olhada", igual faz em branchs. **`git checkout versaoTal`**
    - Você pode usar isso pra criar uma branch a partir de tal tag, tpo pra corrigir bugs de tal versão, etc. **`git switch -c <new-branch-name>`**

  - **Bare repository**: Significa repositório central, remoto. Lembrando que o git é descentralizado, mas é comum que tenhamos um repositório central, ainda mais quando trabalhamos em equipe.

  </details>

  <details><summary>Issue, Fork e Pull Request</summary>

  - **Issue:** quando uma pessoa acha um problema em um projeto seu, pode reportar uma **issue**. Você também pode fazer isso com os outros. Mas quando reportar uma issue, pesquise bem antes, pra não criar uma que já foi resolvida.
    - Dá pra fechar uma issue no commit, dentro da mensagem dele, no final coloque **`Closes #IssueID`**

  - **Fork:** normalmente você forka um projeto pra resolver uns bugs ou melhorar e dar pull request, ou também quando quer criar algo novo com base naquele.

  - **Pull request:** é uma requisição para que o owner aceite as alterações feitas no se fork para o bare. Você também pode passar no título do pull request **`Closes #IssueID`** para que além de aceitar, fechar uma issue dele.
    - É uma boa prática ao invés de dar um merge com pull request, você dar um fetch (lembrando que o fetch baixa mas sem fundir), pra testar se realmente está tudo certo.  **`git fetch origin pull/IdPullRequest/head:NomeDaBranch`**
    - Aí você olha o log, verifica o arquivo mexido, se está legal. E então vai no github e confirma o merge do pull request.

  </details>

  <details><summary>Gist</summary>

  Pequenos trechos de códigos que você cria pra você mesmo ou outras pessoas. Snippets.

  Para usar facilmente com frequência.

  Permite o compartilhamento de pequenos trechos de código. Há também quem use o Gist para receber feedbacks daquele código específico. Também pode publicar parte do seu código e usar o plugin do Gist para mostrar seu código em sites, fóruns e outros locais. Para isso, só precisa publicar o código (depois de logar no GitHub) e clicar em “Show Embed” e ele lhe mostrará um código javascript para colar onde quiser. Onde você colar o javascript vai aparecer uma caixinha bonitinha com o trecho de código e um link para o seu Gist. Alterando seu Gist, todos os lugares onde você publicou seu código serão alterados ao mesmo tempo.

  </details>

</details>

<details><summary>JS</summary>
<br>

  - `document.querySelector('ELEMENTO/ID/CLASS')` para elementos individuais
  - `document.querySelectorAll('ELEMENTO/ID/CLASS')` para elementos múltiplos
    - Usar o `foreach` quando for iterar
  - Pra capturar eventos `addEventListener('click', () => { COMANDOS })`
    - Outros eventos comuns: `mousemove`, `mouseout`, `mouseenter`, `mouseleave`
  - Para alterar uma classe `ELEMENTO.classList.contains('CLASS') ? ELEMENTO.classList.remove('CLASS') : ELEMENTO.classList.add('CLASS')`
  - Usar `$` nas variáveis que "puxam" HTML
  - Sempre que possível colocar `const` ao invés de `let`
  - Checar o *false* primeiro no condicional
  - Funcionamento de um **foreach**: 
  ```
  ELEMENTOS.forEach((e, index) =>
    e.innerHTML = `Número ${index+1}`
  )
  ```
  - Checar o *false* primeiro no condicional
  - **this** pro primeiro escopo anterior, mais que isso tem que dar a volta
  - Onde tem **await** tem **async**. E quando usar uma função que tem async/await, tem que transformar o código que está chamando também em **await** **async**

 </details>

<details><summary>NodeJS, ExpressJS, EJS</summary>
<br>  

  - **Instalação** do NodeJS:
    `sudo apt install wget`
    `wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash`
    `source ~/.profile`
    Mostrar todas as versões disponíveis: `nvm ls-remote`
    Por motivos de estabilidade baixe a versão LTS mais atual: `nvm install --lts`
    Para verificar as versões do NodeJS e NPM: `node -v` `npm -v`
    Para desinstalar: `nvm deactivate` e depois `nvm uninstall --lts`
  <br>  

  - `npm init -y` pra iniciar um projeto
  - `npm i {package}` pra baixar um pacote, exemplo o *Express* `npm i express`
    - Se passar no final o parâmetro `-D` você está dizendo pro npm que essa depedência não é crucial, a aplicação funciona sem ela, é só pra fim de **desenvolvimento**.
  - Sempre colocar no arquivo *.gitignore* a pasta *node_modules*
  - `npm uninstall {package}` pra deletar um pacote
  - Quando você clonar um repositório, para que todos pacotes do NodeJS funcione, rode no terminal `npm i`
  - Use o *Nodemon* pra não precisar toda hora atualizar o server manualmente.
    - Instalando `npm i nodemon -D`, já que é só pra fim de nos ajudar no desenvolvimento.
    - No package.json em *main* aponte pro arquivo do servidor; e em *scripts*, adicione `"dev": "nodemon ."`
    - No terminal rode `npm run dev` (dev se refere ao script adicione alteriormente).
  - `require` pra importar uma função de outro arquivo (o qual precisa do `module.exports = {função}`)
    - Se for passar mais de uma função, melhor criar um objeto com várias funções
  - `ctrl + c` pra parar o servidor
  - Com **ExpressJS** você escreve menos código do que com NodeJS puro, é mais enxuto e escalável
  - Nem sempre sabemos em que porta a aplicação está rodando, então guardamos numa constante a porta, indepedente de qual seja: `const port = process.env.PORT || 8080`
  - O Express/Node é meio burrinho praa char o caminho de um diretório, então você precisa utiliza a lib *path*
  - Por padrão **forms** utilizam o método Get.
    - O atributo *name* no **form** é o que dá nome as propriedades usadas na requisição
  <br>  

  - **Arquitetura de Projeto**: cada arquivo/pasta tem que ter seu papel bem definido. Isso ajuda a não ficar com arquivos com centenas ou milhares de linhas, também economiza tempo quando for fazer manutenção, por já saber onde cada coisa está. Deixar tudo separadinho, de acordo com sua "responsabilidade": rotas, models, views, controllers, etc.
  - Padrão **MVC** (model - dados, view - visualização, controller - gerenciador dos dados)
  - É uma convenção ter uma pasta public, para imagens, styles, scripts front, etc, coisas que podem ser públicas e que *não vão mudar com muita frequência*.
  - EJS é uma engine de visualização, com ele conseguimos de uma maneira fácil e simples transportar dados do back-end para o front-end, basicamente conseguimos utilizar códigos em javascript no html de nossas páginas.
  - `<%- include('{partial}') %>` pra inserir uma partial `<% {código} %>` pra inserir código `<%= {variável} %>` pra inserir um valor
    - Esse valor antes tem que ser enviado pela rota dentro do render
    - Se esse valor o JS tiver HTML dentro, você precisa fechar o EJS antes de começar o HTML, e abrir de novo quando começar o JS de novo
  - Para tornar um parâmetro opcional na rota coloque `?`, exemplo: `router.get('/products/:id?', ProductsController.get)`.
    - Nesse tipo de parâmetro se usa o `req.params`
    - Na QueryString `?id=123` se usa o `req.query` no GET
    - No POST se usa o `req.body`

  </details>

<details><summary>API - Restful</summary>
<br>

  - O **Server API** fica responsável apenas por fornecer dados (em JSON) quando o usuário fazer a requisição, não em entregar os arquivos static, que já são entregues no começo (HTML, CSS e Javascript)
    - O Servidor se torna mais independente, você pode ter quantas aplicações client quiser se conectando com o servdiro.
  - **Rest** é um padrão de comunicação, pois ambas aplicações utilizando o server precisam falar a mesma língua
    1. **Client- Server**: client side e server side totalmente independentes
    2. **Stateless**: cada requisição que o client fizer pro server, tem que conter todas informações/recursos necessários para que o servidor entenda e consiga entregar a resposta.
    3. **Cacheable**: cada requisição que o client fizer, o server tem que ser explícito e responder se ele pode ou não cachear aquela informação (guardar no cache a informação)
    4. **Layered System**: sistema de camadas. Temos que ter endpoints (rotas) para se comunicar com o server. Garante também que o usuário não precise entender o quão complexo foi para que a requisição fosse atendida.
  - **Restful** é a aplicação completa de todos padrões Rest.
  - **Verbos HTTP** (métodos):
    1. **GET**: obter dados
    2. **POST**: enviar dados (visão do client) | receber dados (visão do server)
    3. **PUT**: atualizar dados
    4. **DELETE**: remover dados
  - **CORS**: é o mecanismo que gerencia se outros domínios, fora do domínio ao qual pertence o recurso (ex: API), podem fazer requisições.
    - `app.use(cors())` habilita pra qualquer domínio (tipo API's públicas)
    - Pra habilitar um domínio específico `app.use(cors({origin: 'http://127.0.0.1:5500'}))`
    - Mas se quiser vários em específico é assim:
      ```javascript
      const allowedOrigins = ['http://127.0.0.1:5500', 'http://localhost:5500']

      app.use(cors({
        origin: function(origin, callback) {
          let allowed = true
          
          // permitir requests sem origem (tipo mobile apps e curl)
          if(!origin) allowed = true

          if(!allowedOrigins.includes(origin)) allowed = false

          callback(null, allowed)
        }
      }))
      ```

</details>

<details><summary>CSS</summary>
<br>

  - Parentescos:
    - **`>`** diz que a regra tem que ser aplicada somente aos filhos da classe
    - **`+`** aplica a regra pro primeiro irmão direto
    - **`~`** aplica a regra pra todos irmãos diretos
  - Quando usar o `display: inline-block;`? quando precisa que fique na mesma (igual o inline) mas precisa acessar as propriedades height e width
  - `position: absolute;` é relativo ao body, se quiser que ele seja relativo ao pai, tem que colocar `position: relative;` no pai dele
  - `:root` é normalmente usado para se guardar variáveis
  - Variáveis são declaradas assim `--variavel-etc: #fff;` e usadas assim `color: var(--variavel-etc);`
    - Alguns padrões: `--color/background/font-primary` `--color/background/font-secondary`
  - `*` aplicador universal, aplica as propriedades em tudo que conseguir
    - Alguns padrões: `box-sizing: border-box;`, `margin: 0;`, `padding: 0;`, `font-family: sans-serif;`
  - `box-sizing: border-box;` significa que todas box não vão extrapolar o box-model ![Box Model](./img/box-model.png)
  - Para importar um arquivo, fonte, etc `@import url('inserir aqui');`
  - [CSS Gradient](https://cssgradient.io/)
  - Efeitos de "sumir":
    - `display: none;` faz o elemento desaparecer e desocupa o espaço dele
    - `visibility: hidden;` faz o elemento desaparecer e mantêm o espaço dele
    - `opacity: 0;` faz o elemento ficar transparente e mantêm o espaço dele
  - Aquele **menu hambúrguer** é "empurrado" atráves do **position** ou **margin**. Não se usa muito `display: block` porque esse não permite efeito de transition, fica "seco"
    - Também se usa `overflow-x: hidden;` pra esconder esse menu que está "empurrado"
  - `transition: all 300ms ease;` `transition: background-color 300ms ease;`
  - Criar animação exemplo:
  ```
  @keyframes animação {
    0% {
      transform: rotateX(0deg);
    }
    100% {
      transform: rotateX(-90deg);
    }
  }
  ```
  - Usar a animação `animation: animação 300ms ease` 
  - Pra adicionar conteúdo em um elemento através do css `content: '';`
  - Responsividade exemplo
  ```
  @media (max-width: 550px) {
    .gallery.active div {
      width: 90%;
    }
    .seasons button {
      margin: 5px 10px;
    }
  }
  ```
  - O Flex é aplicado na box pai ![Flex](./img/flex.png)
  - [Flexbox Froggy](https://flexboxfroggy.com/)
  - [Flexbox Defense](http://www.flexboxdefense.com/)
  - `align-items:` alinha na vertical. Só funciona com o `flex-direction: row;` que é o padrão do direction
  - Quando o flex direction é `column`, o _justify-content_ muda para a vertical e o _align-items_ para a horizontal
  - Para alinhar um elemento individual em uma ordem específica na horizontal, use a propriedade `order`. Por padrão começa em zero e também aceita negativo
    - Na vertical use o `align-self`, lembrando da regra do _flex-direction_
  - `align-content:` alinha quando você tem o wrap, lembrando da regra do _flex-direction_
  - Flex Direction ![Direction](./img/direction1.jpg) ![Direction](./img/direction2.jpg)
  - Flex Wrap ![Wrap](./img/wrap.jpg)
  - Justify Content ![Justify Content](./img/justify-content1.jpg) ![Justify Content](./img/justify-content2.jpg) ![Justify Content](./img/justify-content3.jpg) ![Justify Content](./img/justify-content4.jpg) ![Justify Content](./img/justify-content5.jpg)

</details>

<details><summary>Settings VSCode</summary>
<br>
  
  - Para instalar a fonte FiraCode, no terminal rode: `sudo apt update && sudo apt install fonts-firacode`

  ```json
  {
    "workbench.iconTheme": "material-icon-theme",
    "workbench.colorTheme": "Ayu Dark Bordered",
    "editor.fontFamily": "Fira Code",
    "editor.fontSize": 14,
    "editor.fontLigatures": true,
    "window.zoomLevel": 1,
    "editor.bracketPairColorization.enabled": true,
    "editor.guides.bracketPairs": true,
    "editor.guides.bracketPairsHorizontal": true,
    "editor.guides.highlightActiveBracketPair": true,
    "workbench.colorCustomizations": {
      "editorBracketHighlight.foreground1": "#e6a939",
      "editorBracketHighlight.foreground2": "#24a4e6",
      "editorBracketHighlight.foreground3": "#bb80b3",
      "editorBracketHighlight.foreground4": "#b7e86d"
    },
    "editor.minimap.enabled": false,
    "workbench.startupEditor": "none",
    "workbench.editor.labelFormat": "short",
    "breadcrumbs.enabled": false,
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "prettier.printWidth": 100,
    "prettier.semi": false,
    "prettier.singleQuote": true
  }
  ```

</details>
