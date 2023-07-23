<template>
  <!-- <MarkdownViewer :source="content"/> -->
  <HtmlViewer :source="content" />
  <div class="editor">
    <Toolbar
      style="border-bottom: 1px solid #ccc"
      :defaultConfig="CONFIG.toolbar"
      :editor="editorRef"
    />
    <Editor
      style="height: 500px; overflow-y: hidden;"
      v-model="content"
      :defaultConfig="CONFIG.editor"
      @onCreated="handleCreated"
    />
  </div>
</template>
<script setup>
import '@wangeditor/editor/dist/css/style.css';
import { onBeforeUnmount, ref, shallowRef,onMounted } from 'vue';
import { Editor, Toolbar } from '@wangeditor/editor-for-vue';
import { Boot } from '@wangeditor/editor'
import formulaModule from '@wangeditor/plugin-formula'

// 注册。要在创建编辑器之前注册，且只能注册一次，不可重复注册。
Boot.registerModule(formulaModule);

// import MarkdownViewer from './MarkdownViewer.vue';
import HtmlViewer from './HtmlViewer.vue';

const CONFIG = {
  toolbar: {
    insertKeys: {
      index: 0,
      keys: [
        'insertFormula', // “插入公式”菜单
      ],
    },
  },
  editor: {
    hoverbarKeys: {
      formula: {
        menuKeys: ['editFormula'], // “编辑公式”菜单
      },
    },
  }
}

const content = ref('<p><span data-w-e-type="formula" data-w-e-is-void data-w-e-is-inline data-value="c = \\pm\\sqrt{a^2 + b^2}"></span></p>');

const editorRef = shallowRef();

onBeforeUnmount(()=>{
  const editor = editorRef.value;
  if(editor == null) return;
  editor.destory();
});

const handleCreated = (editor)=> {
  editorRef.value = editor;
}
</script>