# 知識點
- 雖然 [官方說明](https://vitejs.dev/guide/features.html#css-pre-processors) 說要安裝 corresponding pre-processor，但我在沒有安裝 sass 的狀況下，也有成功的跑起來了，不太明白原因是甚麼


# 參考文章
- [Difference between 'npm add' and 'npm install --save'?](https://stackoverflow.com/questions/51466746/difference-between-npm-add-and-npm-install-save)
  > npm install and add are aliases


# Prettier, ESLint, Stylelint

## Prettier
- 使用 Prettier 非常簡單，比較需要注意的事情是與 ESLint, Stylelint 的整合
  - `npm i -D prettier`
  - 安裝 [vs code 的 prettier extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
  -  `.vscode/settings` 中設定
     - 存檔時自動排版
       -  `"editor.formatOnSave": true`
     - 指定檔案類型的 formatter 為剛剛安裝的 extension(esbenp.prettier-vscode)
      ```json
      "[html, css, scss, javascript, typescript, typescriptreact, yaml]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      },
      ```

## ESLint
- 透過 vite 創建的 react + typescript 專案(`$ npm create vite@latest`)，預設就有引入 ESLint  
  預設有裝的幾個 package
  1. `eslint:recommended`
     - `eslint`
  2. `plugin:@typescript-eslint/recommended`
     - `typescript-eslint` enables ESLint to run on TypeScript code.
       1. allows ESLint to parse TypeScript syntax
       2. creates a set of tools for ESLint rules to be able to use TypeScript's type information
       3. provides a large list of lint rules that are specific to TypeScript and/or use that type information
  3. `eslint-plugin-react-hooks`
     - `plugin:react-hooks/recommended`
     - 用於檢測 React Hooks 規則的 ESLint 插件
  4. `eslint-plugin-react-refresh`
     - Validate that your components can safely be updated with fast refresh.
  5. `@typescript-eslint/parser`
     - An ESLint parser which leverages TypeScript ESTree to allow for ESLint to lint TypeScript source code.
  6. `@typescript-eslint/eslint-plugin`
     - An ESLint plugin which provides lint rules for TypeScript codebases.
- 預設的 eslint 設定
  ```js
  module.exports = {
    root: true,
    env: { 
      browser: true,
      es2020: true
    },
    extends: [
      'eslint:recommended',
      'plugin:@typescript-eslint/recommended',
      'plugin:react-hooks/recommended',
    ],
    ignorePatterns: ['dist', '.eslintrc.cjs'],
    parser: '@typescript-eslint/parser',
    plugins: ['react-refresh'],
    rules: {
      'react-refresh/only-export-components': [
        'warn',
        { allowConstantExport: true },
      ],
    },
  }
  ```
- 參考文章
  - [eslint-plugin-react-hooks 配置](https://juejin.cn/s/eslint-plugin-react-hooks%20%E9%85%8D%E7%BD%AE)
  - [TypeScript 新手指南 - 程式碼檢查](https://willh.gitbook.io/typescript-tutorial/engineering/lint#zai-typescript-zhong-shi-yong-eslint)
    > 由於 ESLint 預設使用 `Espree` 進行語法解析，無法識別 TypeScript 的一些語法，故我們需要安裝 `typescript-eslint-parser`，替代掉預設的解析器  

    > 由於 `typescript-eslint-parser` 對一部分 ESLint 規則支援性不好，故我們需要安裝 `eslint-plugin-typescript`，彌補一些支援性不好的規則。
### ESLint + prettier
1. 另外安裝下列兩個 package
   1. `eslint-plugin-prettier`
      - Runs Prettier as an ESLint rule and reports differences as individual ESLint issues.
   2. `eslint-config-prettier`
      - Turns off all rules that are unnecessary or might conflict with Prettier
2. add `plugin:prettier/recommended` as the last extension in your `.eslintrc.json`(意指 eslint 設定檔)
    ```json
    {
      "extends": [
        // ... other extends
        "plugin:prettier/recommended"
      ]
    }
    ```


## Stylelint

### Stylelint + Prettier




# GitHub Actions workflow
- src/main.tsx(21,7): error TS2580: Cannot find name 'process'. Do you need to install type definitions for node? Try `npm i --save-dev @types/node`.
fix: 修復 github actions 觸發的問題

報錯訊息：
error TS2580: Cannot find name 'process'. Do you need to install type definitions for node? Try `npm i --save-dev @types/node`.

解決方法：
`npm i --save-dev @types/node`


# TypeScript
- `document.getElementById('root') as HTMLElement` 這個語法叫做類型斷言，有時也叫做轉換。  
當你比類型檢查器更清楚一個表達式的類型的時候，你可以用這種方式告知 TypeScript。  
我們之所以這麼做是因為 getElementById 的回傳值類型是 HTMLElement | null。  
簡單地說，getElementById返回null是當無法找對對應id元素的時候。 我們假設getElementById總是成功的，因此我們要使用as語法告訴TypeScript這點。

  TypeScript還有一種感嘆號（!）結尾的語法，它會從前面的表達式中移除 null 和 undefined。 所以我們也可以寫成 document.getElementById('root')!，但在這里我們想寫的更清楚些。
  - [TypeScript 中文手册 React](https://typescript.bootcss.com/tutorials/react.html#:~:text=as%20HTMLElement%0A)%3B-,%E7%B1%BB%E5%9E%8B%E6%96%AD%E8%A8%80,-%E8%BF%99%E9%87%8C%E8%BF%98%E6%9C%89%E4%B8%80%E7%82%B9)




可以嘗試把 複製 404 的步驟移除，然後看看 router 重新導向會不會出問題
→ 會



# 前端開發環境基礎建設
- 統一開發環境
  - Docker
- 統一跨 IDE 的檔案格式設定
  - .editorconfig
    - 維護程式碼基底中的一致編碼樣式和設定，例如縮排樣式、索引標籤寬度、行尾字元、編碼等等，不論您使用的編輯器或 IDE 為何
- 開發時
  - vs code 套件自動修正
    - editorconfig
    - prettier
    - eslint
    - stylelint
- commit 前檢查
  - husky
  - lint-staged
  - prettier
  - eslint
  - stylelint
  - test
- 檢查 commit message
  - husky
  - commitlint
- ci, cd
  - github actions

