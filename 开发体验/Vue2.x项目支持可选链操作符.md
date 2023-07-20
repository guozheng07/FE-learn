1. 安装依赖
   ```
   yarn add @babel/plugin-proposal-optional-chaining@7.13.12
   yarn add @babel/plugin-proposal-nullish-coalescing-operator@7.13.8
   ```
2. 在仓库 babel.config.js 或 .babelrc 中添加插件
   ```
   // .babelrc 
    {
        "plugins": [
            "@babel/plugin-proposal-nullish-coalescing-operator",
            "@babel/plugin-proposal-optional-chaining"
        ]
    }

    // babel.config.js
    module.exports = {
        plugins: [
            '@babel/plugin-proposal-nullish-coalescing-operator',
            '@babel/plugin-proposal-optional-chaining'
        ],
    };
   ```
3. 重启项目
