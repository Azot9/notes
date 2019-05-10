<template>

	<div class="notes-grid" ref="grid">
		<button 
		v-if="hashtag" 
		class="add-button"
		@click="returnToAll"
		>return</button>
		<note-item v-for="note in notesWithTags" :key="note.id" :note="note" />	
	</div>

</template>

<script>
	import Masonry from 'masonry-layout';
	import NoteItem from '@/components/NoteItem.vue';
	import { filter } from 'minimatch';

	export default {
		name: 'notes-grid',
		props: {
			notes: {
				type: Array,
				required: true,
			},
			hashtag:{
				type: String,
				default: ""
			}
		},

		components: {
			NoteItem,
		},

		methods:{
			log(){
				console.log(this.hashtag);
			},
			returnToAll(){
				this.$parent.hashtag = "";
			}
		},

		computed: {
			notesWithTags(){
				return this.notes.filter((note) => {
					if(this.hashtag) {
						return note.hashtags.indexOf(this.hashtag) !== -1;
					} else {
						return true;
					}
				});
			}
		},

		mounted() {
			
			const grid = this.$refs.grid;
			this.msnry = new Masonry(grid, {
				itemSelector: '.note',
				columnWidth: 200,
				gutter: 10,
				isFitWidth: true,
			});
		},

		updated() {
			this.msnry.reloadItems();
			this.msnry.layout();
		},
	};
</script>

<style lang="scss" scoped>
	.hashtag{

		height: 50px;
		background-color: red;
	}
	.notes-grid {
		margin: 0 auto;
	}
		.add-button {
		position: relative;
		left: -120px;
		width: 100px;
		background-color: #44c767;
		border-radius: 8px;
		border: 1px solid #18ab29;
		cursor: pointer;
		color: #ffffff;
		font-size: 14px;
		padding: 8px 8px;
		text-transform: uppercase;
		text-decoration: none;
		text-shadow: 0px 1px 0px #2f6627;
	}
</style>
