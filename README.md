# ESLint + Prettier Angular

## Добавляем ESLint в проект

```
ng add @angular-eslint/schematics
```

## Устанавливаем Prettier

```
npm install prettier --save-dev
```

Нужно добавить файл `.prettierrc.json` и 
`.prettierignore` в корневую папку проекта.

В файл `.prettierrc.json` добавляем настройки

```json
{
  "tabWidth": 2,
  "useTabs": false,
  "singleQuote": true,
  "semi": false,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "trailingComma": "es5",
  "bracketSameLine": true,
  "printWidth": 80
}
```

Чтобы ESLint и Prettier сработались, нам нужно запустить Prettier как плагин ESLint.
```git
npm install prettier-eslint eslint-config-prettier eslint-plugin-prettier -save-dev
```

Теперь нам нужно отредактировать файл 
`.eslintrc.json`, чтобы включить `prettier` плагин.
```json
{
  "root": true,
  "overrides": [
    {
      "files": ["*.ts"],
      "extends": [
        ...
        "plugin:prettier/recommended"
      ],
    },
    // NOTE: WE ARE NOT APPLYING PRETTIER IN THIS OVERRIDE, ONLY @ANGULAR-ESLINT/TEMPLATE
    {
      "files": ["*.html"],
      "extends": ["plugin:@angular-eslint/template/recommended"],
      "rules": {}
    },
    // NOTE: WE ARE NOT APPLYING @ANGULAR-ESLINT/TEMPLATE IN THIS OVERRIDE, ONLY PRETTIER
    {
      "files": ["*.html"],
      "excludedFiles": ["*inline-template-*.component.html"],
      "extends": ["plugin:prettier/recommended"],
      "rules": {
        // NOTE: WE ARE OVERRIDING THE DEFAULT CONFIG TO ALWAYS SET THE PARSER TO ANGULAR (SEE BELOW)
        "prettier/prettier": ["error", { "parser": "angular" }]
      }
    }
  ]
}
```

Для VS-кода нам нужно добавить эти строки в `settings.json`:
```json
{
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    "editor.formatOnSave": false
  },
  "[typescript]": {
    "editor.defaultFormatter": "dbaeumer.vscode-eslint",
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    "editor.formatOnSave": false
  },
}
```

Для Webstorm

Нам нужно включить: `Run eslint --fix` на странице настроек `Actions On Save`

