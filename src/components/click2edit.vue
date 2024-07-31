<template>
  <div id="container"></div>
</template>

<script setup lang="ts">

import {onMounted, ref} from "vue";


type Opt = {
  val?: string
  placeholder?: string
}

class ClickToEdit {

  // state
  value: string
  placeholder: string = '请输入'
  container: HTMLElement | null
  containerSize: object = { width: 0, height: 0 }
  frame: HTMLElement | null = null
  editor: HTMLInputElement | null = null
  callback: Function

  // constructor
  constructor(id: string, options: Opt, callback:Function = () => {}) {
    this.value = options.val || ''
    this.container = document.getElementById(id)
    this.placeholder = typeof options.placeholder === 'string' ? options.placeholder : this.placeholder
    this.initDom()
    this.callback = callback
  }

  // methods
  initDom(): void {
    if (!this.container) return
    this.getContainerSize()
    this.createShowFrame()
    this.createEditor()
    this.hideEditor()
  }

  getContainerSize():void {
    this.containerSize = this.container.getBoundingClientRect()
  }

  createShowFrame() {
    this.frame = document.createElement('span')
    this.frame.className = 'frame-box'
    this.frame.innerText = this.value || this.placeholder
    this.frame.style.width = this.containerSize.width
    this.frame.addEventListener('click', () => {
      this.showEditor()
    })
    this.container?.appendChild(this.frame)
  }

  createEditor():void {
    this.editor = document.createElement('input')
    this.editor.value = this.value || ''
    this.editor.placeholder = this.placeholder
    this.editor.addEventListener('blur', (e) => {
      this.handleBlur(e)
    })
    this.container?.appendChild(this.editor)
  }

  handleBlur(e:FocusEvent) {
    const content = e?.target?.value
    this.value = content || this.placeholder
    this.setFrameValue()
    this.callback(content)
    this.hideEditor()
  }

  hideEditor() {
    if (!this.editor || !this.frame) return
    this.editor.style.display = 'none'
    this.frame.style.display = 'inline-block'
  }

  showEditor() {
    if (!this.editor || !this.frame) return
    this.editor.style.display = 'inline-block'
    this.frame.style.display = 'none'
    this.editor.focus()
  }

  setFrameValue() {
    this.frame.innerText = this.value
  }

}

const sign = ref('')

const handleChange = (value: string) => {
  sign.value = value
}

onMounted(() => {
  const myEditor = new ClickToEdit('container', {val: '', placeholder: '请输入个性签名'}, handleChange)
  console.log(myEditor)
})


</script>

<style lang='less'>
#container {
  width: 1200px;
  height: 100px;
  background-image: url("/src/assets/bg.webp");
  padding: 50px;

  .frame-box {
    color: #5c5c5c;
    border: 2px solid rgba(255,255,255, 0);
    width: 200px;
    padding: 3px 5px;
    border-radius: 4px;
    font-size: 14px;
    transition: all ease .5s;
    &:hover {
      border-color: #fff;
      background-color: rgba(255,255,255, 0.5);
    }
  }

  input {
    padding: 3px 5px;
    border: 2px solid #fff;
    height: 29px;
    box-sizing: border-box;
    width: 200px;
    border-radius: 4px;
    outline: none;
    font-size: 14px;
    font-family: "Microsoft YaHei";
    transition: all ease .5s;
    &::placeholder {
      font-size: 14px;
      color: #5c5c5c;
    }
  }
 }
</style>
