<script setup>
import BulletList from "@tiptap/extension-bullet-list";
import Document from "@tiptap/extension-document";
import ListItem from "@tiptap/extension-list-item";
import OrderedList from "@tiptap/extension-ordered-list";
import Paragraph from "@tiptap/extension-paragraph";
import Text from "@tiptap/extension-text";
import { Editor, EditorContent } from "@tiptap/vue-3";
import { defineAsyncComponent, onBeforeUnmount, onMounted, ref } from "vue";

const ToggleButton = defineAsyncComponent(() =>
  import("./components/toggleButton.vue")
);

const editor = ref(null);
const content = ref("Sample Text");

// Extend the bullet list
const MyCustomBullet = () => {
  return BulletList.extend({
    name: "myBulletList",

    // Set the default class
    addAttributes() {
      return {
        class: {
          default: "list-disc",
        },
      };
    },

    // Add command to toggle class name
    addCommands() {
      return {
        ...this.parent?.(),
        toggleClass:
          (className) =>
          ({ commands, chain }) => {
            console.log(this.name);
            if (!this.editor.isActive(this.name)) {
              return chain().toggleBulletList().updateAttributes(this.name, {
                class: className,
              });
            }

            if (!this.editor.isActive(this.name, { class: className })) {
              return commands.updateAttributes(this.name, {
                class: className,
              });
            }
          },
      };
    },
  });
};

onMounted(() => {
  let myBulletList = MyCustomBullet();
  editor.value = new Editor({
    extensions: [
      Document,
      Paragraph,
      Text,
      myBulletList,
      ListItem,
      OrderedList,
    ],
    editorProps: {
      // Style the editor
      attributes: {
        class: "",
        style: `min-height: 256px; background-color: white; color: black; outline: none; width: 24rem; border-radius: 0.5rem; text-align: start`,
      },
    },
    content: `
        <p>Bullet List</p>
        <ul>
          <li>A list item</li>
          <li>And another one</li>
        </ul>

        <p>Ordered List</p>
        <ol>
          <li>A list item</li>
          <li>And another one</li>
        </ol>
      `,
  });
});

onBeforeUnmount(() => {
  editor.value.destroy();
});
</script>

<template>
  <div>
    <div class="actions">
      <ToggleButton
        label="None"
        :active="editor?.isActive('myBulletList', { class: 'list-none' })"
        @click="editor?.chain().focus().toggleClass('list-none').run()"
      />
      <ToggleButton
        label="Disc"
        :active="editor?.isActive('myBulletList', { class: 'list-disc' })"
        @click="editor?.chain().focus().toggleClass('list-disc').run()"
      />
      <ToggleButton
        label="Square"
        :active="editor?.isActive('myBulletList', { class: 'list-square' })"
        @click="editor?.chain().focus().toggleClass('list-square').run()"
      />
      <ToggleButton
        label="Circle"
        :active="editor?.isActive('myBulletList', { class: 'list-circle' })"
        @click="editor?.chain().focus().toggleClass('list-circle').run()"
      />
      <ToggleButton
        label="Decimal"
        :active="editor?.isActive('myBulletList', { class: 'list-decimal' })"
        @click="editor?.chain().focus().toggleClass('list-decimal').run()"
      />
    </div>

    <EditorContent :editor="editor" />
  </div>
</template>

<style>
.list-disc {
  list-style-type: disc !important;
}

.list-square {
  list-style-type: square !important;
}

.list-none {
  list-style-type: none !important;
}

.list-circle {
  list-style-type: circle !important;
}

.list-decimal {
  list-style-type: decimal !important;
}

.actions {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
</style>
