<template>
	<div class="container row">
		<AppAlert :alert="alert" @close="alert = null" />
	</div>
	<div class="container column">
		<ResumeForm @block-added="addBlock" />
		<ResumeView :blocks="summary" v-on:resumeSave="save" />
	</div>
	<div class="container">
		<ResumeComments
			v-if="isLoaded"
			:comments="comments"
			@load-comments="loadComments"
		/>
		<AppLoader v-else />
	</div>
</template>

<script>
import ResumeForm from './components/ResumeForm.vue'
import ResumeView from './components/ResumeView.vue'
import ResumeComments from './components/ResumeComments.vue'
import AppLoader from './components/AppLoader.vue'
import AppAlert from './components/AppAlert.vue'
export default {
	components: {
		ResumeForm,
		ResumeView,
		ResumeComments,
		AppLoader,
		AppAlert,
	},
	data() {
		return {
			summary: [],
			isLoaded: true,
			comments: [],
			URL: 'https://vue-summary-164b1-default-rtdb.asia-southeast1.firebasedatabase.app/summary.json',
			alert: null,
		}
	},
	mounted() {
		this.getFromServer()
	},
	methods: {
		addBlock(data) {
			console.log(data)
			this.summary.push(data)
		},
		async getFromServer() {
			try {
				const response = await fetch(this.URL, {
					method: 'GET',
					headers: {
						'Content-Type': 'application.json',
					},
				})
				const data = await response.json()
				if (!data) {
					throw new Error('В firebase не сохранено ничего')
				}
				Object.keys(data).map(idx => {
					this.summary.push(...data[idx])
				})
			} catch (e) {
				this.alert = {
					type: 'danger',
					title: 'Резюме не создано',
					text: e.message,
				}
			}
		},
		save() {
			try {
				fetch(this.URL, {
					method: 'POST',
					headers: {
						'Content-Type': 'application.json',
					},
					body: JSON.stringify(this.summary),
				})
					.then(data => data.json)
					.then(data => console.log(data))
				throw new Error()
			} catch (e) {
				this.alert = {
					type: 'primary',
					title: 'Данные сохранены успешно в firebase',
					text: e.message,
				}
			}
		},
		loadComments() {
			const URL = 'https://jsonplaceholder.typicode.com/comments?_limit=42'
			this.isLoaded = false
			fetch(URL, {
				method: 'GET',
				headers: {
					'Content-Type': 'application/json',
				},
			})
				.then(data => {
					console.log(data)
					return data.json()
				})
				.then(data => {
					this.comments = data
					this.isLoaded = true
				})
		},
	},
}
</script>
