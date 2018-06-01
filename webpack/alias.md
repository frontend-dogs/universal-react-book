# Алиасы



## Алиасы в webpack

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

`__dirname` это [глобальная переменная node.js](https://nodejs.org/docs/latest/api/modules.html#modules_dirname) которая хранит имя каталога текущего модуля

`path.join()` это [метод модуля](https://nodejs.org/api/path.html#path_path_join_paths) `path` который собирает путь до файла, в зависимости от платформы:

```javascript

// Если __dirname у нас равен 'users/USER/project/', то финальные пути будут выглядеть так:

module.exports = {
  // ...
  resolve: {
    alias: {
      common: path.join(__dirname, '../src/common'), // users/USER/project/src/common'
      features: path.join(__dirname, '../src/features'), // users/USER/project/src/features'
      components: path.join(__dirname, '../src/common/components'), // users/USER/project/src/common/components'
    },
  },
  // ...
}

```
