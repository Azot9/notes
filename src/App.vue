<template>
	<div class="notes-app">
		<notes-header :title="title" />
		<notes-editor  @createNote="createNote" />
		<notes-grid :notes="notes" :hashtag="hashtag"/>
	</div>
</template>

<script>
	import NotesHeader from '@/components/NotesHeader.vue';
	import NotesEditor from '@/components/NotesEditor.vue';
	import NotesGrid from '@/components/NotesGrid.vue';

	const notes = localStorage.notes ? JSON.parse(localStorage.notes) : [];

	export default {
		name: 'notes-app',

		components: {
			NotesHeader,
			NotesEditor,
			NotesGrid,
		},

		data: () => ({
			title: 'Notes App',
			notes,
			hashtag: ""
		}),

		methods: {
			createNote(note) {
				this.notes.push(note);
				localStorage.notes = JSON.stringify(notes);
			},

			deleteNote(id) {
				this.notes = this.notes.filter(note => note.id !== id);
				localStorage.notes = JSON.stringify(this.notes);
			},
			editNote(newNote) {

				this.notes = this.notes.map(note => {
					if (note.id === newNote.id) {
						note.text = newNote.text;
					}
					return note;
				});
				localStorage.notes = JSON.stringify(notes);
			},
			setHashtag(hashtag){			
				this.hashtag = hashtag;
			}
		},
	};
</script>

<style lang="scss">
	$color: #eaeaea;

	* {
		box-sizing: border-box;
	}

	body {
		font-family: sans-serif;
		font-weight: 300;
		background-color: $color;
	}

	.notes-app {
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 100%;
		max-width: 980px;
		margin: 0 auto;
	}
</style>
