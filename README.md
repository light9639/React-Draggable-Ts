# 📌 React-Draggable-Ts 연습 페이지입니다.
:octocat: https://light9639.github.io/React-Draggable-Ts/

![light9639 github io_React-Draggable-Ts_](https://user-images.githubusercontent.com/95972251/216325962-b0ebdf05-5b56-41f5-95ec-1c057da36f88.png)

:sparkles: React의 `react-draggable` 라이브러리를 이용하여 만든 페이지입니다. :sparkles:
## :tada: React 생성
- React 생성
```bash
npm create-react-app my-app
# or
yarn create react-app my-app
```

- vite를 이용하여 프로젝트를 생성하려면
```bash
npm create vite@latest
# or
yarn create vite
```
- 터미널에서 실행 후 프로젝트 이름 만든 후 `React` 선택, `Typescirpt` or `Typescirpt - SWC` 선택하면 생성 완료.
## :memo: vite.config.ts 수정.
- `build`시 이미지, CSS. js 파일을 각각 다른 파일에 저장하고 경로를 `./`로 하기 위해 다음과 같이 설정한다.
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

- 작성 이후 오류가 나는 부분들을 해결하기 위해 터미널에 yarn 명령어로 `vite-tsconfig-paths`, `@types/node`파일들을 설치한다.
```bash
yarn add vite-tsconfig-paths @types/node
```

## 🚅 SCSS 라이브러리 설치.
- **CSS** 대신 **SCSS**를 사용하기 위해 **SASS 라이브러리**를 설치한다.
```bash
yarn add sass
```

- 설치가 완료되면 `.css`를 `.scss`로 바꿔서 사용이 가능하다.

## ✒️ App.tsx, App.scss 수정 및 작성
### :zap: App.tsx
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
- `App.scss`에 box 클래스에 스타일 추가.
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

## :test_tube: 박스들을 마우스로 이동시키기.
- 박스들을 마우스나 손으로 드래그 하면 파일이 이동하면서 박스의 위치를 표시한다.

![light9639 github io_React-Draggable-Ts_ (1)](https://user-images.githubusercontent.com/95972251/216326069-73ea2122-9a62-450e-b0c8-20e5bb910148.png)

## :tada: 프로젝트 배포.
- 프로젝트를 전부 작성하였으면 `gh-pages` 라이브러리를 통해 쉽게 배포할 수 있다. 먼저 `gh-pages`를 설치한다.
```bash
yarn add gh-pages
```

- 설치 이후 **package.json**의 **scripts** 부분을 수정하고 **homepage** 부분에 배포 경로를 적으면 된다.
```js
"scripts": {
  "predeploy": "yarn build",
  "deploy": "gh-pages -d dist"
},
"homepage": "http://light9639.github.io/React-Draggable-Ts",
```

- **package.json** 수정이 완료되면 **yarn deploy**를 실행하면 **gh-pages** 브랜치로 빌드된 파일이 업로드 되어 배포가 완료된다.
