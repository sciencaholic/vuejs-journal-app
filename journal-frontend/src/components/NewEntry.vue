<template>
	<div class="child-box-foot">
		<form class="entry" autocomplete="off" @submit="onSubmit">
			<highlight-btn 
				:isHighlighted="newEntry.highlight"
				@toggle-highlight="toggleHighlight"
			/>
			<p class="entry-c2">
				<!-- <div class="input-field entry-c2 ml-1 mr-8"> -->
					<textarea 
						type="text" 
						name="text" 
						class="materialize-textarea"
						v-model="newEntry.text" 
						v-on:keyup.enter="onSubmit"
						placeholder="Tap to make an entry"
						maxlength="120"
						wrap="soft"
					></textarea>
				<!-- </div> -->
			</p>
			<p class="entry-c3"></p>
			<div class="entry-c3">
				<button 
					type="submit" 
					class="entry-add-btn waves-effect waves-black btn-floating p-1"
				>
					<i class="material-icons">add</i>
				</button>
			</div>
			
		</form>
	</div>
</template>

<script>
import moment from 'moment'
import HighlightBtn from './HighlightBtn.vue'
import shared from "../shared";

export default {
  components: { HighlightBtn },
	name: 'NewEntry',
	props: {},
	data() {
		return {
			newEntry: {
				highlight: false,
				date: '',
				text: ''
			},
			isMobileView: false
		}
	},
	created() {
		this.isMobileView = shared.handleView();
	},
	methods: {
		toggleHighlight() {
			this.newEntry.highlight = !this.newEntry.highlight;
		},
		onSubmit(e) {
			e.preventDefault();
			this.newEntry.text = this.newEntry.text.trim(/\n/g, '');
			if (!this.newEntry.text) { 
				shared.toast(shared.errorTexts.EMPTY_ENTRY_TEXT)
				return;
			}

			// this.newEntry.id = Math.floor(Math.random()*100000); // TODO: be
			this.newEntry.date = moment().format();
			this.newEntry.tags = shared.findHashtags(this.newEntry.text) || [];
			console.log("newEntry: ", this.newEntry);
			this.$emit('add-entry', this.newEntry);
			// resetting the form
			this.newEntry = {
				highlight: false,
				date: '',
				text: ''
			}
		}
	}
}
</script>

<style scoped>
/* .entry-add-btn {} */
.materialize-textarea {
	color: white;
}
/* textarea {
	white-space: pre;
  overflow-wrap: normal;
} */
</style>
