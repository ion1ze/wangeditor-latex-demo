<script lang="jsx">
import { defineComponent,h,watch,onMounted,nextTick} from "vue";
import "katex/dist/katex.css";
import katex from "katex";

export default defineComponent({
  name: "HtmlViewer",
  props: {
    source: {
      type: String,
      default: ()=> ""
    }
  },
  setup(props){
    const renderFormulas = ()=> {
      // 获取所有含有 data-w-e-type="formula" 属性的元素
      const formulas = document.querySelectorAll('[data-w-e-type="formula"]');
      // 遍历每个元素，渲染数学公式
      formulas.forEach(formula => {
        const latex = formula.getAttribute('data-value');
        katex.render(latex, formula, {
          throwOnError: false, // 如果公式有错误，不会抛出异常
          displayMode: false   // 是否将公式渲染为显示模式（行级模式）
        });
      });
    }

    onMounted(()=>{
      renderFormulas();
    });

    watch(()=>props.source,(newValue,oldValue)=>{
      if(newValue === oldValue) return;
      nextTick(()=>{
        renderFormulas();
      });
    });

    return ()=> h("div",{class: "markdown-body",innerHTML: props.source}); 
  },
});
</script>