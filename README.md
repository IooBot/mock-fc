# mock-fc

zero code graphql server / endpoint , support serverless deploy
use mock data build backend, include schema automate generation

不需要编程，通过模拟/测试数据即可构建基于graphql的后端服务，包括数据库。

## 特点：

1. 从数据中推理出数据库的表结构和数据关联，自动生成schema；
2. 并支持直接部署到云平台上（仅需一条指令）；
3. 如配合graphql-cache，即可实现即简洁又高效的开发方式:
   同时具备mongodb的数据库访问的便利性和graphql的多表数据一次获取自动展开等特性。
4. zero code 构建数据查询 query

## Installation
+ from source : lein uberjar
or
+ download mock-fc.jar, mock.bat from github

modify mock.bat, change the path of mock-fc.jar to where it is

## Usage
`<mock data>` 表示 mock数据文件名， 例如 taro-shop.json 或者带目录 mock/taro-shop.json

 生成schema，并启动本地graphql server

    mock <mock data>

 生成增强模式的schema,并启动 graphql server

    mock -e <mock data>

 增强模式下,还可以增加token 进行访问控制

    mock -e <mock data> <token config>

 跳过生成schema，直接启动本地graphql server

    mock -s <mock data>     仍使用mock data, 如果之前有db，会被覆盖
    mock -s -f <mock data>  直接用当前fcdb，不使用mock data
    mock -s -p <mock data>  使用 mp cloud db
    mock -s -m <mock data>  使用 mongodb

 只生成schema，不启动graphql server

    mock -b <mock data>

 不生成simple script, 生成数据，并启动server，通常执行完 mock -b -g之后

    mock -a <mock data>

 从simple script 生成 schema和data

    mock -b -g <simple script>

 从simple script 生成 graphql.js 的schema和graphql sinc需要的keyinfo

    mock -j <simple script> <output file name>
    mock -j <simple script> <visual-fields> <output file name>

 部署到腾讯云

    mock -d <mock data>

 升级腾讯云上的serverless （函数服务）

    mock -u <mock data>

 Import data which export from db

    mock -i <data directory> <output file>

 Import csv data with field type parse (support GBK encode)

    mock -i csv-file <output file> field1 type1 field2 type2

 zero code 构建查询 (question) 注意: 如果尚未创建schema, 需要先 build (-b)

     mock -r <mock data>

 复制fcdb 到 edn 数据文件, 当insert 的数据需要保留时，会用到这个功能

    mock -c -f <db file> <output edn file>

 复制edn 数据到mongodb

    mock -c -m <edn file>

 复制edn  数据到小程序云数据库

    mock -c -p <edn file>    (第二个参数可选 <"remove"|"append">）

 从模板产生工程 (ice相关模板已经迁址ice 项目下)

    mock -t <template-name > <project-name>
    例如： mock -t ice_design_school myschool  (通过模板生成)
        mock -t mock/order.edn.info myorder  （通过 info文件 生成）
        mock -t -b mock/hr.edn ../../test/rl/js/temp/myhr ice_design   （从数据生成）
        mock -t -b mock/ecommerce.edn ../taro-shop-graphql/client/mock/temp/ec ice_design

 ** 注意：如果不是最新版本，请先用lein run，然后lein uberjar，再用mock （mock只是一个批处理而已）
## Examples

java -jar mock-fc.jar mock/simple1.json

mock taro-library.json

mock -e fruits.edn

打开 http://localhost:3000/ql 查看graphql server

### Bugs
 please post issue

## why we create this product
 在开发web app和小程序的过程中，我们发现现有的mock server工具，并不能自动产生带关联性质的schema，
 我们制作过手写schema的工具（见workbench），深知手写schema是多么容易出错和麻烦。如果从数据推导出schema，
 可以最大程度保证schema和数据的一致性，避免错误。最关键的是，它很方便、省事，能提高我们的开发效率。

 推荐配合我们的前端graphql开发包，它们会让开发速度 —— 飞起来！

## License

Copyright © 2019 Ioobot 栋天科技

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
