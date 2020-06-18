<template>
    <div>
        <div ref="styleEditor" class="leftContent">
            <div class="code" v-html="codeInStyleTag"></div>
            <pre class="" v-html="style"></pre>
        </div>
        <div class="rightContent">
            <div v-if="isMarkdown" v-html="markedResume" :class="markdownClass"></div>
            <pre v-else>{{resume}}</pre>
        </div>
    </div>
</template>

<script>
    import '../style/github-markdown.css'
    import marked from 'marked'

    export default {
        name: 'app',
        data() {
            return {
                // interval: 40,
                styleContent: [
                    `/* Hello, My name is Yichao Tang, you can call me Isen 
    * This is how I write my resume
    * It is base on react. I use create-react-app to build the scaffold.
    * I hope you like it~
    * ===================================================
    */`, `/* This interface is not like development interface.
    /* So , let me beautify the interface ^_^
    */`,
                    `
    {
    -webkit-transition: all .3s;
    transition: all .3s;
    }
    html {
    color: #5CACEE; background: #030303; 
    }
    .leftContent {
    padding: 2px;
    border: 1px solid;
    margin: 5px;
    overflow: auto;
    width: calc((100vw - 256px - 10px)/2); 
    height: 90vh;
    }
    /* It looks like a  command-line tool now , right ?
    /* Let me finish my resume~
    * ====================================================
    * First of all , I need a paper.
    */
    .rightContent {
    position: fixed; right: 0; top: 64px;
    padding: .5em;  
    margin: 5px;
    width: calc((100vw - 256px - 10px)/2); 
    height: 90vh; 
    border: 1px solid;
    background: white; color: #222;
    overflow: auto;
    }
    /* Now, I got a white paper. 
    * Let me start~~
    */`, `/*As you see, it wirte by markdown grammar.
    /*Let me translated into HTML by using marked package.*/
    /*Let us start~*/
    /*....1....*/
    /*....2....*/
    /*....3....*/
    /*.action!.*/`,
                    `
    /*Emmmmmmmmmmmmmmmm......*/
    /*It is not pretty right?*/
    /*So I use css to beautify my resume. I used github-markdown-css.*/
    /*Let us start~*/
    /*....1....*/
    /*....2....*/
    /*....3....*/
    /*...go!...*/`,
                    '/*And here we are.*/?'
                ],

                resumeContent: [`
    ## MyResume

    ### Name : *Yichao Tang*
    ### Gender : *male*
    ### Email : *3104255643@qq.com*
    ### Education : Nanjing University of Posts and Telecommunication`],
                isMarkdown: false,
                markdownClass: '',
                style: '',
                resume: '',
                timer: null, // 定时器
            }
        },
        computed: {
            codeInStyleTag () {
                return `<style>${this.style}</style>`
            },
            markedResume () {
                return marked(this.resume);
            }
        },
        created() {
            this.makeResume()
        },
        methods: {
            makeResume() {
                const { showContent, interval } = this;
                this.timer = setInterval(() => showContent(), interval);
                // this.showContent();
            },
            getIndex() {
                const length = this.style.length;

                /* 0代表左边，1代表右边 */
                let part = 0;
                let index = 0;

                if (length < this.styleContent.slice(0, 1).join('').length) {
                    index = 0;
                } else if (length >= this.styleContent.slice(0, 1).join('').length && length < this.styleContent.slice(0, 2).join('').length) {
                    index = 1;
                } else if (length >= this.styleContent.slice(0, 2).join('').length && length < this.styleContent.slice(0, 3).join('').length) {
                    index = 2;
                } else if (length >= this.styleContent.slice(0, 3).join('').length && length < this.styleContent.slice(0, 4).join('').length) {
                    part = 1;
                    index = 0;
                }

                if (this.resume.length >= this.resumeContent[0].length) {
                    part = 0;
                    index = 3;
                }

                if (length >= this.styleContent.slice(0, 4).join('').length) {
                    if (!this.isMarkdown) {
                        this.getIsMarked();
                    }
                    index = 4;
                }

                if (length >= this.styleContent.slice(0, 5).join('').length) {

                    if (this.markdownClass === '') {
                        this.getMarkedStyle();
                    }
                    index = 5;
                }

                return { part, index };

            },
            getIsMarked() {
                this.isMarkdown = true
            },
            getMarkedStyle() {
                this.markdownClass = 'markdown-body'
            },
            showContent() {
                /* 获取信息数组下标 */
                let { index, part } = this.getIndex();
                console.log(index, part)

                /* 左边为样式部分，右边为简历部分 */
                let content = part === 0 ? this.styleContent : this.resumeContent;

                /* 左边 */

                /* 总长度 */
                let totalLength = part === 0 ? this.styleContent.slice(0, index + 1).join('').length :
                    this.resumeContent.slice(0, index + 1).join('').length;

                /* 现在所在的位置 */
                let currentIndex = part === 0 ? this.style.length - this.styleContent.slice(0, index).join('').length :
                    this.resume.length - this.resumeContent.slice(0, index).join('').length;

                /* 如果相同，返回，去下一个内容 */
                if (currentIndex === totalLength) {

                    return;
                }

                /* 塞入对应字符 */
                if (part === 0) {
                    this.style = this.style + content[index].substring(currentIndex, currentIndex + 1)
                } else {
                    this.resume = this.resume + content[index].substring(currentIndex, currentIndex + 1)
                }

                /* 自动滚动到底部 */
                if (this.$refs.styleEditor){
                    this.$refs.styleEditor.scrollTop = 100000;
                    // this.$refs.styleEditor.goBottom();
                }

                let styleAllLength = this.styleContent.slice(0, 6).join('').length
                if(this.style.length > styleAllLength){
                    window.clearInterval(this.timer)
                }
            }

        }
    }
</script>

<style scoped>
</style>