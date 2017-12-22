<template>
  <div class="vme">
    <div class="emoji-container">
      <div class="emoji-category" ref="emojiCon">
        <span v-for="(emoji, i) in emojis" :style="emojiWidth" :class="_emojiName(emoji)"
              @click="emojiClick(emoji, i, $event)"
        >
          <img :src="require('./common/emoji/' + _emojiName(emoji))"/>
        </span>
      </div>
    </div>
    <div class="bar-container">
      <div v-for="(item, index) in barControl" :class="{'actived': activedIndex == index}"
           :key="index"
           @click.stop="barClick(item, index)"
      >
        {{item}}
      </div>
    </div>
  </div>
</template>

<script>
  import emojiData from "./common/emoji-data"

  export default {
    props: {
      perPage: {
        default: 8,
        type: Number
      }
    },
    data() {
      return {
        activedIndex: 0
      };
    },
    computed: {
      emojiWidth() {
        const margin = 5;
        const width = (document.body.clientWidth - (this.perPage + 1) * margin) / this.perPage;
        return {width: width + 'px'};
      },
      emojis() {
        const ret = this.barControl.map(item => {
          return Object.keys(emojiData[item])
        });
        let arr = [];
        ret.map((item, index) => {
          this.rangeKeys[index] = this._emojiName(item[0]);
          item.map((text) => {
            arr.push(text);
          })
        });
        return arr;
      }
    },
    created() {
      this._initUI();
    },
    mounted() {
      this.$nextTick(()=> {
        this._caculateRange();
      });
    },
    methods: {
      _caculateRange() {
        let arr = [];
        arr = this.rangeKeys.map((item) => {
          if (document.getElementsByClassName(item).length > 0) {
            return document.getElementsByClassName(item)[0].offsetLeft;
          }
        });
        this.widthRange = arr;
        this._listenScroll();

        this.barClick("", 0);
      },
      _listenScroll() {
        // 监听滚动事件
        this.$refs.emojiCon.addEventListener('scroll', ()=> {
          const scrollLeft = this.$refs.emojiCon.scrollLeft;
          for(var i = 0; i < this.widthRange.length - 1; i++){
            if(scrollLeft >= this.widthRange[i] && scrollLeft < this.widthRange[i+1]){
              this.activedIndex = i;
              break;
            }else if(scrollLeft >= this.widthRange[this.widthRange.length - 1]){
              this.activedIndex = this.widthRange.length - 1;
              break;
            }else{
              this.activedIndex = 0;
            }
          }
        });
      },
      _emojiName(code) {
        return code.replace(/:/g, '') + ".png";
      },
      _initUI() {
        // 初始化bar control数据
        this.barControl = ['表情', '自然', '物品', '地点', '符号'];
        this.rangeKeys = [];
      },
      barClick(item, index) {
        this.activedIndex = index;
        this.$refs.emojiCon.scrollLeft = index === 0 ? 0 :this.widthRange[index];
      },
      emojiClick(emoji, index, e){
        const dom = document.getElementsByClassName(this._emojiName(emoji))[0];
        dom && dom.setAttribute('selected','selected');

        setTimeout(()=> {
          dom.removeAttribute('selected');
        }, 200);

        const pngName = this._emojiName(emoji);
        this.$emit('emojiSelected', emoji, require('./common/emoji/' + pngName));
      }
    }
  }
</script>

<style>
  .vme {
    background: #F2F4F7;
    width: 100%;
    height: 200px;
    position: relative;
  }

  .vme .emoji-container {
    position: absolute;
    top: 0;
    width: 100%;
    bottom: 36px;
  }

  .vme .emoji-container .emoji-category {
    box-sizing: border-box;
    padding: 5px 5px 0 5px;
    width: 100%;
    overflow: scroll;
    overflow-y: hidden;
    display: flex;
    flex-wrap: wrap;
    flex-direction: column;
    height: 100%;
  }

  .vme .emoji-container .emoji-category span {
    border-radius: 5px;
    text-align: center;
    height: 38px;
    line-height: 38px;
  }
  .vme .emoji-container .emoji-category span[selected]{
    background-color: #d8d8d8;
  }

  .vme .emoji-container .emoji-category span img {
    margin-top: 5px;
    width: 28px;
    height: 28px;
  }

  .vme .bar-container {
    position: absolute;
    left: 0;
    width: 100%;
    bottom: 0;
    height: 35px;
    border-top: 1px solid #ddd;
    border-bottoM: 1px solid #ccc;
  }

  .vme .bar-container {
    display: flex;
  }

  .vme .bar-container div {
    flex: 1 1;
    text-align: center;
    line-height: 35px;
    height: 35px;
    border-right: 1px solid #ddd;
    font-size: 13px;
    color: #888;
    font-weight: 300;
  }

  .vme .bar-container div:last-child {
    border-right: none;
  }

  .vme .bar-container .actived {
    background: #E0E5EE;
    font-weight: 350;
  }
</style>
