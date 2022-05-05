
# [作者跑步主页](https://yihong.run/running)
# [作者源代码](https://github.com/yihong0618/running_page)




# 我的设置步骤

### 1、下载源代码

```
git clone https://github.com/yihong0618/running_page.git
```
### 2、安装及测试 (node >= 12 and <= 14 python >= 3.6)

```
pip3 install -r requirements.txt
yarn install
yarn develop
```
### 3、访问 http://localhost:8000/ 查看

### 4、拉取自己的数据。
以keep为例：
```
python3 scripts/keep_sync.py ${{ secrets.KEEP_MOBILE }} ${{ secrets.KEEP_PASSWORD }} --with-gpx
```
```
python3 scripts/keep_sync.py 13800000000 12345678 --with-gpx
```
### 5、在本地生成自己的跑步数据

```
python3(python) scripts/gen_svg.py --from-db --title "${{ env.TITLE }}" --type github --athlete "${{ env.ATHLETE }}" --special-distance 10 --special-distance2 20 --special-color yellow --special-color2 red --output assets/github.svg --use-localtime --min-distance 0.5
```
```
python3 scripts/gen_svg.py --from-db --title "v" --type github --athlete "b" --special-distance 10 --special-distance2 20 --special-color yellow --special-color2 red --output assets/github.svg --use-localtime --min-distance 0.5
```
```
python3(python) scripts/gen_svg.py --from-db --title "${{ env.TITLE_GRID }}" --type grid --athlete "${{ env.ATHLETE }}"  --output assets/grid.svg --min-distance 10.0 --special-color yellow --special-color2 red --special-distance 20 --special-distance2 40 --use-localtime
```
```
python3 scripts/gen_svg.py --from-db --title "v" --type grid --athlete "b"  --output assets/grid.svg --min-distance 3.0 --special-color yellow --special-color2 red --special-distance 20 --special-distance2 40 --use-localtime
```
生成年度环形数据

```
python3(python) scripts/gen_svg.py --from-db --type circular --use-localtime
```
```
python3 scripts/gen_svg.py --from-db --type circular --use-localtime
```
### 6、push本地代码到自己的repo。
### 7、修改Actions [源码](https://github.com/yihong0618/running_page/blob/master/.github/workflows/run_data_sync.yml)需要做如下步骤

1. 更改成你的 app type 及 info

   ![image](https://user-images.githubusercontent.com/15976103/94450124-73f98800-01df-11eb-9b3c-ac1a6224f46f.png)
2. 在 repo Settings > Secrets 中增加你的 secret (只添加你需要的即可)

   ![image](https://user-images.githubusercontent.com/15976103/94450295-aacf9e00-01df-11eb-80b7-a92b9cd1461e.png)
   我的 secret 如下

   ![image](https://user-images.githubusercontent.com/15976103/94451037-8922e680-01e0-11eb-9bb9-729f0eadcdb7.png)
3. 添加你的 [GitHub secret](https://github.com/settings/tokens)并和项目中的 GitHub secret 同名

   ![image](https://user-images.githubusercontent.com/15976103/94450721-2f222100-01e0-11eb-94a7-ef1f06fc0a59.png)

### 8、开启workflows
### 9、配置GitHub page
1. 配置 GitHub Action。如需使用自定义域名，可以修改 [.github/workflows/gh-pages.yml](.github/workflows/gh-pages.yml) 中的 `fqdn`（默认已注释掉）

2. 修改 `gatsby-config.js`，更新 `pathPrefix`。【如果使用自定义域名，可跳过这一步】

3. 在项目的 `Actions -> Workflows -> All Workflows` 中选择 Publish GitHub Pages，点击 `Run workflow`

4. 在项目的 `Settings -> GitHub Pages -> Source` 部分，选择 `Branch: gh-pages` 并点击 `Save`。

### 10、同步即可。


## 其他
### 1、使用自己的mapbox token。
替换 `src/utils/const.js` 文件中的 Mapbox token

> 建议有能力的同学把代码中的 Mapbox token 自己的 [Mapbox token](https://www.mapbox.com/)

```javascript
const MAPBOX_TOKEN =
  'pk.eyJ1IjoieWlob25nMDYxOCIsImEiOiJja2J3M28xbG4wYzl0MzJxZm0ya2Fua2p2In0.PNKfkeQwYuyGOTT_x9BJ4Q';
```

### 2、 一些个性化选项

* 在仓库目录下找到 `gatsby-config.js`，找到以下内容并修改成你自己想要的。

```javascript
siteMetadata: {
  siteTitle: 'Running Page', #网站标题
  siteUrl: 'https://yihong.run', #网站域名
  logo: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQTtc69JxHNcmN1ETpMUX4dozAgAN6iPjWalQ&usqp=CAU', #左上角LOGO
  description: 'Personal site and blog',
  navLinks: [
    {
      name: 'Blog', #右上角导航名称
      url: 'https://yihong.run/running', #右上角导航链接
    },
    {
      name: 'About',
      url: 'https://github.com/yihong0618/running_page/blob/master/README-CN.md',
    },
  ],
},
```
* 修改 `src/utils/const.js` 文件中的样式： 
```javascript
// styling: 关闭虚线: 设置为 `false`
const USE_DASH_LINE = true;
// styling: 透明度: [0, 1]
const LINE_OPACITY = 0.4;
```