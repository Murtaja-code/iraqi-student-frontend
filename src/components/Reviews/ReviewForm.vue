<template>
	<b-form @submit.prevent="formSumbit">
		<label for="textarea">أترك مراجعة: </label>
		<b-form-textarea
			class=" mb-1"
			id="textarea"
			v-model="text"
			placeholder="اكتب ..."
			rows="2"
		></b-form-textarea>
		<b-popover target="textarea" placement="topleft" variant="info" v-if="!refresh">
			يرجىء
			<b-button variant="outline-primary" size="sm" to="/login">تسجل الدخول</b-button>
			أولا
		</b-popover>

		<b-button
			v-if="!loading"
			variant="primary"
			:disabled="!refresh || text === ''"
			class="mb-2"
			type="submit"
			>ارسال</b-button
		>
		<b-button v-else variant="primary" class="mb-2" disabled>
			ارسال...
			<b-spinner small></b-spinner>
		</b-button>
	</b-form>
</template>

<script>
import helper from "../../helper"
import { mapState } from "vuex"

export default {
	data() {
		return {
			loading: false,
			text: ""
		}
	},
	props: {
		fetchReview: Function,
		building: Number,
		sub_url: String
	},
	computed: mapState({
		refresh: (state) => state.tokenModel.refresh
	}),
	methods: {
		formSumbit() {
			this.loading = true
			helper
				.sendReviewRating({
					review: this.text,
					building: this.building,
					sub_url: this.sub_url,
					method: "POST"
				})
				.then(() => {
					this.fetchReview(1)
					this.loading = false
				})
			this.text = ""
		}
	}
}
</script>

<style></style>
