<template>
  <div>
    <button>
      <h1 @click="saveTemplates">Save Template</h1>
    </button>
    <div v-for="(page, i) in pages" :key="page.id">
      <!-- This is done so that we dont have to increase index again and again -->
      <slot v-bind="++i"></slot>
      <h4 style="padding-left: 2rem">Page {{ i }}</h4>
      <div
        style="border: 1px solid green; margin: 10px"
        :id="`editor-${page.id}`"
      ></div>

      <div style="text-align: center">
        <button @click="addNewPage(i)">+ New Page</button>
      </div>
    </div>
  </div>
</template>
 
<script>
import EditorJS from "@editorjs/editorjs";
import { v4 as uuidv4 } from "uuid";

const useId = uuidv4();

export default {
  name: "App",
  mounted() {},
  data() {
    return {
      pages: [
        {
          id: useId,
          ["editor-" + useId]: new EditorJS({
            holder: "editor-" + useId,
            data: {
              blocks: [
                {
                  id: "8bizPE-LLh",
                  type: "paragraph",
                  data: {
                    text: "Cover Page",
                    level: 2,
                  },
                },
              ],
            },
          }),
        },
      ],
      formData: {
        template: "",
        editorDatas: [],
      },
    };
  },
  methods: {
    addNewPage(i) {
      const uniqId = useId + Math.random().toString(16).slice(2);

      const newObj = {
        id: uniqId,
      };

      this.pages.splice(i, 0, newObj);

      newObj["editor-" + uniqId] = new EditorJS({
        holder: "editor-" + uniqId,
        data: {
          blocks: [
            {
              id: "8bizPE-LLh",
              type: "paragraph",
              data: {
                text: "The World Is Yours " + uniqId,
              },
            },
          ],
        },
      });
    },
    saveTemplates() {
      this.formData.template = "New Template";
      this.pages.forEach((page, i) => {
        page[`editor-${page.id}`].save().then((outputData) => {
          this.formData.editorDatas.push(outputData);
        });
      });

      console.log(this.formData);
    },
  },
};
</script>