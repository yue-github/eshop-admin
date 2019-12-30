<template>
	<div id="editor"></div>
</template>

<script>
	var editor;
	var E = require('wangeditor/release/wangeditor') // 使用 npm 安装
	//var E = require('/wangEditor.min.js') // 使用下载的源码
	export default {
		name: 'editor',
		data() {
			return {
				editorContent: ''
			}
		},
		methods: {
			getContent: function() {
				return editor.txt.html()
			},
			setContent: function(content) {
				if(content != ''){
					editor.txt.html(content)
				}else{
					editor.txt.html('')
				}
			}
		},
		mounted() {
			// 创建编辑器
			editor = new E('#editor')
			// 开启图片上传
			editor.customConfig.showLinkImg = true
			// 图片上传路径
			editor.customConfig.uploadImgServer = this.baseUrl + 'admin/file/uploadFile'
			// 自定义fileName
			editor.customConfig.uploadFileName = 'file'
			// 菜单配置
			/*editor.customConfig.menus = [
		      'head', 
		      'bold', 
		      'fontSize', 
		      'fontName', 
		      'italic', 
		      'underline', 
		      'strikeThrough', 
		      'foreColor', 
		      'backColor', 
		      'link', 
		      'list', 
		      'justify', 
		      'quote', 
		      'emoticon', 
		      'image', 
		      'table', 
		      'video', 
		      'code', 
		      'undo', 
		      'redo',
		      'fullscreen'
		    ];*/

			editor.create()
		}
	}
</script>

<style>

</style>