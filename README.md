# ng2-taidiiv2

## Based On

- [angular 2](https://angular.io/) 
- [materialize](https://github.com/InfomediaLtd/angular2-materialize)
- [webpack](https://github.com/preboot/angular2-webpack)

## Getting Started

<details>
<summary>
  Installing to deploying
</summary>

### Installing

```
npm install
```

### Developing

```
npm start
```

### Testing

```
npm test
```

### Production

```
npm run build
```

### Deploying

Copy all files in [config](config/) into your `~/.ssh/` and execute the following script

```
tar zcf ng2-taidiiv2.tar.gz dist
rsync -av ng2-taidiiv2.tar.gz taidii.aly:~/
```

Then log in to aliyun and move `/dist` to `/data/www/ng2-taidiiv2`

```
ssh taidii.aly
tar zxf ng2-taidiiv2.tar.gz
sudo mv dist /data/www/ng2-taidiiv2
```

### Other Scripts

[npm scripts](npm-scripts.md)

</details>

## Documentation

[Angular2 开发指南](ng2-guide.md)
  
[开发备忘录](mome.md)

[Git 规范](git-guide.md)

[接口文档](api.md)

[版本历史记录](CHANGELOG.md)

