<script lang="ts" setup>
import EditorJS from '@editorjs/editorjs';
import Header from '@editorjs/header';
import List from '@editorjs/list';
import Table from '@editorjs/table';
import Quote from '@editorjs/quote';
import Image from '@editorjs/image';

const id = 'editorjs';
const editor = new EditorJS({
  onChange: (api, event) => {
    console.log("Now I know that Editor's content changed!", event);
  },
  autofocus: true,
  placeholder: 'write something',
  logLevel: 'ERROR',
  onReady: () => {
    console.log('Editor.js is ready to work!');
  },
  data: {
    blocks: [],
  },
  holder: id,
  tools: {
    image: {
      class: Image,
      config: {
        endpoints: {
          byFile: 'http://localhost:8008/uploadFile', // Your backend file uploader endpoint
          byUrl: 'http://localhost:8008/fetchUrl', // Your endpoint that provides uploading by Url
        },
      },
    },
    quote: {
      class: Quote,
      inlineToolbar: true,
      shortcut: 'CMD+SHIFT+O',
      config: {
        quotePlaceholder: 'Enter a quote',
        captionPlaceholder: "Quote's author",
      },
    },
    table: {
      class: Table,
      inlineToolbar: true,
      config: {
        rows: 2,
        cols: 3,
      },
    },
    header: {
      class: Header,
      inlineToolbar: ['link'],
    },
    list: {
      class: List,
      inlineToolbar: true,
    },
  },
});

editor.isReady
  .then(() => {
    console.log('Editor.js is ready to work!');
    /** Do anything you need after editor initialization */
  })
  .catch((reason) => {
    console.log(`Editor.js initialization failed because of ${reason}`);
  });

const save = () => {
  editor
    .save()
    .then((outputData) => {
      console.log('Article data: ', outputData);
    })
    .catch((error) => {
      console.log('Saving failed: ', error);
    });
};
</script>
<template>
  <div autofocus="'autofocus'" :id="id" class=""></div>
  <button @click="save">save</button>
</template>
