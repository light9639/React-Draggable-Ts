# ๐ช React-Draggable-Ts ์ฐ์ต ํ์ด์ง์๋๋ค.
:octocat: https://light9639.github.io/React-Draggable-Ts/

![light9639 github io_React-Draggable-Ts_](https://user-images.githubusercontent.com/95972251/216325962-b0ebdf05-5b56-41f5-95ec-1c057da36f88.png)

:sparkles: React์ `react-draggable` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ด์ฉํ์ฌ ๋ง๋  ํ์ด์ง์๋๋ค. :sparkles:
## :tada: React ์์ฑ
- React ์์ฑ
```bash
npm create-react-app my-app
# or
yarn create react-app my-app
```

- vite๋ฅผ ์ด์ฉํ์ฌ ํ๋ก์ ํธ๋ฅผ ์์ฑํ๋ ค๋ฉด
```bash
npm create vite@latest
# or
yarn create vite
```
- ํฐ๋ฏธ๋์์ ์คํ ํ ํ๋ก์ ํธ ์ด๋ฆ ๋ง๋  ํ `React` ์ ํ, `Typescirpt` or `Typescirpt - SWC` ์ ํํ๋ฉด ์์ฑ ์๋ฃ.
## :memo: vite.config.ts ์์ .
- `build`์ ์ด๋ฏธ์ง, CSS. js ํ์ผ์ ๊ฐ๊ฐ ๋ค๋ฅธ ํ์ผ์ ์ ์ฅํ๊ณ  ๊ฒฝ๋ก๋ฅผ `./`๋ก ํ๊ธฐ ์ํด ๋ค์๊ณผ ๊ฐ์ด ์ค์ ํ๋ค.
```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'
import tsconfigPaths from 'vite-tsconfig-paths';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react(), tsconfigPaths()],
  base: process.env.NODE_ENV === 'development' ? '/' : './',
  build: {
    rollupOptions: {
      output: {
        assetFileNames: (assetInfo) => {
          let extType = assetInfo.name.split('.').at(1);
          if (/png|jpe?g|svg|gif|tiff|bmp|ico/i.test(extType)) {
            extType = 'img';
          }
          return `assets/${extType}/[name]-[hash][extname]`;
        },
        chunkFileNames: 'assets/js/[name]-[hash].js',
        entryFileNames: 'assets/js/[name]-[hash].js',
      },
    },
  },
})
```

- ์์ฑ ์ดํ ์ค๋ฅ๊ฐ ๋๋ ๋ถ๋ถ๋ค์ ํด๊ฒฐํ๊ธฐ ์ํด ํฐ๋ฏธ๋์ yarn ๋ช๋ น์ด๋ก `vite-tsconfig-paths`, `@types/node`ํ์ผ๋ค์ ์ค์นํ๋ค.
```bash
yarn add vite-tsconfig-paths @types/node
```

## ๐ SCSS ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ค์น.
- **CSS** ๋์  **SCSS**๋ฅผ ์ฌ์ฉํ๊ธฐ ์ํด **SASS ๋ผ์ด๋ธ๋ฌ๋ฆฌ**๋ฅผ ์ค์นํ๋ค.
```bash
yarn add sass
```

- ์ค์น๊ฐ ์๋ฃ๋๋ฉด `.css`๋ฅผ `.scss`๋ก ๋ฐ๊ฟ์ ์ฌ์ฉ์ด ๊ฐ๋ฅํ๋ค.

## โ๏ธ App.tsx, App.scss ์์  ๋ฐ ์์ฑ
### :zap: App.tsx
- `Draggable`, `DraggableData` ํจ์๋ฅผ ์ด์ฉํ๋ฉด `React`์์ ๋๋๊ทธ๋ฅผ ์ฝ๊ฒ ๊ตฌํํ  ์ ์๋ค.
- `Opacity` ๊ฐ์ ์ง์ ํ์ฌ ๋๋๊ทธ๋ฅผ ์์ํ์ฌ `true`๊ฐ ๋  ์ ํฌ๋ช๋๊ฐ ๋ฎ์์ง๋ฉฐ, ๋๋๊ทธ๋ฅผ ๋ฉ์ถฐ์ `false`๊ฐ ๋  ๋์๋ ํฌ๋ช๋๊ฐ 1์ด ๋๋ค.
- `trackPos` ํจ์๋ฅผ ์ด์ฉํ์ฌ ๋๋๊ทธ๊ฐ ๊ฐ๋ฅํ๋ฉฐ `handleStart`๋ ํด๋ฆญํ์ฌ ์ด๋์ ์ด๋์ ์์ผ์ฃผ๋ฉฐ `handleEnd`๋ ๋ ์ด์ ํด๋ฆญํ์ง ์์ ์ํ์ผ ๊ฒฝ์ฐ ์ด๋์ํค์ง ์๋ ํจ์๋ฅผ ์๋ฏธํ๋ค.
```js
import React, { useState, useRef } from "react";
import reactLogo from './assets/react.svg'
import Draggable, { DraggableData } from "react-draggable";
import './App.scss'

export default function App(): JSX.Element {
  const nodeRef = useRef(null);

  const [position, setPosition] = useState({ x: 0, y: 0 });
  const [position2, setPosition2] = useState({ x: 50, y: 50 });

  const [Opacity, setOpacity] = useState(false);
  const [Opacity2, setOpacity2] = useState(false);

  const trackPos = (data: DraggableData) => {
    setPosition({ x: data.x, y: data.y });
  };

  const trackPos2 = (data: DraggableData) => {
    setPosition2({ x: data.x, y: data.y });
  };

  const handleStart = () => {
    setOpacity(true);
  };
  const handleEnd = () => {
    setOpacity(false);
  };

  const handleStart2 = () => {
    setOpacity2(true);
  };
  const handleEnd2 = () => {
    setOpacity2(false);
  };

  return (
    <div className="App">
      <h1>React-Draggable-Ts</h1>
      <Draggable
        nodeRef={nodeRef}
        onDrag={(e, data) => trackPos(data)}
        onStart={handleStart}
        onStop={handleEnd}
      >
        <div
          ref={nodeRef}
          className="box"
          style={{ opacity: Opacity ? "0.6" : "1" }}
        >
          <div>BOX</div>
          <div>
            x: {position.x.toFixed(0)}, y: {position.y.toFixed(0)}
          </div>
        </div>
      </Draggable>

      <Draggable
        nodeRef={nodeRef}
        onDrag={(e, data) => trackPos2(data)}
        onStart={handleStart2}
        onStop={handleEnd2}
        scale={2}
      >
        <div
          ref={nodeRef}
          className="box box2"
          style={{ opacity: Opacity2 ? "0.6" : "1" }}
        >
          <div>BOX with scale</div>
          <div>
            x: {position2.x.toFixed(0)}, y: {position2.y.toFixed(0)}
          </div>
        </div>
      </Draggable>
    </div>
  )
}
```

### :zap: App.scss
- `App.scss`์ box ํด๋์ค์ ์คํ์ผ ์ถ๊ฐ.
```scss
.box {
  cursor: pointer;
  padding: 2.5rem 0rem;
  border-radius: 5px;
  width: 250px;
  display: flex;
  flex-direction: column;
  margin: 25px auto;
  color: #fff;
  box-shadow: 0 0 15px hsla(0deg, 0%, 0%, 0.5);
  text-shadow: 0 2px 3px hsla(0deg, 0%, 0%, 0.25);
  background-image: linear-gradient(rgb(0, 198, 251, 0.9) 0%, rgb(0, 91, 234, 0.9) 100%);
}
```

## โ๏ธ ๋ฐ์ค๋ค์ ๋ง์ฐ์ค๋ก ์ด๋์ํค๊ธฐ.
- ๋ฐ์ค๋ค์ ๋ง์ฐ์ค๋ ์์ผ๋ก ๋๋๊ทธ ํ๋ฉด ํ์ผ์ด ์ด๋ํ๋ฉด์ ๋ฐ์ค์ ์์น๋ฅผ ํ์ํ๋ค.

![light9639 github io_React-Draggable-Ts_ (1)](https://user-images.githubusercontent.com/95972251/216326069-73ea2122-9a62-450e-b0c8-20e5bb910148.png)

## :tada: ํ๋ก์ ํธ ๋ฐฐํฌ.
- ํ๋ก์ ํธ๋ฅผ ์ ๋ถ ์์ฑํ์์ผ๋ฉด `gh-pages` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ํตํด ์ฝ๊ฒ ๋ฐฐํฌํ  ์ ์๋ค. ๋จผ์  `gh-pages`๋ฅผ ์ค์นํ๋ค.
```bash
yarn add gh-pages
```

- ์ค์น ์ดํ **package.json**์ **scripts** ๋ถ๋ถ์ ์์ ํ๊ณ  **homepage** ๋ถ๋ถ์ ๋ฐฐํฌ ๊ฒฝ๋ก๋ฅผ ์ ์ผ๋ฉด ๋๋ค.
```js
"scripts": {
  "predeploy": "yarn build",
  "deploy": "gh-pages -d dist"
},
"homepage": "http://light9639.github.io/React-Draggable-Ts",
```

- **package.json** ์์ ์ด ์๋ฃ๋๋ฉด **yarn deploy**๋ฅผ ์คํํ๋ฉด **gh-pages** ๋ธ๋์น๋ก ๋น๋๋ ํ์ผ์ด ์๋ก๋ ๋์ด ๋ฐฐํฌ๊ฐ ์๋ฃ๋๋ค.
