<!--
  label:   用于给富文本赋予默认值, 清空值label=" ",必须加空格, 否則不会清空
  v-model: 单向绑定，由于富文本监听问题，不能用 v-model 赋予默认值
-->
<template>
  <div class="froalaEditor">
    <froala
      :tag="'textarea'"
      :config="froalaConfig"
      v-model="body"
    />
  </div>
</template>

<script>
import Vue from 'vue'

// Import Froala Editor 
import 'froala-editor/js/plugins.pkgd.min.js';
// Import third party plugins
import 'froala-editor/js/third_party/embedly.min';
import 'froala-editor/js/third_party/font_awesome.min';
import 'froala-editor/js/third_party/spell_checker.min';
import 'froala-editor/js/third_party/image_tui.min';
// Import Froala Editor css files.
import 'froala-editor/css/froala_editor.pkgd.min.css';
// Import Froala languages
import 'froala-editor/js/languages/zh_cn'
import 'froala-editor/js/languages/zh_tw'
import 'froala-editor/js/languages/th'
// Import and use Vue Froala lib.
import VueFroala from 'vue-froala-wysiwyg'
Vue.use(VueFroala)

export default {
  props: {
    // 显示的工具列表
    placeholder: {
      type: String
      //   required: true
    },
    height: {
      type: Number
    },
    value: {
      type: String,
      default: null
    },
    label: {
      type: String,
      default: ''
    },
    index: {
      type: Number,
      default: 1
    }
  },
  name: 'froala-editor',
  data() {
    const that = this
    return {
      editor: null,
      body: null,
      froalaConfig: {
        toolbarButtons: [
          'undo',
          'redo',
          'clearFormatting',
          'bold',
          'italic',
          'underline',
          'strikeThrough',
          'fontFamily',
          'fontSize',
          'textColor',
          'backgroundColor',
          'inlineStyle',
          'paragraphFormat',
          'align',
          'formatOL',
          'formatUL',
          'outdent',
          'indent',
          'quote',
          'insertLink',
          'insertImage',
          'insertVideo',
          'embedly',
          'insertFile',
          'insertTable',
          'emoticons',
          'specialCharacters',
          'insertHR',
          'selectAll',
          'print',
          'spellChecker',
          'html',
          'help',
          'fullscreen'
        ],
        // theme: "dark",//主题
        placeholderText: this.placeholder || '请在此输入',
        language: 'zh_cn', //国际化
        imageAllowedTypes: ['jpeg', 'jpg', 'png'],
        imageInsertButtons: ['imageUpload'],
        imageEditButtons: ['imageAlign', 'imageCaption', 'imageRemove', '|', 'imageLink', 'linkEdit', 'linkRemove', '-', 'imageDisplay', 'imageStyle', 'imageAlt', 'imageSize'],
        quickInsertButtons: ['image', 'table', 'ul', 'ol', 'hr'], //快速插入项
        // toolbarVisibleWithoutSelection: true,//是否开启 不选中模式
        // disableRightClick: true,//是否屏蔽右击
        // colorsHEXInput: false, //关闭16进制色值
        toolbarSticky: false, //操作栏是否自动吸顶,
        zIndex: 2501,
        // height: this.height || '400',
        // autofocus: true,
        events: {
          initialized: function() {
            that.editor = this
            that.body = that.value
          },
          // blur: () => {
          //   console.log('blur....');
          //   const text = this.getSimpleText(this.body)
          //   this.$emit('blur', text)
          // },
          contentChanged: () => {
            console.log('contentChange....');
            const text = this.getSimpleText(this.body)
            this.$emit('change', text)
          },
          'image.beforeUpload': function(files) {
              // 如果要自行定義圖片上傳方法，可在這做攔截
              const fr = new FileReader()
              fr.readAsDataURL(files[0])
              fr.onload = (e) => {
                const result = e.target.result
                that.editor.image.insert(result, null, null, that.editor.image.get())
              }
              that.editor.popups.hideAll()
              // Stop default upload chain.
              return false
          },
        }
      },
    }
  },
  watch: {
    body: function(newVal, old) {
      if (old !== newVal) {
        let val = this.getSimpleText(this.editor.html.get(true))
        this.$emit('input', val)
      }
    },
    label: function(newVal, old) {
      if (old !== newVal) {
        this.editor.html.set(newVal)
      }
    }
  },
  mounted() {
    this.setIndex(this.index)
  },
  methods: {
    // 更改富文本层级
    setIndex(val) {
      this.$nextTick(() => {
        let dv = document.getElementsByClassName('fr-box')
        for (let i in dv) {
          if (!dv[i].style) {
              return
          }
          dv[i].style.cssText = 'z-index:' + val
        }
      })
    },
    // 富文本中提取纯文本
    getSimpleText: (html) => {
      var re1 = new RegExp('<p data-f-id="pbf".+?</p>', 'g') // 匹配html标签的正则表达式，"g"是搜索匹配多个符合的内容
      var msg = html.replace(re1, '') // 执行替换成空字符
      return msg
    },
    // 重置富文本
    resetValue(val) {
      this.body = val || ''
    }
  }
}
</script>

<style lang="scss">
  a[href="https://froala.com/wysiwyg-editor"], a[href="https://www.froala.com/wysiwyg-editor?k=u"] {
    padding: 0 !important;
    background-color: transparent !important;
    color: transparent !important;
    z-index: -999 !important;
    transform: translateY(-200%);
  }

  .froalaEditor {
    word-break: break-word;
    overflow: hidden;
  }
</style>