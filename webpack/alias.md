# Алиасы в webpack

Алиасы это удобный способ навигации внутри проекта, например:

```javascript
// Вместо того чтобы писать
import module from '../../../direction/module'

// Можно писать
import module from 'direction/module'
```

Для этого существует механизм [алиасов](https://webpack.js.org/configuration/resolve/#resolve-alias) webpack:

```javascript
const path = require('path')

module.exports = {
  // ...
  resolve: {
    alias: {
      common: path.join(__dirname, '../src/common'),
      features: path.join(__dirname, '../src/features'),
      components: path.join(__dirname, '../src/common/components'),
    },
  },
  // ...
}
```

`__dirname` это [глобальная переменная node.js](https://nodejs.org/docs/latest/api/modules.html#modules_dirname) хранит имя каталога текущего модуля

`path.join()` это [метод модуля](https://nodejs.org/api/path.html#path_path_join_paths) `path` собирает путь до файла, в зависимости от платформы:

```javascript
// Если __dirname у нас равен 'users/USER/project/',
// то для вебпака пути будут выглядеть так:
module.exports = {
  // ...
  resolve: {
    alias: {
      common: 'users/USER/project/src/common',
      features: 'users/USER/project/src/features',
      components: 'users/USER/project/src/common/components',
    },
  },
  // ...
}
```

Теперь у нас есть удобный механизм алиасов для директорий.

## Бонус

Простой способ динамического получения алиасов:

```javascript
const fs = require('fs')
const path = require('path')

// Имя директории внутри которой все имена папок станут алиасами
const DIR_INCLUDE_ALIAS = '../src'
function getDirectories = (src) =>
  fs.readdirSync(src).filter((file) => fs.lstatSync(path.join(src, file)).isDirectory())

// Получение списка директорий
const appFolders = getDirectories(path.resolve(__dirname, DIR_INCLUDE_ALIAS))

const ourAlias = appFolders.reduce((acc, item) => (
  Object.assign({}, acc, {
    [item]: path.resolve(__dirname, `${DIR_INCLUDE_ALIAS}/${item}/`),
  })
), {})

// Можно использовать прямо так:
module.exports = {
  // ...
  resolve: {
    alias: ourAlias,
  },
  // ...
}

// Можно дополнить другими алиасами:
const moreAlias = Object.assign(ourAlias, {
  config: path.resolve(__dirname, `../../config`),
  styles: path.resolve(__dirname, '../../../styles/'),
  images: path.resolve(__dirname, '../../../public/images/'),
})
```

