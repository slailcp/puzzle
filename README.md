@[TOC](这里写目录标题)

# vuePuzzle
# 文档地址

[vuePuzzle-文档地址](https://blog.csdn.net/sllailcp/article/details/139597472?spm=1001.2014.3001.5502)

# 案例地址

[vuePuzzle-案例地址](https://slailcp.github.io/vuePuzzle/index.html)

# 源码地址

[vuePuzzle-源码地址](https://github.com/slailcp/puzzle)



# npm 安装
npm i --save vuePuzzle

# 引入
import {Puzzle1} from 'vuePuzzle'

# 使用

## 单个拼图

```js
<Puzzle1 />
```

## 设置大小（默认100px）

```js
<Puzzle1 size="150px" />

<Puzzle1 size="1.5rem" />
```

## 设置背景色，边框粗细，边框颜色

```js
<Puzzle1 bg-color="#88fb92" border-color="#FF0000" borderWidth="10" />

/**背景色可以设置透明度 */
<Puzzle1 bg-color="rgba(0,0,0,0.5)" border-color="#FF0000" borderWidth="10" />
```

## 设置阴影,阴影程度，阴影扩散大小,

```js
// 注意：需要设置index或者mark唯一标识
<Puzzle1 mark="10" shadowColor="#FF0000" shadowRatio="0.4" shadowSpread="100" />
```

## 设置渐变

```js
// 注意：需要设置index或者mark唯一标识
/**默认线性从上倒下渐变， */
<Puzzle1 mark="0" :colors="['#ffff00', '#88fb92', '#ff0000', '#ffff00']">默认<br />线性渐变<br />从上到下</Puzzle1>

/**线性渐变设置渐变方向和渐变透明度 */
<Puzzle1 mark="1" :colors="['#ffff00', '#88fb92', '#ff0000', '#ffff00']" 
:gradient="{ type: 'linear', opacity: 0.8, point: { x1: `725`, y1: `725`, x2: `0`, y2: `0` } }"
>渐变角度</Puzzle1>

/**径向渐变 */
<Puzzle1 mark="2" :colors="['#ffff00', '#88fb92', '#ff0000', '#ffff00']" 
:gradient="{ type: 'radial', opacity: 0.9, point: { cx: '40%', cy: '60%', r: '50%', fx: '50%', fy: '50%' } }"
>径向渐变</Puzzle1>
```

## 蒙版-裁切图片

```js
// 注意：需要设置index或者mark唯一标识
 <Puzzle1 mark="2" :imgpath="require('@/assets/hw.png')" />
```

## 列表（左侧和底部完全填充）

```js
/*
注意：根据单个拼图的大小(size)和每一行的个数，需要设置父节点的宽高,并设置overflow:hidden,隐藏上边和右边凸出的部分。 
*/
<div style="width: 600px; height: 600px; font-size: 0; overflow: hidden; border-radius: 12px">
  <div v-for="(f, fi) of hightlist1" :key="fi">
    <Puzzle1 mark="mark36" :index="fi" :total="hightlist1.length" linenum="6" style="float: left"/>
    <div>
</div>

const hightlist1 = ref([...Array(36).keys()]); // 36组数据
```

# slot

```js
<Puzzle1>张三</Puzzle1>
```


# props

| name | 描述 | 类型  | 默认值 |
|-|---|--|--|
| bgColor  | 背景色 | String | "#F2F2F3" |
| borderColor  | 背景色 | String | "#3E5185" |
| borderWidth  | 背景色 | [Number, String] | 2 |
| index  | 背景色 | [Number, String] | 0 |
| shadowColor  | 背景色 | String | 无 |
| shadowRatio  | 阴影明显程度 | [Number, String] | 0.2 |
| shadowSpread  | 阴影扩散程度 | [Number, String] | 125 |
| colors  | 渐变色数组 | Array | [] |
| total  | 如果是一个列表的话，一共多少个 | [Number, String] | 0 |
| linenum  | 如果是一个列表的话，一行多少个 | [Number, String] | 6 |
| size  | 拼图大小 | String | 100px |
| mark  | 当前拼图的标识，防止内部使用特效重复 | String | 'one' |
| imgpath  | 背景图片 | String | '' |
| gradient  | 渐变配置 | gradientObj | - |


### gradientObj

| name | 描述 | 类型  | 默认值 | 备注|
|-|---|--|--|--|
| type (linear/radial)  | 渐变类型 | String | linear |linear:线性渐变， radial:径向渐变 |
| opacity  | 渐变透明度 | [Number, String] | 1 | - |
| point  | 渐变起始点 | Object | {  x1: "362.5",y1: "725",x2: "362.5",y2: "0" } |  |

```
注意：
线性渐变格式类型：{x1: "362.5", y1: "725", x2: "362.5", y2: "0"}
径向渐变格式类型：{ cx: '40%', cy: '60%', r: '50%', fx: '50%', fy: '50%' }
```


