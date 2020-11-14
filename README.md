# Curso React Avançado - BoilerPlate



# Como criar um projeto através do boilerplate

```bash
yarn create next-app -e <endereço do repo do boilerplate>
```

informar o nome do seu projeto quando solicitado no terminal

renomear o projeto no `package.json`

# Boilerplate - Como foi criado

## 🎆 Criar boilerplate Next

```bash
yarn create next-app
```

nome do projeto: client

### principais comandos

```bash
// startar servidor em modo desenvolvimento
yarn dev

// build de produção
yarn build

// starta o servidor em modo produção com o build feito
yarn start
```

## typescript

add o arquivo tsconfig.json na pasta raíz client.

instalar dependencias:

```bash
yarn add --dev typescript @types/react @types/node
```

startar o servidor novamente para que o next atualize o arquivo tsconfig.json:

```bash
yarn dev
```

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true, // modificar para true
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve"
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

## atualizando a estrutura do projeto

criar pasta src

mover a pasta pages e a pasta styles para dentro da pasta src

renomear o arquivo index.js para index.tsx

## editor config

criar arquivo .editorconfig na pasta raíz (client) e add as rules:

```json
# editorconfig.org
root = true

[*]
indent_style = spaces
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

## eslint

```bash
npx eslint --init
```

opções:

✔ How would you like to use ESLint? · To check syntax and find problems
✔ What type of modules does your project use? · JavaScript modules (import/export)
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · Yes
✔ Where does your code run? · browser

✔ What format do you want your config file to be in? · JSON
✔ Would you like to install them now with npm? · No / Yes 

se optar por instalar as dependências manualmente:

```bash
yarn add --dev eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest eslint@latest
```

add plugin para react hooks

```bash
yarn add eslint-plugin-react-hooks --dev
```

add config no arquivo eslintrc.json:

```json
{
	// ...
	"settings": {
        "react": {
            "version": "detect"
        }
  },
	"extends": [
      "eslint:recommended",
      "plugin:@typescript-eslint/eslint-recommended",
      "plugin:@typescript-eslint/recommended",
    ],
  "plugins": [
    // ...
    "react-hooks"
  ],
  "rules": {
    // ...
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/prop-types": "off", // para evitar conflitos com o typescript
		"explicit-module-boundary-types": "off", // pq já estamos utilizando o ts
		"react/react-in-jsx-scope": "off" // pq estamos trabalhando com o next, não precisamos importar
   }
}
```

instalar o plugin no vscode (para que os erros sejam apontados automaticamente no ediitor)

[https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint), ou rodar no terminal

```bash
eslint src --fix
```

src: nome da pasta que quer verificar

—fix: opcional, caso queira corrigir alguns problemas automaticamente

caso queria, pode cadastrar o comando no package.json.

## prettier

[https://prettier.io/docs/en/configuration.html](https://prettier.io/docs/en/configuration.html)

```bash
yarn add --dev prettier eslint-config-prettier eslint-plugin-prettier
```

criar arquivo .prettierrc e add suas config,  por exemplo:

```json
{
  "trailingComma": "none",
  "semi": false,
  "singleQuote": true
}
```

no arquivo .eslintrc add config do prettier:

```json
{
	// ...
	"extends": [
        "plugin:prettier/recommended",
  ],
	// ...
	"rules": {
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "prettier/prettier": [
      "error",
      {
        "endOfLine": "auto"
      }
    ]
  }
}
```

você também pode criar um arquivo de config do VSCode do projeto:

.vscode/settings.json

```json
{
	"editor.formatOnSave": false,
  "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
  }
}
```

*formatOnSave deve ficar como false

## husky e lint staged

```bash
yarn add --dev husky lint-staged
```

package.json:

```json
{
  "scripts": {
  // ...
  "lint": "eslint src --max-warnings=0"
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "src/**/*": ["yarn lint --fix"]
  },
}
```

## Jest

```bash
yarn add --dev jest @babel/preset-typescript @types/jest
```

```bash
yarn add --dev babel-jest @babel/core @babel/preset-env
```

add jest no arquivo .eslintrc

```bash
{
    "env": {
			// ...
      "jest": true,
			"node": true
    },
}
```

criar arquivo jest.config.js:

```jsx
module.exports = {
  testEnvironment: 'jsdom',
  testPathIgnorePatterns: ['/node_modules/', '/.next/'],
  collectCoverage: true,
  collectCoverageFrom: ['src/**/*.ts(x)'],
  setupFilesAfterEnv: ['<rootDir>/.jest/setup.ts']
}
```

- testEnvironment: front jsdom, simula browser
- collectCoverageFrom: arquivos dentro da pasta src que tenham extensão .ts ou .tsx

criar pasta e arquivo .jest/setup.ts

```jsx
// configurar depois
```

criar arquivo .babelrc

```jsx
{
  "presets": ["next/babel", "@babel/preset-typescript"]
}"test": "jest",
    "test:watch": "yarn test --watch",
```

package.json:

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "yarn test --watch",
  },
  // ...
}
```

## Testing library

```bash
yarn add --dev @testing-library/react @testing-library/jest-dom
```

add import no arquivo .jest/setup.ts

```jsx
import '@testing-library/jest-dom'
```

package.json

```bash
{
  // ...
  "lint-staged": {
    "src/**/*": [
      "yarn test --findRelatedTests --bail"
    ]
  },
  // ...
}
```

*—bail: para o teste quando falhar/ --findRelatedTests só quebrar quando algum teste quebrar

## Styled Components

```bash
yarn add styled-components
```

```bash
yarn add --dev @types/styled-components babel-plugin-styled-components
```

add plugins config ao arquivo .babelrc

```json
{
  "plugins": [
    [
      "babel-plugin-styled-components",
      {
        "ssr": true
      }
    ]
  ],
  "presets": ["next/babel", "@babel/preset-typescript"]
}
```

por estarmos trabalhando com next, temos que configurar e sobreescrever o arquivo Documents original, para isso criamos um arquivo _document.tsx dentro da pasta pages:

```tsx
import Document, {
  Html,
  Head,
  Main,
  NextScript,
  DocumentContext
} from 'next/document'
import { ServerStyleSheet } from 'styled-components'

export default class MyDocument extends Document {
  static async getInitialProps(ctx: DocumentContext) {
    const sheet = new ServerStyleSheet()
    const originalRenderPage = ctx.renderPage

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App) => (props) =>
            sheet.collectStyles(<App {...props} />)
        })

      const initialProps = await Document.getInitialProps(ctx)
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>
        )
      }
    } finally {
      sheet.seal()
    }
  }

  render() {
    return (
      <Html lang="pt-BR">
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    )
  }
}
```

### Configurar Jest com styled components

```bash
yarn add --dev jest-styled-components
```

add import no arquivo .jest/setup.ts

```tsx
**import 'jest-styled-components';**
```

## Absolute imports

tsconfig.json

```tsx
{
  "compilerOptions": {
    "baseUrl": "src"
		// ...
	}
}
```

## Global Styles

arquivo principal: src/pages/_app.tsx

```tsx
import { AppProps } from 'next/app';
import Head from 'next/head';

import GlobalStyles from 'styles/global';

function App({ Component, pageProps }: AppProps) {
  return (
    <>
      <Head>
        <title>React Avançado - Boilerplate</title>
        <link rel="shortcut icon" href="/img/icon-512.png" />
        <link rel="apple-touch-icon" href="/img/icon-512.png" />
        <meta
          name="description"
          content="A simple project starter to work with TypeScript, React, NextJS and Styled Components"
        />
      </Head>
      <GlobalStyles />
      <Component {...pageProps} />
    </>
  );
}

export default App;
```

stilo principal: src/styles/global.ts

```tsx
import { createGlobalStyle } from 'styled-components';

const GlobalStyles = createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  html {
    font-size: 62.5%;
  }
  html, body, #__next {
    height: 100%;
  }
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif
  }
`;

export default GlobalStyles;
```

## StoryBook

```bash
npx -p @storybook/cli sb init --type react
```

```bash
yarn add --dev @storybook/addon-knobs
```

```bash
yarn add @types/node
```

para rodar:

```bash
yarn storybook
```

*ver config:  

- [https://github.com/layshidani/react-avancado-boilerplate/commit/35c03ea0ea80e28df5c6c50f912dbd1754715eaf](https://github.com/layshidani/react-avancado-boilerplate/commit/35c03ea0ea80e28df5c6c50f912dbd1754715eaf)
- [https://github.com/layshidani/react-avancado-boilerplate/commit/2ac64b32a60b999f4136d343e795b8b63e24d83f](https://github.com/layshidani/react-avancado-boilerplate/commit/2ac64b32a60b999f4136d343e795b8b63e24d83f)

## PWA

```bash
yarn add next-pwa
```

config: 

- [https://github.com/layshidani/react-avancado-boilerplate/commit/726b541b708fdffaff688bcaa28da2c0556136fc](https://github.com/layshidani/react-avancado-boilerplate/commit/726b541b708fdffaff688bcaa28da2c0556136fc)

rodar pela 1ª vez:

```bash
NODE_ENV=production yarn build
```

depois, rodar o projeto `yarn run start`
no chrome dev tools > lighthouse
