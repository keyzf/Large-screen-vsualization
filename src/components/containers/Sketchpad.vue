/**
* Sketchpad 画板
*/

<style scoped lang="less" rel="stylesheet/less">
  .sketchpad-box {
    overflow: auto;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;

    .sketchpad {
      display: inline-block;
      width: 1200px;
      height: 1200px;
      position: absolute;
      left: 50%;
      margin-left: -600px;
      margin-top: 100px;
      margin-bottom: 100px;
      z-index: 100;
      background: #ffffff;
      background-image: linear-gradient(#f4f4f4 2px,transparent 0),
      linear-gradient(90deg, #f4f4f4 2px,transparent 0),
      linear-gradient(hsla(0,0%,100%,.3) 1px,transparent 0),
      linear-gradient(90deg,hsla(0,0%,100%,.3) 1px,transparent 0);
      background-size:20px 20px,20px 20px,15px 15px,15px 15px;
      box-shadow: 0 0 2px 2px rgba(0, 0, 0, .1);
    }
    .inputBox {
      display: none;
    }
  }
</style>

<template>
  <div class="sketchpad-box" :style="boxStyle" @dblclick="ondblclickPad">
    <div class="sketchpad" id="sketchpad" @dblclick.stop>
      <!-- 文本输入框 -->
      <input class="inputBox" autofocus value="">
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Sketchpad',
    data () {
      return {
        // 面板样式
        panelStyle: {
          left: null,
          right: null
        }
      }
    },
    computed: {
      boxStyle () {
        const _t = this
        const boxStyle = {}
        const keys = Object.keys(_t.panelStyle)
        keys.forEach(key => {
          const panelStyle = _t.panelStyle[key]
          if (panelStyle) {
            // 位置数据
            const positionVal = isNaN(parseInt(panelStyle[key])) ? 0 : parseInt(panelStyle[key])
            const width = isNaN(parseInt(panelStyle.width)) ? 0 : parseInt(panelStyle.width)
            boxStyle[key] = positionVal + width + 'px'
          }
        })
        return boxStyle
      }
    },
    methods: {
      ondblclickPad () {
        const _t = this
        _t.$bus.$emit('editor/pad/dblclick')
      }
    },
    created () {
      const _t = this
      _t.$bus.$on('editor/panel/toggle', function (data) {
        _t.$set(_t.panelStyle, data.name, data.style)
      })
    }
  }
</script>
