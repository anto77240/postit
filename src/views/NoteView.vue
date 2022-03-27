<!-- @format -->

<template>
	<div class="note">
		<Note msg="Your note" />
	</div>
</template>

<script>
// @ is an alias to /src
import Note from "@/components/OneNote.vue";

export default {
	name: "NoteView",
	components: {
		Note,
	},
};
</script>
