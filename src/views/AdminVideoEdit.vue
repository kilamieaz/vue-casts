<template>
	<v-container v-if="video">
		<VideoForm :video="video" :saveVideo="saveVideo" buttonText="Save Video"></VideoForm>
	</v-container>
</template>

<script>
import VideoForm from "@/components/VideoForm";
import { mapState } from "vuex";
export default {
	components: {
		VideoForm
	},
	computed: {
		...mapState({ videos: state => state.videos.videos }),
		video() {
			return this.videos.find(v => v.id == this.$route.params.id);
		}
	},
	methods: {
		async saveVideo() {
			let video = await this.$store.dispatch("videos/edit", this.video);
			this.$store.dispatch("snackbars/setSnackbar", {
				text: `You have successfully edited your video, ${video.name}`
			});
			this.$router.push({ name: "admin-video-list" });
		}
	}
};
</script>