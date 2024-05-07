<script>
	let files_array = [];
	let file_input;
	let images = [];
	// let on_load_complete;
	function send_click_to_input() {
		file_input.click();
	}
	function remove_from_files(index) {
		files_array.splice(index, 1);
		images.splice(index, 1);
		files_array = files_array;
		images = images;
	}
	function remove_all() {
		files_array = [];
		images = [];
	}
	async function add_to_files_array() {
		console.log("on change FilesList", file_input.files);
		files_array = [...files_array, ...file_input.files];
		console.log("on change files array", files_array);
		//Loads with readAsDataURL
		Promise.all(files_array.map(read_images)).then((values) => {
			images = values;
		});
		// on_load_complete.innerText = "";
	}
	//Loads with readAsDataURL
	function read_images(file) {
		return new Promise((resolve, reject) => {
			let fr = new FileReader();
			fr.onload = () => {
				resolve(fr.result);
			};
			fr.onerror = () => {
				reject(fr);
			};
			fr.readAsDataURL(file);
		});
	}
	function send_files() {
		var form_data = new FormData();
		files_array.map((value) => {
			form_data.append("uploadfile[]", value);
		});
		// Send file to server
		console.log("form_data", form_data);
	}
</script>

<input
	type="file"
	accept="image/*"
	style="display: none"
	bind:this={file_input}
	multiple
	on:change={add_to_files_array}
/>

<div class="attachment-container">
	<button on:click={send_click_to_input} class="attachment">
		<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
			<path fill-rule="evenodd" d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25ZM12.75 9a.75.75 0 0 0-1.5 0v2.25H9a.75.75 0 0 0 0 1.5h2.25V15a.75.75 0 0 0 1.5 0v-2.25H15a.75.75 0 0 0 0-1.5h-2.25V9Z" clip-rule="evenodd"/>
		</svg>
	</button>
	{#if files_array && files_array[0]}
		{#each files_array as file, i}
			<div class="image_block">
				<img src={images[i]} alt="" />
				<button
					class="remove_cross"
					on:click={() => remove_from_files(i)}
				>
				<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
					<path fill-rule="evenodd" d="M16.5 4.478v.227a48.816 48.816 0 0 1 3.878.512.75.75 0 1 1-.256 1.478l-.209-.035-1.005 13.07a3 3 0 0 1-2.991 2.77H8.084a3 3 0 0 1-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 0 1-.256-1.478A48.567 48.567 0 0 1 7.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 0 1 3.369 0c1.603.051 2.815 1.387 2.815 2.951Zm-6.136-1.452a51.196 51.196 0 0 1 3.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 0 0-6 0v-.113c0-.794.609-1.428 1.364-1.452Zm-.355 5.945a.75.75 0 1 0-1.5.058l.347 9a.75.75 0 1 0 1.499-.058l-.346-9Zm5.48.058a.75.75 0 1 0-1.498-.058l-.347 9a.75.75 0 0 0 1.5.058l.345-9Z" clip-rule="evenodd" />
				</svg>
				</button>
			</div>
		{/each}
	{/if}
</div>
{#if files_array && files_array[0]}
	<button on:click={send_files} class="btn-submit-files">Submit files</button>
{/if}
<style>
	.attachment-container{
		display: flex;
		flex-wrap: wrap;
		flex-direction: row;
		align-content: center;
		justify-content: flex-start;
		align-items: flex-start;
		gap: 4px;
	}
	.image_block {
		display: flex;
		flex-wrap: wrap;
		flex-direction: row;
		align-content: center;
		justify-content: flex-start;
		align-items: flex-start;
		position: relative;
	}
	.image_block img{
		width: 100px;
		height: 100px;
		object-fit: cover;
		border-radius: 8px;
		border: 1px dashed #ccc;
	}
	.attachment{
		width: 100px;
		height: 100px;
		border-radius: 8px;
		padding: 5px;
		border: 1px dashed #ccc;
		font-weight: 500;
		text-align: center;
		text-decoration: none;
		display: inline-block;
	}
	.attachment:hover {
		background-color: #DAEAF1;
		transition: background-color 150ms linear;
	}
	.attachment svg{
		width: 20px;
		height: 20px;
		margin-left: 5px;
		margin: auto;
		cursor: pointer;
	}
	.attachment svg:hover{
		color: #007bff;
	}
	.remove_cross {
		position: absolute;
		top: 5px;
		right: 5px;
		margin: unset;
		padding: unset;
		border: none;
		width: 20px;
		height: 20px;
		border-radius: 5px;
		background-color: #151515;
	}
	.remove_cross svg{
		width: 15px;
		height: 15px;
		color: white;
	}
	.remove_cross svg:hover{
		color: red;
	}
	.btn-submit-files{
		padding: 5px 10px;
		font-size: 16px;
		color: white;
		background-color: #007bff;
		border-radius: 8px;
	}
</style>
