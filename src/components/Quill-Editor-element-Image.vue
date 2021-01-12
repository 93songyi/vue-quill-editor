<template>
  <div class="quill-wrap" :id="id">
    <el-upload
      :id="id + 'uploadImg'"
      class="upload-demo"
      v-show="false"
      :action="severUrl"
      name="file"
      multiple
      :on-success="uploadSuccess"
      :on-error="uploadError"
      :before-upload="beforeUplaod"
    >
      <el-button size="small" type="primary" class="button">点击上传</el-button>
    </el-upload>
    <quill-editor
      style="width: 335px; padding: 0"
      class="ql-editor"
      v-model="content"
      :ref="refs"
      :options="editorOptions"
      @blur="onEditorBlur($event)"
      @focus="onEditorFocus($event)"
      @change="onEditorChange($event)"
    ></quill-editor>
  </div>
</template>

<script>
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'
import { quillEditor } from 'vue-quill-editor'

const toolbarOptions = [
  ['bold', 'italic', 'underline', 'strike'],
  ['blockquote', 'code-block'],
  [{ header: 1 }, { header: 2 }],
  [{ list: 'ordered' }, { list: 'bullet' }],
  [{ header: [1, 2, 3, 4, 5, 6, false] }],
  [{ script: 'sub' }, { script: 'supper' }],
  [{ indent: '-1' }, { indent: '+1' }],
  [{ size: ['small', false, 'large', 'huge'] }],
  [{ color: [] }, { background: [] }],
  [{ align: [] }],
  ['link', 'image', 'video'],
  ['clean']
]

export default {
  name: 'VueQuillEditorElementImage',
  props: ['pcontent', 'id', 'refs'],
  components: { quillEditor },
  data () {
    const id = this.id
    return {
      content: '', // 富文本变量
      length: '',
      severUrl: imageURL, // 上传图片服务器地址
      // 富文本
      editorOptions: {
        toolbar: {
          container: toolbarOptions,
          handlers: {
            image: function (value) {
              if (value) {
                document.querySelector(`#${id}uploadImg input`).click()
              } else {
                this.quill.format('image', false)
              }
            }
          }
        }
      }
    }
  },
  computed: {
    editor () {
      return this.$refs[this.refs].quill
    }
  },
  created () {
    this.content = this.pcontent
  },
  watch: {
    pcontent (newVal, oldVal) {
      if (newVal) this.content = newVal
    }
  },
  methods: {
    onEditorChange (editor) {
      this.$emit('content', {
        value: this.content,
        prop: this.pcontent
      })
    },
    onEditorFocus (event) {
      this.length = event.getSelection().index
    },
    onEditorBlur (editor) {
      console.log(editor)
    },
    // 图片上传成功并移动光标
    uploadSuccess (res, file) {
      // 获取富文本组件实例
      let quill = this.$refs[this.refs].quill
      if (res && res.status === 0) {
        // 获取光标所在位置
        let length = quill.getSelection().index
        // 插入图片insertEmbed()三个参数，前两个固定即可
        quill.insertEmbed(length, 'image', res.content)
        // 调整光标到最后
        quill.setSelection(length + 1)
      } else {
        this.$message.error('图片插入失败')
      }
    },
    // 图片上传失败抛异常
    uploadError () {
      this.$message.error('图片上传失败')
    },
    // 图片上传前校验
    beforeUplaod (file) {
      let isPass = false
      if (
        file.type === 'image/png' ||
        file.type === 'image/jpeg' ||
        file.type === 'image/jpg'
      ) {
        isPass = true
      }
      const isLt = file.size / 1024 / 1024 < 5
      if (!isPass) {
        this.$message.error('当前仅支持上传图片JPG/jpeg/png格式!')
        return false
      }
      if (!isLt) {
        this.$message.error('上传图片大小不能超过 5MB!')
        return false
      }
      return isPass && isLt
    },
    handleCustm (node, delta) {
      let opsList = []
      delta.ops.forEach((ops) => {
        if (ops.insert && typeof ops.insert === 'string') {
          opsList.push({ insert: ops.insert })
        }
      })
    }
  }
}
</script>

<style>
</style>
