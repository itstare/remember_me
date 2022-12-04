<template>
	<base-dialog v-if="inputIsInvalid" title="Input is invalid" v-on:close="confirmError">
		<template v-slot:default>
			<p>Unfortunately, at least one input value is invalid.</p>
			<p>Please check all input fields and make sure correct data is entered.</p>
		</template>
		<template v-slot:actions>
			<base-button v-on:click="confirmError">Ok</base-button>
		</template>
	</base-dialog>
	<base-card>
		<form v-on:submit.prevent="submitData">
			<div class="form-control">
				<label for="title">Title</label>
				<input type="text" name="title" id="title" ref="titleInput">	
			</div>
			<div class="form-control">
				<label for="description">Description</label>
				<textarea name="description" id="description" rows="3" ref="descInput"></textarea>	
			</div>
			<div class="form-control">
				<label for="link">Link</label>
				<input type="url" name="link" id="link" ref="urlInput">	
			</div>
			<div>
				<base-button type="submit">Add Resource</base-button>
			</div>
		</form>
	</base-card>
</template>

<script>
	export default {
		inject: ['addResource'],

		data() {
			return {
				inputIsInvalid: false
			}
		},

		methods: {
			submitData() {
				const enteredTitle = this.$refs.titleInput.value;
				const enteredDesc = this.$refs.descInput.value;
				const enteredUrl = this.$refs.urlInput.value;

				if(enteredTitle.trim() === '' || enteredDesc.trim() === '' || enteredUrl.trim() === '') {
					this.inputIsInvalid = true;
					return;
				}

				this.addResource(enteredTitle, enteredDesc, enteredUrl);
			},
			confirmError() {
				this.inputIsInvalid = false;
			}
		}
	}
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>