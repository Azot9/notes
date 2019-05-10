<template>
	<div class="note-editor">
		<textarea v-model="text" placeholder="Enter your note here ..." rows="5" />
		<div class="controll-panel">
		<input v-model="color" class="add-color" type="color">
		<button
			class="add-button"
			@click="createNote"
			:disabled="text.trim().length === 0"
		>
			Add
		</button>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'notes-editor',

		data: () => ({
			text: '',
			color: '#415689'
		}),

		methods: {
			createNote() {
				const rnd = () => Math.floor(Math.random() * 255);

				const note = {
					id: new Date().getTime(),
					text: this.text,
					bgcolor: this.color,
					hashtags: []
				};
				// this.$parent.notes.push(note);
				this.$emit('createNote', note);
				this.text = '';
			},
		},
	};
</script>

<style lang="scss" scoped>
	.note-editor {
		width: 100%;
		max-width: 600px;
		padding: 16px;
		margin: 16px auto;
		background-color: white;
		box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
		border-radius: 2px;
		display: flex;
		flex-direction: column;
	}

	textarea {
		width: 100%;
		resize: none;
		margin: 5px;
		font-size: 14px;
		border: none;
		font-weight: 300;
	}

	textarea:focus {
		outline: 0;
	}

	.add-button {
		align-self: flex-end;
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
		margin-left: 15px;
	}
	.add-color{
		display: inline-block;
		align-self: flex-start;
		height: 25px;

	}
	.controll-panel{

	}

	.add-button:hover {
		background-color: #5cbf2a;
	}

	.add-button:active {
		position: relative;
		top: 1px;
	}

	.add-button:focus {
		outline: 0;
	}
</style>
