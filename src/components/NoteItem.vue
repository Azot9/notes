<template>
	<div class="note" :style="{ backgroundColor: note.bgcolor }">
		<span class="delete-note" @click="$parent.$parent.deleteNote(note.id)">
			x
		</span>
		<span class="edit-note" @click="startEdit">
			{{isContenteditable ? "save" : "edit"}}
		</span>
		<div class="changed" v-html="notesWithTags"   :contenteditable="isContenteditable">
		
		</div>
	
	</div>
</template>

<script>
import { log } from 'util';
	export default {
		name: 'note-item',
		props: {
			note: {
				type: Object,
				required: true,
			},
		},
		data() {
			return {
				isContenteditable: false
			}
		},
		methods: {
			startEdit($event) {
				console.log(this.isContenteditable);

				this.isContenteditable = !this.isContenteditable;
				if (!this.isContenteditable) {
					let newNote = {
						id: this.note.id,
						text: $event.target.nextElementSibling.innerHTML
					}

				this.$parent.$parent.editNote(newNote);
				}
			}
		},
		computed: {
			notesWithTags (){
				let reqExp = /#\w+/g;
				let text = this.note.text;

				if(!this.isContenteditable){
					return text.replace(reqExp, (str) => {
						this.note.hashtags.push(str);
						return `<span class="hashtag">${str}</span>`;
					})
				} 
				return this.note.text;
				
			}
		},
		mounted(){
			let hashtag = document.getElementsByClassName("hashtag");
			for (const tag of hashtag) {
				tag.onclick = ()=> {
					this.$parent.$parent.setHashtag(tag.innerHTML);
				}
			}	
		}
	};
</script>

<style lang="scss" scoped>
	.note {
		width: 200px;
		height: auto;
		float: left;
		box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
		border-radius: 2px;
		padding: 10px;
		padding-top: 15px;
		margin-bottom: 10px;
		transition: box-shadow 0.3s;
		word-wrap: break-word;
		position: relative;
	}
	.changed{
		outline: none;
	}

	.note:hover {
		box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
	}

	.delete-note {
		position: absolute;
		top: 2px;
		right: 5px;
		display: none;
		color: rgba(0, 0, 0, 0.6);
		cursor: pointer;
	}

	.edit-note{
		position: absolute;
		top: 2px;
		right: 20px;
		display: none;
		color: rgba(0, 0, 0, 0.6);
		cursor: pointer;
	}

	.note:hover .edit-note {
		display: block;
	}

	.note:hover .delete-note {
		display: block;
	}


</style>
<style lang="scss">
	.hashtag {
		color: #4286f4;
		cursor: pointer;
	}

	.hashtag:hover {
		 text-decoration: underline; 
	}
</style>

