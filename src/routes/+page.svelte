<script lang="ts">
	import LinkOutput from '../lib/components/LinkOutput.svelte';

	let downloadLink: string
	let isLoading = false
	let errorMsg: string

	async function handleConvert(event: Event) {
		event.preventDefault();
		const form = event.target as HTMLFormElement;
		const input = form.elements.namedItem('googleDocLink') as HTMLInputElement;
		const link = input.value.trim();

		isLoading = true;
		errorMsg = '';

		try {
			const id = link.split('/')[5];
			const newUrl = `https://drive.google.com/uc?export=download&id=${id}`;
			downloadLink = newUrl;
		} catch (error) {
			errorMsg = 'Error converting file';
			console.error(error);
		}

		isLoading = false;
	}
</script>

<main class="min-h-screen min-w-screen bg-base-200">
	<section class="flex place-content-center w-full p-20">
		<div class="text-center">
			<div class="max-w-xl">
				<h1 class="text-5xl font-bold">Google Drive Shareable Link To Download Link</h1>
				<p class="py-10">
					This is a tool to convert a Google Doc shareable link into a direct download link.
				</p>
				<p>
					To use this tool, enter the Google Doc link in the provided input field and click the
					"Convert" button. The tool will convert the link and display the direct download link
					below. If an error occurs during the conversion process, an error message will be
					displayed.
				</p>
			</div>
		</div>
	</section>
	<!-- Form Section-->

	<section>
		<!-- Form Wrapper -->
		<div class="max-w-lg mx-auto rounded-md shadow-md p-10 bg-white">
			<form on:submit={handleConvert}>
				<div class="flex flex-1 flex-col items-center">
					<label class="input-group py-10" for="googleDocLink">
						<span>Shareable Link</span>
						<input
							type="text"
							name="googleDocLink"
							required
							id="googleDocLink"
							placeholder="https://drive.google.com/file/d/1mt__rg..."
							class="input input-bordered w-full"
						/>
					</label>
					<button
						type="submit"
						class="btn btn-primary w-1/2 disabled:opacity-50"
						disabled={isLoading}
					>
						<span class="mr-2">Convert</span>
						{#if isLoading}
							<svg
								class="animate-spin h-5 w-5 text-white"
								xmlns="http://www.w3.org/2000/svg"
								fill="none"
								viewBox="0 0 24 24"
							>
								<circle
									class="opacity-25"
									cx="12"
									cy="12"
									r="10"
									stroke="currentColor"
									stroke-width="4"
								/>
								<path
									class="opacity-75"
									fill="currentColor"
									d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 016 12H2c0 2.18.89 4.16 2.329 5.623l-.002-.002L6 17.291zm5.633 2.408A8 8 0 0112 20c3.526 0 6.664-2.278 7.765-5.704l-1.845-.744A6 6 0 0012 14.82V10h-1v4.82a6 6 0 00-6.92 5.732l-1.847.748zM17 12a5 5 0 11-10 0 5 5 0 0110 0z"
								/>
							</svg>
						{/if}
					</button>
				</div>
			</form>
		</div>
	</section>
	<section class="bg-base-200 min-h-fit p-10">
		{#if errorMsg}
			<p class="text-error mt-2">{errorMsg}</p>
		{:else if downloadLink}
			<LinkOutput downloadLink={downloadLink}/>
		{/if}
	</section>
</main>
