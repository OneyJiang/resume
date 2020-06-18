<template>
    <div style="padding: 33px">
        <div class="leftContent" ref="styleBox">
            <div style="display:none" v-html="leftCodeStyleTag"></div>
            <pre v-html="leftCode"></pre>
        </div>

        <div class="rightContent">
            <pre v-html="rightContent" :class="className"></pre>
        </div>
    </div>
</template>

<script>
import marked from 'marked'
import '../style/github-markdown.css';
    export default {
        name: 'Home',
        data() {
            return {
                leftCode: '', /* 左面style内容 */
                rightContent: '', /* 右面简历内容 */
                interval: 8, /* 间隔时间 */
                leftCount: 0, /* 左侧字数 */
                rightCount: 0, /* 右侧字数 */
                className: '', /* 简历Class */
                /* 简历内容 */
                resumeContent: [
`
## MyResume

### name : *Ban Yuan*
### slogen : *一次相遇终身相伴*
### Email : *contact@banyuan.club*
### website : www.banyuan.club`
                ],

                /* 高亮内容 */
                styleContent: [
`
/* Hello, My name is Ban Yuan,
* This is how I write my resume.
* I hope you like it~
* ===================================================
*/`
,
`/* This interface is not like development interface.
/* So , let me beautify the interface ^_^
*/`
,
`
{
    -webkit-transition: all .3s;
    transition: all .3s;
}
html {
    color: #06EB00; background: #030303;
}
.leftContent {
    padding: .5em;
    border: 1px solid;
    margin: .5em;
    overflow: auto;
    width: 45vw; height: 90vh;
}
/* It looks like a  command-line tool now , right ?
/* Let me finish my resume~
* ====================================================
* First of all , I need a paper.
*/
.rightContent {
    position: fixed; right: 0; top: 33px;
    padding: .5em;  margin: .5em;
    width: 48vw; height: 90vh; 
    border: 1px solid;
    background: white; color: #222;
    overflow: auto;
}
/* Now, I got a white paper. 
* Let me start~~
*/`
,
`/*As you see, it wirte by markdown grammar.
/*Let me translated into HTML by using marked package.*/
/*Let us start~*/
/*....1....*/
/*....2....*/
/*....3....*/
/*.action!.*/`
,
`
/*Emmmmmmmmmmmmmmmm......*/
/*It is not pretty right?*/
/*So I use css to beautify my resume. I use github-markdown-css.*/
/*Let us start~*/
/*....1....*/
/*....2....*/
/*....3....*/
/*...go!...*/`
,
`/*And here we are.*/?
`
                ],
                intervalController: null, /* 定时器全局变量 */
            }
        },
        computed: {
            /* style构建字符串 */
            styleStr () {
                return this.styleContent.join('')
            },
            // 已高亮部分的style内容 塞到<style></style>标签里 --> 渲染到页面上
            leftCodeStyleTag () {
                return `<style>${this.leftCode}</style>`
            }
        },
        created() {
            this.start();
        },
        methods: {
            start () {
                /* 定时调用，增加字数 */
                this.intervalController = setInterval(this.showContent,this.interval)
            },

            /* 向左侧style添加内容 */
            addContentToLeft () {
                /* 左边增加内容 */
                this.leftCode =  this.styleStr.substring(0,this.leftCount);
                // this.leftCode = leftAdd;
            },
            showContent () {

                /* 显示 简历rightContent 之前 */
                if(this.leftCount < this.styleContent.slice(0,3).join('').length){
        
                    this.leftCount++
        
                    this.addContentToLeft()
        
                }else{
        
                    /* 开始显示简历 */
                    this.rightCount++

                    this.rightContent = this.resumeContent[0].substring(0,this.rightCount);

                    /* 简历显示完成，继续显示左边内容 */
                    if(this.rightCount > this.resumeContent[0].length){
        
                        this.leftCount++
                        /* 继续向左边增加内容 */
                        this.addContentToLeft()
            
                        /* 左面 第四部分style填充后 --> 解析markdown语法 */
                        if(this.leftCount >= this.styleContent.slice(0,4).join('').length){
                            /* marked */
                            this.rightContent = marked(this.resumeContent[0].substring(0,this.rightCount));
                        }    
                    }
        
                    /* 给right简历部分 添加class，增加样式 */
                    if(this.leftCount > this.styleContent.slice(0,5).join('').length){
                        this.className = 'markdown-body'
                    }
                }

                this.$refs.styleBox.scrollTop = 100000;
        
                /* 结束，清除定时器 */
                if(this.leftCount> this.styleStr.length){
        
                    window.clearInterval(this.intervalController)
                }
            }
        },
    }
</script>

<style scope>
</style>