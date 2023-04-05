<script setup>
import { reactive, ref } from 'vue'

const state = reactive({
	files: [], // { title, url }
	activeMedia: null,
})
const video = ref()

function addFiles({ target }) {
	const { files } = target

	if (files.length === 0) { return }

	for (const file of files) {
		console.log(file)
		state.files.push({
			title: file.name,
			url: URL.createObjectURL(file)
		})
	}
}

function activeMedia(index) {
	state.activeMedia = index
	video.value.src = state.files[index].url
}
</script>

<template>
	<div class="wrapper">
		<div class="player">
			<video controls ref="video"></video>
		</div>
		<div class="content">
			<label class="file_input">
				<h3>Add files</h3>
				<input type="file" @change="addFiles" multiple>
			</label>

			<ul class="media_list">
				<li class="media_list__item" :class="{ active: state.activeMedia === index }" v-for="(file, index) in state.files" :key="index" @click="activeMedia(index)">
					<span class="media_list__item__title">{{ file.title }}</span>
					<!-- <span class="media_list__item__address">C:\test\location</span> -->
				</li>
			</ul>
		</div>
	</div>
</template>

<style scoped>
.wrapper {
	width: 80%;
	height: 100vh;
	min-height: 0;
	display: flex;
	flex-direction: row;
	justify-content: space-around;
	flex: 1;
	margin: 0 auto;
	gap: 10px;
}

.player {
	display: flex;
	padding: 10px;
	flex: 1 0 30%;
	min-height: 0;
	justify-content: center;
	/* align-items: center; */
	overflow: hidden;
}
video {
	width: 100%;
	height: 450px;
	aspect-ratio:16 / 9;
}
.content {
	display: flex;
	min-height: 0;
	flex: 1;
	flex-direction: column;
	gap: 10px;
	padding: 10px;
	overflow: auto;
}

.file_input {
	width: 100%;
	height: auto;
	border: 1px solid rgba(0, 0, 0, .2);
	padding: 5px;
	text-align: center;
	border-radius: 5px;
	background-color: dodgerblue;
	color: #fff;
}
.file_input input { display: none; }

.media_list { list-style: none; }
.media_list__item {
	display: flex;
	flex-direction: column;
	border-bottom: 1px solid rgba(0, 0, 0, .1);
	padding: 5px;
	cursor: pointer;
	transition: background .3s linear;
}
.media_list__item.active {
	background: hsla(120, 100%, 90%, 1);
}
.media_list__item:hover {
	background: hsla(120, 40%, 80%, 1);
}
.media_list__item:last-child { border: none; }
.media_list__item__title { font-weight: bold; }
.media_list__item__address { 
	font-size: 12px; 
	padding-left: 5px;
}

@media (width < 1200px) {
	.wrapper {
		width: 100%;
	}
}
</style>
