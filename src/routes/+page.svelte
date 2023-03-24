<script lang="ts">

  let downloadLink: string;
  let isLoading = false;
  let errorMsg: string;

  async function handleConvert(event: Event) {
    event.preventDefault();
    const form = event.target as HTMLFormElement;
    const input = form.elements.namedItem('googleDocLink') as HTMLInputElement;
    const link = input.value.trim();

    isLoading = true;
    errorMsg = '';

    try {
      const id = link.split("/")[5];
      const newUrl = `https://drive.google.com/uc?export=download&id=${id}`;
      downloadLink = newUrl;
    } catch (error) {
      errorMsg = 'Error converting file';
      console.error(error);
    }

    isLoading = false;
  }
</script>

<main>
  <h1>Google Doc Shareable Link to Download Converter</h1>

  <form on:submit={handleConvert}>
    <label for="googleDocLink">Enter Google Doc link:</label>
    <input type="text" id="googleDocLink" name="googleDocLink" required />
    <button type="submit" disabled={isLoading}>Convert</button>
  </form>

  {#if isLoading}
    <p>Converting...</p>
  {:else if errorMsg}
    <p>{errorMsg}</p>
  {:else if downloadLink}
    <div>
      <p>{downloadLink}</p>
    </div>
  {/if}
</main>
