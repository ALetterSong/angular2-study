```
    "clean": "rimraf node_modules doc dist && npm cache clean", // 通过 rimraf 清理
    "clean-install": "npm run clean && npm install", // 清理并安装依赖
    "clean-start": "npm run clean-install && npm start", // 清理并启动应用
    "watch": "webpack --watch --progress --profile", // 启动 webpack 动态监听
    "build": "rimraf dist && webpack --progress --profile --bail", // 打包生产环境代码
    "server": "webpack-dashboard -- webpack-dev-server --inline --port 8080", // 启动开发环境
    "webdriver-update": "webdriver-manager update", // https://github.com/preboot/angular2-webpack#testing
    "webdriver-start": "webdriver-manager start", // https://github.com/preboot/angular2-webpack#testing
    "lint": "tslint --force \"src/**/*.ts\"", // 启动 tslint 语法检查工具
    "e2e": "protractor", // 启动 protactor 端对端测试
    "e2e-live": "protractor --elementExplorer", //
    "pretest": "npm run lint", // 启动 tslint 语法检查工具
    "test": "karma start", // 启动 karma 单元测试
    "posttest": "remap-istanbul -i coverage/json/coverage-final.json -o coverage/html -t html", //
    "test-watch": "karma start --no-single-run --auto-watch", //
    "ci": "npm run e2e && npm run test", // 持续集成
    "docs": "typedoc --options typedoc.json src/app/app.component.ts", // 生成 typedoc 文档工具
    "start": "npm run server", // 启动开发环境
    "start:hmr": "npm run server -- --hot", // 启动开发环境和热替换
    "postinstall": "npm run webdriver-update" // https://github.com/preboot/angular2-webpack#testing
```