<template>
  <div id="app">
    <FroalaEditor
      :label="editorContentLaebl"
      v-model="basicForm.editorContent"
      @change="getEditorContent"
    />
  </div>
</template>

<script>
import FroalaEditor from './components/FroalaEditor'

export default {
  name: 'app',

  components: {
    FroalaEditor,
  },

  mounted() {
    this.getApiData()
  },

  data () {
    return {
      editorContentLaebl: '',
      basicForm: {
        editorContent: '',
      },
    }
  },

  methods: {
    getApiData() {
      console.log('開始存取遠端資料!')
      const text = '<p style="color: red">這是從遠端存取的文字片段</p>'
      window.setTimeout(() => {
        console.log('遠端資料存取完成!')
        this.text = text
        this.editorContentLaebl = text
      }, 1000)
    },
    getEditorContent(content) {
        this.basicForm.editorContent = content
    },
  },
}
</script>