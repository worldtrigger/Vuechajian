# Vue 区分生产环境和开发环境

## Vue2 版本

### 创建 3 个 url 文件

- 在 src 下面新建一个文件夹 config

- 里面新建 3 个文件分别是

- api_development.js

```javascript
let url = {
  homeurl: '这就是开发环境',
};
export default url;
```

- api_production.js

```javascript
let url = {
  homeurl: '这就是生产环境',
};
export default url;
```

- api_test.js

```javascript
let url = {
  homeurl: '这就是测试环境',
};
export default url;
```

### 在根目录(非 src)目录下创建 3 个.env 开头文件

- env.development

```javascript
NODE_ENV = 'development';

VUE_APP_MODE = 'development';
```

- env.production

```javascript
NODE_ENV = 'production';

VUE_APP_MODE = 'production';
```

- env.test

```javascript
NODE_ENV = 'test';

VUE_APP_MODE = 'test';
```

### 在 src 文件下新建 config.js 文件

```javascript
let ROOT;
if (process.env.VUE_APP_MODE == 'development') {
  console.log('开发环境');
  ROOT = require('@/config/api_development.js');
} else if (process.env.VUE_APP_MODE == 'production') {
  console.log('生产环境');
  ROOT = require('@/config/api_production.js');
} else if (process.env.VUE_APP_MODE == 'test') {
  console.log('测试环境');
  ROOT = require('@/config/api_test.js');
}

export default ROOT.default;
```

### 组件里引入 config.js

```javascript
<template>
  <div>
     {{message}}
  </div>
</template>

<script>
import url from '@/config.js'
  export default {
    data() {
      return {
        message: '头部'
      }
    },
    mounted(){
      this.message = url.homeurl
    }
  }
</script>

<style lang="less" scoped>

</style>
```

### 在 package.json 里面写命令

```javascript
{
  "scripts":{
    "serve":"vue-cli-service serve --mode development",
    "build-stage":"vue-cli-service build --mode test",
    "build-proud":"vue-cli-service build --mode production",
    "serve-production": "vue-cli-service serve --mode production",
    "serve-development": "vue-cli-service serve --mode development",
    "serve-stage": "vue-cli-service serve --mode stage",
    "serve-test": "vue-cli-service serve --mode test",
    "lint":"vue-cli-service lint"
  }
}
```

### 使用的时候调用不同的命令

### 封装 axios 的时候要是希望带上基础路径可以

```javascript
import Axios from 'axios';

const axios = Axios.create({
  baseURL: process.env.BASE_URL,
  headers: {
    'Content-Type': 'application/json;charset=UTF-8',
  },
});
export default axios;
```

## Vue3 同 Vue2 环境
