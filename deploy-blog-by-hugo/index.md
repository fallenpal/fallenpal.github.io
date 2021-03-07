# 改用Hugo作为Blog生成器


<!--more-->



# Hugo 部署个人博客

## Hugo配置

### test

#### something
生成静态页面
```
hugo --baseUrl="http://fallenpal.github.io/
```


## Github Pages设置

1. 申请一个`USERNAME.github.io`的repo
2. 将上一步生成的public目录同步到Github：
```bash
cd public 
git init  
git remote add origin git@github.com:fallenpal/fallenpal.github.io.git
git add .
git commit -m "initial version"
git push -u origin main // 推送到github上的repo
```
