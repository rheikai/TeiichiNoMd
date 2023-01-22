<template>
<main>
    <div id="editor">
        <h2>エディタ</h2>
        <textarea @keyup="onMdEditorChanged($event.target.value)"></textarea>
    </div>
    <div id="preview">
        <h2>プレビュー</h2>
        <div v-html="preview">
        </div>
    </div>
</main>
</template>

<script>
import MarkdownIt from "markdown-it";
const md = new MarkdownIt();

export default {
    data() {
        return {
            preview: ""
        }
    },
    methods: {
        onMdEditorChanged(value) {
            const symbols = {
                "ー": "-",
                "＾": "^",
                "￥": "\\",

                "！": "!",
                "”": "\"",
                "＃": "#",
                "＄": "$",
                "％": "%",
                "＆": "&",
                "’": "'",
                "（": "(",
                "）": ")",
                "＝": "=",
                "〜": "~",
                "｜": "|",

                "＠": "@",
                "「": "[",
                "；": ";",
                "：": ":",
                "」": "]",
                "、": ",",
                "。": ".",
                "・": "/",
                "￥": "\\",

                "｀": "`",
                "｛": "{",
                "＋": "+",
                "＊": "*",
                "｝": "}",
                "＜": "<",
                "＞": ">",
                "？": "?",
                "＿": "_",

                "　": " ",
            };
            for (let [key, val] of Object.entries(symbols)) {
                value = value.replaceAll(key, val);
            }

            for (let i = "０".charCodeAt(0); i <= "９".charCodeAt(0); i++) {
                value = value.replaceAll(String.fromCharCode(i), String.fromCharCode("0".charCodeAt(0) + i - "０".charCodeAt(0)));
            }

            for (let i = "ａ".charCodeAt(0); i <= "ｚ".charCodeAt(0); i++) {
                value = value.replaceAll(String.fromCharCode(i), String.fromCharCode("a".charCodeAt(0) + i - "ａ".charCodeAt(0)));
            }

            for (let i = "Ａ".charCodeAt(0); i <= "Ｚ".charCodeAt(0); i++) {
                value = value.replaceAll(String.fromCharCode(i), String.fromCharCode("A".charCodeAt(0) + i - "Ａ".charCodeAt(0)));
            }
            this.preview = md.render(value);
        }
    }
}
</script>

<style scoped>
main {
    margin: 0;
    padding: 0;
    border: 0;
    width: 100%;
}

div#editor,
div#preview {
    display: inline-block;
    vertical-align: top;
    margin: 0 calc(1% - 1px);
    padding: 0;
    border: 0;
    width: calc(50% - 2% - 2px - 2px);
}

h2 {
    margin: 5px 0 10px;
    padding: 0;
    border: 0;
    height: 30px;
    font-size: 28px;
    font-family: sans-serif;
}

div#editor>textarea,
div#preview>div {
    margin: 0;
    padding: 5px;
    border: 1px solid black;
    width: 100%;
    height: calc(100vh - 32px - 10px - 15px);
    overflow-y: scroll;
}
</style>
