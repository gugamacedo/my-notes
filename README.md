## Anotações de comandos e aprendizados

###### Obs: Alguns comandos aqui são exclusivos do Linux

<details><summary><img src="https://img.shields.io/badge/React-08081d.svg?style=flat-square&logo=react&logoColor=%2361DAFB" alt="React" style="height: 20px;"> <img src="https://img.shields.io/badge/React_Router-CA4245?style=flat-square&logo=react-router&logoColor=white" alt="React Router" style="height: 20px;"> </summary>
<br />

- No React para iniciar um projeto é `npx create-react-app nome-do-projeto`
- Um **component** React é uma função Javascript que retorna HTML (JSX)
  - Pra vias de organização, **component** é somente algo que recebe uma informação e exibe na tela. Não é algo que gerencia um determinado estado da minha aplicação.
  - Uma organização comum de pastas: `components` `pages` `partials` `templates`
- Componentes React são em UpperCase. Estrutura básica de um component React:

  ```Javascript
  import React from 'react' // importando o React, ele é uma lib, não framework

  // A função com o nome do arquivo, retornando HTML
  function App() {
    return (
    <div>
      <h1>Hello World</h1>
    </div>
    )
  }

  export default App // habilitando para importação
  ```

- Depois só importar e usar o component como tag: `<App />`
  - Se esse component for ter filhos, colocar assim: `<App> Conteúdo </App>`
- No retorno sempre tem que ter um elemento pai. Se não tem pai, pode usar o React fragment: `<></>`
- Com **props** é possível passar propriedades personalizadas, por parâmetros de função nas tags HTML pro JSX.
  - É preciso desestruturar porque ele vem como um objeto no parâmetro da função
  - Se quiser pegar o filho de um component, no _props_ tem a propriedade `children`
- Comandos como o `innerHTML` não funcionam, porque o JSX retorna um objeto JS, não HTML. Nesse caso tem que usar o `appendChild()`
- O React não renderiza na página o código HTML, já que ele está em JSX. Isso prejudica o SEO do site, o Google não vai achar nada. Pra isso serve o framework NextJS, que é um framework para React, para fazer a renderização estática e pelo lado do servidor.
- O `class` do HTML, no JSX é `className`
- O _css_ tem que ser um arquivo pra cada component, e também em UpperCase
- Quando está usando o `export const` (não o `export default`) na hora do _import_ tem que ser entre `{}`
- **useState**: quando você quer alterar o estado (_state_) de um component, precisa utilizar o useState.
  1. Importe ele junto com o React `{ useState }`
  2. `const [initialValue, setNewValue] = useState(estado inicial)` o primeiro parâmetro é a variável de valor inicial, que será utilizada como estado inicial no começo da aplicação. O segundo parâmetro é a variável do novo valor/estado, que vai fazer as atualizações. (Ambas variáveis são `const`). Dentro do `useState()` fica o valor inicial, que vai entrar no `initialValue`.
  3. Dentro do `handler` ou `listener` você coloca o `setNewValue(newValue)`. A variável `newValue` é só pra legebilidade, você poderia colocar o nome valor, ou a lógica diretamente aí.
- **useEffect**:

  - Recebe dois parâmetros. No exemplo de código, toda vez que a variável `count` tem o _state_ alterado, executa o `useEffect` que altera o `title` da página pro `count`.
  - **Obs**: o `useEffect` tem um `return` opcional. Ele serve pra dizer o que fazer quando o _component_ for "desmontado", quando ele deixar de existir

  1. Uma função de callback que executa o que você quer
  2. Um array de depedências. Se estiver vazio, então só executa uma vez. Se tiver uma variável, executa toda vez que a depência é alterada. Por ser um array, pode colocar múltiplas dependências.

  ```Javascript
  useEffect(() => {
    document.title = count

    return () => document.title = 'React App'
  }, [count])
  ```

- **React Router** é uma biblioteca que cuida das rotas/navegação, em aplicações React. Instalação `npm install react-router-dom`. Estrutura básica da declaração das rotas:

  ```Javascript
  // importando os component necessários
  import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'

  <Router>
    <Switch>
      <Route exact path="/">
        <Home />
      </Route>
      <Route exact path="/about">
        <About />
      </Route>
      <Route exact path="/contact">
        <Contact />
      </Route>
    </Switch>
  </Router>
  ```

  - Tem que colocar duas configurações no index.html: `<meta name="viewport" content="initial-scale=1, width=device-width" />` e `<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" />`
  - Na página onde ficará o menu de navegação:

  ```Javascript
  // importando os componentes necessários
  import { Link } from 'react-router-dom'

  <ul>
  // Repare que não se usa "a href" e sim "Link to"
    <li><Link to="/">Home</Link></li>
    <li><Link to="/users">Usuários</Link></li>
  </ul>
  ```

  - **Obs:** uma coisa bem legal é que o component **Link to** não vai até o servidor buscar a página, tanto que a página nem recarrega. É como se ele "escondesse" a página atual, e mostrasse a nova. Ao contrário do `a href` que manda a requisição pro server e retorna pro client. Com o **`Link to`** tudo acontecesse do lado do próprio client.
    E se você estpa se perguntando "mas as rotas não são feitas no backend com o **Node**?" Primeiro que se não tiver back, isso já nem importa. Segundo que no caso da estrutura de nossos projetos, sempre faremos a **API-Restful** separada do front, fazendo requisições pelo frontend da aplicação. Então nesse caso as rotas podem perfeitamente serem feitas no frontend, mesmo existindo backend.

- Uma lib para upload de images/arquivos (no front) muito usada é a [React Dropzone](https://react-dropzone.js.org/)
  - Para instalar `npm i --save react-dropzone`. Importando o **hook** `import { useDropzone } from 'react-dropzone'`
  ```Javascript
  const [files, setFiles] = usestate([])

  const { getRootProps, getInputProps } = useDropzone({
    accept: 'image/*', // tipo de arquivo permitido e extensões
    onDrop: (acceptedFile) => {
      const newFiles = acceptedFile.map(file => {
        return Object.assign(file, {
          preview: URL.createObjectURL(file) // criando link pro preview dos files
        })
      })
  
      setFiles([ // adiciona as imagens já existentes no objeto com as novas
      ...files,
      ...newFiles,
      ])
   
    }
  
  })
  ```
  - Depois vai no box/div que vai aceitar a **dropzone** e insira um objeto com um array de props (para a lib fazer o controle). Ex: 
  ```HTML
  <Box className={classes.dropzone} {...getRootProps () }>
    <input {...getInputProps()} />
  </Box>
  ```
  - Para fazer uma feature de **remover imagem**, coloque um handle no botão/icone, levando como parâmetro o `file.name`:
  ```Javascript
  const handleRemoveFile = fileName {
    const newFileState = files.filter(file => file.name !== fileName)
    setFiles(newFileState)
  }
  ```
- Uma lib para carrossel de imagens muito utilizada é a [React MUI Carousel](https://www.npmjs.com/package/react-material-ui-carousel)
  - Para instalar `npm install react-material-ui-carousel --save`
    - Ela depende das libs `@mui/material` `@mui/icons-material` `@mui/styles`
  - Import `import Carousel from 'react-material-ui-carousel'`
  - O uso é bem fácil, a documentação é ótima e tranquila
</details>

<details><summary><img src="https://img.shields.io/badge/Material.UI-%230081CB.svg?style=flat-square&logo=mui&logoColor=white" alt="MUI" style="height: 20px;"> </summary><br />

  - **Material.UI** é uma biblioteca com components prontos e estilizados, para aplicações React, baseado no tema _Material_ da _Google_. Link: [mui.com/pt/components/](https://mui.com/pt/components/)
  - Instalação `npm install @mui/material @mui/icons-material @mui/styles @emotion/react @emotion/styled`
  - A biblioteca `icons-material` não permite desestruturação
- **Estudar bastante as props de cada component**
- **useStyles**: para aplicar CSS dentro do JS 🤯🤯🤯 Se o CSS for grande, normalmente se cria uma **pasta** pra cada component que será estilizado, com um arquivo pro component e outro pro estilo dele, ex: `Header/Header.js` e `Header/Header.style.js`

  - No arquivo do **component style**:

  ```Javascript
  import { makeStyles } from '@mui/styles'

  const useStyles = makeStyles(() => ({
    // declarando os filhos como objetos vazios no começo
    span: {}, 
    word1: {},
    word2: {},

    phrase: {
      fontWeight: 'bold', // se a propriedade CSS tive traço - colocar em camelCase

      '&:hover': { // bem parecido com o SCSS, mas com aspas
        color: 'green'
      },

      '& span': { // como é um elemento, não precisa do cifrão $
        color: 'blue'
      },

      '& $word2': { // como é uma classe, precisa do cifrão $
        color: 'red'
      },

      '&:hover $word1, &:hover $word2': { // aplicando pra múltiplos elementos
        color: 'yellow'
      }
    }
  }))

  export default useStyles
  ```

  - No arquivo do component:

  ```Javascript
  import useStyles from './Header.style'

  const Header = () => {
    const classes = useStyles()

    return (
      // e dentro do componente colocar "className={classes.title}"
    )
  }
  ```

- Quando for colocar seletores como o `hover` ou outros, podefazer igual no **SCSS**, só que tem que colocar entre aspas
  - E no caso de elementos filhos além das aspas, tem que declarar o filho como um objeto vazio **{}** no começo do **useStyles**, e se for uma classe tem que colocar um cifrão **$** antes do nome da classe, se não ele vai achar que é um elemento.
- Em alguns components, tipo o **Grid** ou **Container**, etc, você pode usar como **props** propriedades de responsividade de forma bem simples:
  - **XS**: extra small (até 576px)
  - **SM**: small (até 768px)
  - **MD**: medium (até 992px)
  - **LG**: large (até 1200px)
  - **XL**: extra large (até 1400px)
  - **XXL**: extra extra large (maior que 1400px)
- No **Grid** você consegue passar também propriedades de **flex** como **props**
- Para fazer o **`@media query`** (responsividade), tem o `theme.breakpoints` no hook **useStyles**. **Obs:** tem que colocar em ordem por tamanho do maior pro menor, se não vai bugar. Um exemplo de uso:
  ```Javascript
  import { makeStyles } from '@mui/styles'

  const useStyles = makeStyles((theme) => ({ // pegando o theme
    cards: {
      display: 'grid',
      gridTemplateColumns: '1fr 1fr 1fr',
      gap: 30
    },

    [theme.breakpoints.down(1100)] : { // pode colocar um número se não quiser usar as props de medida
      cards: {
        gridTemplateColumns: '1fr 1fr',
      } 
    },

    [theme.breakpoints.down('xs')] : {
      cards: {
        gridTemplateColumns: '1fr',
      } 
    }
    
  }))

  export default useStyles
  ```
  - Para fazer o `@media query` na **prop sx**é bem parecido com o CSS normal: 
  ```Javascript
  <Container
    sx={{
      width: '50%',

      '@media screen and (max-width: 768px)': {
        width: '90%',
      },

    }}
  /> 
  ```
  - Lembrando que um dos princípios no React é a **modularização**, então não é uma boa ideia fazer um arquivo style JS com a responsividade de toda aplicação
- É uma boa prática organizar o código na seguinte ordem: definições de hooks, depois states, os useEffect, e por fim os Handle.
- **CSS module**: uma maneira alternativa de fazer o CSS no React. Basicamente todo arquivo de CSS terá um `.module` antes de `.css` e no arquivo JS o import será assim: `import style from './Algo.module.css'`. E na hora de definir o _className_ será um objeto: `className={style.classe}`
- **Styled Components**: traduzindo **Componentes estilizados**. É simplesmente isso hahahaha Você faz o CSS dentro do JS, no mesmo arquivo do component. Pra utilizar tem que rodar no terminal `npm install --save styled-components`. Depois no arquivo do component você importa assim `import styled from 'styled-components'`. Depois cria uma `const` com o nome do componente que será estilizado (components sempre em letra maiúscula), ex abaixo, e usa o componente normalmentecomo tag, podendo abrir, passar props, usar propriedades do próprio elemento HTML, etc.

  ```Javascript
  const Square = styled.div`
    width: 200px;
    height: 200px;
    background-color: violet;
  `

  <Square>
    <span>teste</span>
  </Square>
  ```
- Quando está colocando o `color` nas props de Typography, tem que ser `textPrimary` ao invés de `primary`
  
</details>
  
<details><summary><img src="https://img.shields.io/badge/Next-black?style=flat-square&logo=next.js&logoColor=white" alt="Next" style="height: 20px;"></summary>
<br />

- No Next para iniciar um projeto é `npx create-next-app nome-do-projeto`
- Ele fazer o SSR (server side rendering - renderização do lado do servidor). Lembra que no React a indexação fica prejudicada porque ele não renderiza todo HTML pro client? O Next resolve isso!
- Todos componentes, páginas, partials , templates, que fizer no Next continuam sendo feitos em React.
- O Next faz sozinho o **sistema de rotas** \*-\* Ele aproveita a pasta **pages** e roteia os arquivos pelos nomes.
  - Se você quiser fazer subníveis, é só criar uma pasta com o nome da página de nível 1 e um arquivo `index.js` pra página. Aí a página de nível 2 ao lado, mas com o nome da página correspondente. Ex: `products/index.js` e `products/glasses.js`
  - No componente que terá os links você tem que usar o componente próprio do Next para links:
    - Importando: `import Link from 'next/link'`
    - Usando: `<Link href="/products"> <a> Todos produtos </a> </Link>` ou `<Link href="/products/glasses"> <a>Óculos </a> </Link>`
      - Caso você não queria usar um `<a>` como link e sim outro elemento tipo um botão, ou até um component do MUI, tem que colocar a prop **`passHref`** dentro do `<Link>`. Ex: `<Link href="/login" passHref> <Button> Login </Button> </Link>`
  - O título dos arquivos tem que ser tudo minúsculo
- **Rotas dinâmicas:** se você colocar o nome do arquivo entre `[ ]` você pode usar o hook **useRouter** que permite a url receber uma query diferente do nome dela. Continuando do exemplo acima, a página de _óculos_ além de receber na url _glasses_, pode receber um **id** tipo 13579, ficando `localhost/products/13579`. Isso vindo numa estrutura **chave: [valor]**, ex `glasses: ['glasses']`. Para habilitar você roda `import { useRouter } from 'next/router'`, depois para acessar `router.query.glasses`.
  - Também é possível habilitar para receber depois da barra, fazendo um **spread** no nome do arquivo, no começo, dentro do `[ ]`. Ficaria `localhost/products/glasses/13579`
- Você pode customizar o arquivo **`_app.js`**, se quiser adicionar um Template Global por exemplo, ou components globais (um menu por exemplo). Também dá pra customizar o arquivo **`_document.js`**, adicionando coias que faltam no html, por exemplo tags na _Head_, tags de _meta_, tags de _script_, etc.
- Na pasta **`src`** colocaremos: *theme*, *components*, *contexts*, *utils/helpers*, *templates*, *controllers*, *models*, etc.
- O Next.js pode servir arquivos estáticos, como imagens, na pasta **public** no diretório raiz. Arquivos dentro de *public* podem ser referenciados pelo seu código a partir da URL base (/). Por exemplo, se você adicionar uma imagem a `public/logo.png`, o código a seguir acessará a imagem `<Image src="/logo.png" alt="logo" width="64" height="64" />`

</details>

<details><summary><img src="https://img.shields.io/badge/Node-1c562b?style=flat-square&logo=node.js&logoColor=white" alt="Node" style="height: 20px;"> <img src="https://img.shields.io/badge/MongoDB-%23107C10.svg?style=flat-square&logo=mongodb&logoColor=white" alt="MongoDB" style="height: 20px;"> <img src="https://img.shields.io/badge/Express-000000.svg?style=flat-square&logo=express&logoColor=whit" alt="Express" style="height: 20px;"> <img src="https://img.shields.io/badge/<‰%20EJS-a91e50.svg?style=flat-square&logoColor=white" alt="EJS" style="height: 20px;"></summary>
<br />

- **Instalação** do NodeJS. Primeiro verifique se você possui o **[curl](https://curl.se/)** instalado rodando no terminal o comando: `curl --version`
  - Caso ele retorne a versão, pode pular para o próximo passo. 
  - Caso não, basta rodar o comando: `sudo apt install curl`
  - Usando NodeSource:
    - Com o **curl** instalado, execute o comando de instalação da versão LTS mais recente disponível: `curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs`
  - Usando NVM:
    - Com o **curl** instalado, execute o comando `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash`
    - `source ~/.profile`
    - Mostrar todas as versões disponíveis: `nvm ls-remote`
    - Por motivos de estabilidade baixe a versão LTS mais atual: `nvm install --lts`
  - Feche o terminal e abra novamente para as alterações fazerem efeito.      
  - Para verificar se o **Node** e **NPM** estão instalados rode `node -v` e `npm -v`
- `npm init -y` pra iniciar um projeto
- `npm i {package}` pra baixar um pacote, exemplo o _Express_ `npm i express`
  - Se passar no final o parâmetro `-D` você está dizendo pro npm que essa depedência não é crucial, a aplicação funciona sem ela, é só pra fim de **desenvolvimento**.
- Sempre colocar no arquivo _.gitignore_ a pasta _node_modules_
- `npm uninstall {package}` pra deletar um pacote
- Quando você clonar um repositório, para que todos pacotes do NodeJS funcione, rode no terminal `npm i`
- Use o _Nodemon_ pra não precisar toda hora atualizar o server manualmente.
  - Instalando `npm i nodemon -D`, já que é só pra fim de nos ajudar no desenvolvimento.
  - No package.json em _main_ aponte pro arquivo do servidor; e em _scripts_, adicione `"dev": "nodemon ."`
  - No terminal rode `npm run dev` (dev se refere ao script adicione alteriormente).
- `require` pra importar uma função de outro arquivo (o qual precisa do `module.exports = {função}`)
  - Se for passar mais de uma função, melhor criar um objeto com várias funções
- `ctrl + c` pra parar o servidor
- Com **ExpressJS** você escreve menos código do que com NodeJS puro, é mais enxuto e escalável
- Nem sempre sabemos em que porta a aplicação está rodando, então guardamos numa constante a porta, indepedente de qual seja: `const port = process.env.PORT || 8080`
- O Express/Node é meio burrinho praa char o caminho de um diretório, então você precisa utiliza a lib _path_
- Por padrão **forms** utilizam o método Get.

  - O atributo _name_ no **form** é o que dá nome as propriedades usadas na requisição
    <br />

- **Arquitetura de Projeto**: cada arquivo/pasta tem que ter seu papel bem definido. Isso ajuda a não ficar com arquivos com centenas ou milhares de linhas, também economiza tempo quando for fazer manutenção, por já saber onde cada coisa está. Deixar tudo separadinho, de acordo com sua "responsabilidade": rotas, models, views, controllers, etc.
- Padrão **MVC** (model - dados, view - visualização, controller - gerenciador dos dados)
- É uma convenção ter uma pasta public, para imagens, styles, scripts front, etc, coisas que podem ser públicas e que _não vão mudar com muita frequência_.
- EJS é uma engine de visualização, com ele conseguimos de uma maneira fácil e simples transportar dados do back-end para o front-end, basicamente conseguimos utilizar códigos em javascript no html de nossas páginas.
- `<%- include('{partial}') %>` pra inserir uma partial `<% {código} %>` pra inserir código `<%= {variável} %>` pra inserir um valor
  - Esse valor antes tem que ser enviado pela rota dentro do render
  - Se esse valor o JS tiver HTML dentro, você precisa fechar o EJS antes de começar o HTML, e abrir de novo quando começar o JS de novo
- Para tornar um parâmetro opcional na rota coloque `?`, exemplo: `router.get('/products/:id?', ProductsController.get)`. - Nesse tipo de parâmetro se usa o `req.params` - Na QueryString `?id=123` se usa o `req.query` no GET - No POST se usa o `req.body`
  <br />

- API - Restful

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

      app.use(
        cors({
          origin: function (origin, callback) {
            let allowed = true

            // permitir requests sem origem (tipo mobile apps e curl)
            if (!origin) allowed = true

            if (!allowedOrigins.includes(origin)) allowed = false

            callback(null, allowed)
          },
        })
      )
      ```

</details>

<details><summary><img src="https://img.shields.io/badge/JavaScript-29334C.svg?style=flat-square&logo=javascript&logoColor=%23F7DF1E" alt="Javascript" style="height: 20px;"> <img src="https://img.shields.io/badge/CSS-%231572B6.svg?style=flat-square&logo=css3&logoColor=white" alt="JavasCSScript" style="height: 20px;"> <img src="https://img.shields.io/badge/SCSS-f6538c.svg?style=flat-square&logo=SASS&logoColor=white" alt="SCSS" style="height: 20px;"></summary>
<br />

- Javascript:

  - `document.querySelector('ELEMENTO/ID/CLASS')` para elementos individuais
  - `document.querySelectorAll('ELEMENTO/ID/CLASS')` para elementos múltiplos
    - Usar o `foreach` quando for iterar
  - Pra capturar eventos `addEventListener('click', () => { COMANDOS })`
    - Outros eventos comuns: `mousemove`, `mouseout`, `mouseenter`, `mouseleave`
  - Para alterar uma classe `ELEMENTO.classList.contains('CLASS') ? ELEMENTO.classList.remove('CLASS') : ELEMENTO.classList.add('CLASS')`
  - Usar `$` nas variáveis que "puxam" HTML
  - Sempre que possível colocar `const` ao invés de `let`
  - Checar o _false_ primeiro no condicional
  - Funcionamento de um **foreach**:

  ```
  ELEMENTOS.forEach((e, index) =>
    e.innerHTML = `Número ${index+1}`
  )
  ```

  - Checar o _false_ primeiro no condicional
  - **this** pro primeiro escopo anterior, mais que isso tem que dar a volta
  - Onde tem **await** tem **async**. E quando usar uma função que tem async/await, tem que transformar o código que está chamando também em **await** **async**

- CSS:
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

</details>

<details><summary><img src="https://img.shields.io/badge/VS%20Code-0078d7.svg?style=flat-square&logo=visual-studio-code&logoColor=white" alt="SCSS" style="height: 20px;"></summary>
<br />
  
  - Para instalar a fonte FiraCode, no terminal rode: `sudo apt update && sudo apt install fonts-firacode`
  - Extensões usadas no momento: 
    - vscode-styled-components
    - prettier
    - material icon theme
    - markdown preview enhanced
    - live server
    - live sass compiler Glenn
    - ESLint
    - Ayu theme
    - auto rename tag
  
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
  "prettier.singleQuote": true,
  "editor.tabSize": 2,
  "editor.wordWrap": "on",
  "liveServer.settings.donotShowInfoMsg": true
}
```

</details>

<details><summary><img src="https://img.shields.io/badge/GIT-%23F05033.svg?style=flat-square&logo=git&logoColor=white" alt="Git" style="width: 50px"></summary>
<br />
  <details><summary><strong>Sincronizando repo local com o remoto</strong></summary>

- Se você não tem a chave SSH configurada, é bem tranquilo, só seguir esses comandos (_só faça isso se a máquina for sua, já que a chave SSH fica salva no sistema_):

  - **`ssh-keygen -t ed25519 -C "SEU-EMAIL"`**
  - Aperte _ENTER_ nas próximas 3 perguntas
  - **`eval "$(ssh-agent -s)"`**
  - **`ssh-add ~/.ssh/id_ed25519`**
  - **`cat ~/.ssh/id_ed25519.pub`**
  - Copie o resultado do comando anterior, que apareceu no terminal. Essa é a sua chave SSH.
  - Vá até [essa](https://github.com/settings/keys) página, clique em _New SSH key_, coloque o título que quiser, e no campo _key_ cole a sua chave. Clique em _Add SSH Key_, e pronto, sua máquina está com a chave SSH configurada.

- Configure globalmente seu user com os repos. Em qualquer lugar rode no terminal: **`git config --global user.name "SEU-USERNAME"`** e **`git config --global user.email "SEU-EMAIL"`**

- Existem duas formas pra prosseguir:

  - Se você criou o repositório no próprio Github, ou está sincronizando de lá. Entre na pasta onde vai guardar os repositórios e no terminal rode:
    **`git clone git@github.com:SeuUser/NomeDoRepo.git`**
    **`cd NomeDoRepo`**
    **Crie ou edite algum arquivo**
    **`git add .`**
    **`git commit -m "Init"`**
    **`git push -u origin main`**

  - Se você criou a pasta no PC e quer sincronizar com o Github. Crie no Github um repositório, vazio mesmo, com o mesmo nome do repo do PC. Entre na pasta do repo e no terminal rode:
    **`git init`**
    **`git remote add origin git@github.com:SeuUser/NomeDoRepo.git`**
    **`git add .`**
    **`git commit -m "Init"`**
    **`git push -u origin main`**

  - (sempre que mudar algo como username ou nome do repo, na sua máquina entre na pasta .git de cada repo e faça as alterações no arquivo config)

  </details>

  - Ciclo de vida dos arquivos:
    - **Untracked:** estados em que todos arquivos iniciam. Quando não está rastreado, sincronizado no repo local, no Git.
    - **Tracked:** quando o arquivo está rastreado pelo Git, está sob o controle de versionamento.
    - **Modified:** quando modifica um arquivo já rastreado. O Git te avisa que precisa atualizar o rastreamento.
    - **Staged:** quando o arquivo está pronto pro commit.
   
    <br />

  - Comandos Básicos:
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
    - **`git mv arquivo1.extensao arquivo2.extensao`** --> renomeia arquivos. Serve pra diretórios também. Certifique-se de estar no dir correto, e usar \*\*`git mv ./pasta1/ ./pasta2/`
      - Por que fazer isso pelo git e não pelo terminal normal? Porque você terá adicionar/trackear novamente o arquivo. Renomeando pelo próprio git, o arquivo continua trackeado, pronto pro commit.
    - **`git rm arquivo.extensao`** --> deletar arquivo. **`git rm -rf pasta/`** --> deletar diretório
      - Mas preste atenção, só pode excluir um diretório ou arquivo que já esteja sendo tracked pelo Git, do contrário vai dar erro, pois pra ele "não existe". Ah, e diretórios vazios não são sequer enxergados pelo Git, ele nem dá algum aviso. E portanto não dá pra remover, são untracked.
    - **`git diff`** vem de difference, mostra as diferenças de um estado pro outro, de um commit pro que virá. - Você tem que adicionar algo amais, exemplo **`git diff --staged`** para verificar diferença do anterior pro atual. - **`git diff hash`** --> verificar a diferença com um commit especifico. - **`git diff hash..hash`** para ver a diferença de um commit **até** o outro.
      <br />

  - Commit:

    - Um commit é tipo um snapshot do arquivo/algoritmo que está desenvolvendo. É um "okay" pro repo local e informa que o arquivo está pronto para ir pro repo remoto.
      - **`git commit -m ""`** onde **-m** significa a mensagem que aparecerá no commit.
    - Sempre que você fizer um commit, irá gerar um hash id, um identificador, exemplo **`[main 9da4dd5]`**
    - Quando esquecer de mandar certas mudanças pro mesmo commit, ou esquecer arquivos, etc, **antes do push**, você pode usar **`git commit --amend -m "mensagem"`** para fazer essas adições ao último commit.
    - Quando você adiciona um arquivo, deixa ele tracked, mas se arrepende, quer remover do track do Git, **`git restore --staged <file>`**
      <br />

  - Log/Histórico:

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
      <br />

  - Checkout

    - Através do hash id, conseguimos desafazer mudanças. Lembre-se que um commit é um snapshot, uma foto do projeto, você pode entrar naquela foto e voltar pro momento, igual Life is Strange 1 hahahaha.
    - **`git checkout`** e o hash id, exemplo **`0e1b5fa`**
    - Se você só quiser checar algo e voltar pro futuro, ou se arrepender, pode usar **`git checkout main`**
    - Quando se arrepender de uma mudança em um arquivo, tiver feito merda, **antes dele estar add, monitorado**, pode usar **`git checkout <file>`** que o arquivo voltará ao estado do último commit feito.
    - Pra fazer isso com todo projeto: **`git reset HEAD --hard`**
    - Para fazer isso, depois de ter commitado, (você irá voltar todo projeto pro último commit) **`git reset HEAD^ --hard`**
    - Para voltar todos arquivos pro estado original, do último commit, antes de estarem tracked, **`git checkout -- .`**
    - Para fazer isso com apenas um arquivo **`git checkout -- <filename>`**
    - Para fazer isso depois do arquivos estarem tracked: **`git checkout HEAD -- .`**
    - Para fazer isso com apenas um arquivo **`git checkout HEAD -- <filename>`**
      <br />

  - Revert e Reset

    - **Revert**: não desfaz um commit, ele reverte o que foi feito e criando um novo commit. Reverte. **`git revert <HashDoCommit>`**
      - Não esqueça de dar o **push** pro commit ir pro bare.
    - **Reset:** remove commits. **`git reset HEAD~1`** - **`git push -f -u origin main`**
      <br />

  - Branchs

    - Quando você cria um projeto no git, você tem seu **branch main**, que seria o **tronco** da árvore. É perigoso ficar commitando no tronco, pois se fizer algo errado, vai estragar toda árvore. Por isso você tem o conceito de **branchs secundárias**, que seriam os **galhos**, as **ramificações**. Então você está lá desenvolvendo certa **feature** do projeto, se ela der errado, você simplesmente joga o galho fora, corta ele. Mas se der certo, você faz um **merge**, **junta** o galho ao tronco, junta a branch secundária com a feature para a branch main.
    - **`git branch`** retorna quantas branchs existem e em qual branch você está (em verde e com um asterisco \*)
    - Para criar uma branch é bem simples **`git branch NomeDaBranch`**
    - Alternar entre branchs --> **`git checkout NomeDaBranch`**
      - (Se você quiser economizar tempo, pode criar e já alternar pra branch, com um comando só: **`git checkout -b NomeDaBranch`**)
    - Excluir uma branch --> **`git branch -d NomeDaBranch`**
      - Se a branch que vai ser excluída não foi fundida com outra em algum momento, o git vai perguntar se quer mesmo excluir, aí tem que rodar o mesmo comando, mas em caps o **`-D`**
    - Pra dar um **merge** você alterna pra branch que vai _absorver a outra_ (normalmente a main) e digita **`git merge NomeDaBranchAbsorvida`**
      - (Lembrando que após o merge, a branch absorvida não desaparece, ela continua viva e independente). Ah, e quando tal branch recebe o merge, ela absorve também os commit feitos, todo log etc
    - **Rebase** faz quase a mesma coisa que **merge**, mas deixa os commits em ordem, reoorganiza a ordem de todos commits do projeto. **`git rebase NomeDaBranch`** - Não é super indicado, principalmente em pair programming e em empresa. É até legal para projetos pessoais, mas melhor não usar.
      <br />

  - Clone, Push, Fetch, Pull e Tag

    - Pra clonar um repositório --> **`git clone urlDoRepo .`** (o ponto indica pra clonar dentro do repo que está)
      - Depois de clonar, entre no repo e configure seu usuário.
    - O **push** "empurra" pro repo remoto, o bare. **`git push -u origin main`** --> envia seus commits pro repo central
    - O **fetch** baixa os arquivos, mas sem trackear. **`git fetch`** aí depois tem que usar o git rebase pro arquivo organizar os arquivos e commits **`git rebase`**
      - Método menos utilizado.
    - O **pull** faz isso acima em uma tacada só **`git pull origin main`** (vai abrir um editor de código, só digitar ^O + enter + ^X)
      - Se acontecer o erro **refusing to merge unrelated histories**, rode **`git pull origin main --allow-unrelated-histories`**
    - A **tag** é um estado da aplicação, como se fosse um release, a versão. **`git tag versaoTal`**
      - Mas por enquanto isso só está no repo local. Para mandar pro repo remoto, para que todos users saibam da release **`git push origin versaoTal`**
      - Inclusive, você pode alternar para tags, para "dar uma olhada", igual faz em branchs. **`git checkout versaoTal`**
      - Você pode usar isso pra criar uma branch a partir de tal tag, tpo pra corrigir bugs de tal versão, etc. **`git switch -c <new-branch-name>`**
    - **Bare repository**: Significa repositório central, remoto. Lembrando que o git é descentralizado, mas é comum que tenhamos um repositório central, ainda mais quando trabalhamos em equipe.
      <br />

  - Issue, Fork e Pull Request

    - **Issue:** quando uma pessoa acha um problema em um projeto seu, pode reportar uma **issue**. Você também pode fazer isso com os outros. Mas quando reportar uma issue, pesquise bem antes, pra não criar uma que já foi resolvida.
      - Dá pra fechar uma issue no commit, dentro da mensagem dele, no final coloque **`Closes #IssueID`**
    - **Fork:** normalmente você forka um projeto pra resolver uns bugs ou melhorar e dar pull request, ou também quando quer criar algo novo com base naquele.
    - **Pull request:** é uma requisição para que o owner aceite as alterações feitas no se fork para o bare. Você também pode passar no título do pull request **`Closes #IssueID`** para que além de aceitar, fechar uma issue dele. - É uma boa prática ao invés de dar um merge com pull request, você dar um fetch (lembrando que o fetch baixa mas sem fundir), pra testar se realmente está tudo certo. **`git fetch origin pull/IdPullRequest/head:NomeDaBranch`** - Aí você olha o log, verifica o arquivo mexido, se está legal. E então vai no github e confirma o merge do pull request.
      <br />

  - Gist
    - Pequenos trechos de códigos que você cria pra você mesmo ou outras pessoas. Snippets.
    - Para usar facilmente com frequência.
    - Permite o compartilhamento de pequenos trechos de código. Há também quem use o Gist para receber feedbacks daquele código específico.
    - Também pode publicar parte do seu código e usar o plugin do Gist para mostrar seu código em sites, fóruns e outros locais. Para isso, só precisa publicar o código (depois de logar no GitHub) e clicar em “Show Embed” e ele lhe mostrará um código javascript para colar onde quiser. Onde você colar o javascript vai aparecer uma caixinha bonitinha com o trecho de código e um link para o seu Gist. Alterando seu Gist, todos os lugares onde você publicou seu código serão alterados ao mesmo tempo.

</details>
