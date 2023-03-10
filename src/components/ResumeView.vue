<template>
	<!-- <div class="card card-w70" v-if="blocks.length" v-html="stringToHTML()"></div> -->
	<div class="card card-w70" v-if="blocks.length">
		<component
			v-for="block in blocks"
			:key="block.id"
			:is="'resume-' + block.type"
			:data="block"
			v-bind="{ value: block.value }"
			>{{ blocks }}</component
		>
		<button class="btn primary" @click="$emit('resumeSave')">
			Сохранить резюме
		</button>
	</div>
	<div class="card card-w70" v-else>
		<h3>Резюме еще не создано</h3>
	</div>
</template>
<script>
import ResumeTitle from './parts/ResumeTitle.vue'
import ResumeSubtitle from './parts/ResumeSubtitle.vue'
import ResumeText from './parts/ResumeText.vue'
import ResumeAvatar from './parts/ResumeAvatar.vue'

export default {
	props: {
		blocks: {
			type: Object,
			required: true,
		},
	},
	emits: ['resumeSave'],
	components: {
		ResumeTitle,
		ResumeSubtitle,
		ResumeText,
		ResumeAvatar,
	},
	methods: {
		stringToHTML() {
			let tempTemplate = ``
			this.blocks.map(item => {
				console.log(item)
				switch (item.type) {
					case 'title':
						tempTemplate += `<h1>Резюме ${item.value}</h1>`
						break
					case 'subtitle':
						tempTemplate += `<h2>${item.value}</h2>`
						break
					case 'avatar':
						tempTemplate += `
						<div class="avatar">
							<img src="${item.value}"/>
						</div>
					`
						break
					case 'text':
						tempTemplate += `<p>${item.value}</p>`
						break
				}
			})
			return tempTemplate
		},
	},
}
</script>
