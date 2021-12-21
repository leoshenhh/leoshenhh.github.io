---
title: cypress
date: 2021-11-20 14:01:16
tags:
---

### 安装cypress

```bash
yarn add --dev cypress
```



### 打开cypress

```bash
./node_modules/.bin/cypress open
```



### 运行cypress

+ 可以通过点击GUI界面的`spec`运行
+ 可以通过命令行运行
+ `--headed`  为展示测试过程
+ `--spec` 后接测试用例 `js`

```bash
./node_modules/.bin/cypress run --spec cypress\integration\examples\actions.spec.js --headed
```



### 忽略测试用例

`cypress.json` 中添加以下代码，忽略这些测试用例

```json
	{
        "ignoreTestFiles": ["*.hot-update.js"."**/examples/*.*"]
    }
```

### 写一个测试用例

在`integration`文件夹下，新建一个`.spec.js`后缀的文件

```js
describe('百度',()=>{
    it('能搜索',()=>{
        cy.visit('https://baidu.com')
        cy.get('input#kw').type('腾讯')
        cy.contains('百度一下').click()
        cy.contains('腾讯首页').should('exist')
    })
})
```

+ `describe` 为整个测试用例 第一个参数`title`为用例标题，第二个参数为箭头函数
+ `it`为测试用例的其中一步,第一个参数为`title`为步骤标题，第二个参数为箭头函数
+ `cy.visit` ：访问一个`URL`
+ `cy.get` ：获取`dom`元素
+ `cy.type`: 输入文字
+ `cy.contains`: 获取包含 *百度一下* 的 `dom` 元素
+ `cy.click()`: 点击一个`dom`元素
+ `cy.should('exist')`: `dom`元素应该存在
