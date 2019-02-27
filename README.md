# vue-mobile-emoji
> 基于Vue2.0开发的emoji表情组件，一行代码快速集成。

![演示gif](https://github.com/EastblueOkay/vue-mobile-emoji/blob/master/static/vue-mobile-emoji.gif)

## 安装
```js
拷贝 vue-mobile-emoji 文件夹到项目
```
## 使用
```js
import vme from "./vue-mobile-emoji/vme.vue"

//作为组件使用
<vme></vme>
```


### 配置
```html
   <vme @emojiSelected="emojiSected"></vme>

   <h3 contenteditable="true" ref="demo">
     vue-mobile-emoji (可编辑)
   </h3>
```

```javascript
  import vme from "./components/vue-mobile-emoji/vme.vue"

  export default {
    name: 'app',
    components: {
      vme
    },
    methods: {
      emojiSected(emoji, base64) {
        const dom = this.$refs.demo.innerHTML + `<img src="${base64}" />`;
        this.$refs.demo.innerHTML = dom;
      }
    }
  }
```

### Props

|    name    |    Description   |   type   |default|
| -----------------  | ---------------- | :--------: | :----------: |
| perPage       | 每页横向表情个数 |Number| 8

### Events

| name | Description   |
| :--------:   | -----  |
|    emojiSelected    |  选择表情后触发，传递emoji code（用于转发存储）和emoji base64编码（可用于显示）

