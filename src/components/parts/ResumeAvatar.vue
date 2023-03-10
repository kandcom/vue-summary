<template>
	<div class="avatar">
		<img :src="data.value" />
	</div>
</template>
<script>
export default {
	props: ['data'],
}
</script>
<style>
.avatar {
	display: flex;
	justify-content: center;
}

.avatar img {
	width: 150px;
	height: auto;
	border-radius: 50%;
}
</style>
