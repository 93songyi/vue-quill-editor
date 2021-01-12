<template>
  <div class="container">
    <el-form>
      <el-form-item label="富文本使用">
        <!-- 基础富文本 -->
        <!-- <vue-quill-editor
          :content="vueContent"
          @content="setContent"
          :id="content + '--only'"
          :refs="content + '--only'"
        ></vue-quill-editor> -->
        <!-- 支持图片拖拽以及放大缩小 -->
        <!-- <vue-quill-editor-drop-resize
          :content="vueContent"
          @content="setContent"
          :id="content + '--only'"
          :refs="content + '--only'"
        ></vue-quill-editor-drop-resize> -->
        <!-- 支持图片上传服务器 -->
        <vue-quill-editor-image-extend
          :content="vueContent"
          @content="setContent"
          :id="content + '--only'"
          :refs="content + '--only'"
        ></vue-quill-editor-image-extend>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import VueQuillEditor from './Quill-Editor'
import VueQuillEditorDropResize from './Quill-Editor-Drop-Resize'
import VueQuillEditorImageExtend from './Quill-Editor-Image-Extend'

export default {
  name: 'Home',
  components: {
    VueQuillEditor,
    VueQuillEditorDropResize,
    VueQuillEditorImageExtend
  },
  props: { content: String },
  data () {
    return {
      vueContent: null // 富文本变量
    }
  },
  methods: {
    // 更新富文本内容
    setContent (val) {
      if (val) this.content = val
    }
  }
}
</script>

<style>
</style>
