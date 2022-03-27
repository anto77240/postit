<!-- @format -->

<template>
	<div class="home">
		<PostIt msg="Post-it Board" />
	</div>
</template>

<script>
// @ is an alias to /src
import PostIt from "@/components/PostIt.vue";

export default {
	name: "PostItView",
	components: {
		PostIt,
	},
	data() {
		return {
			msg: String,
		};
	},
};
</script>
