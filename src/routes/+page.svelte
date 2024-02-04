<script lang="ts">
  import axios from "axios";
  import { onMount } from "svelte";
  import { fade, fly } from "svelte/transition";

  interface Photo {
    id: string;
    alt_description: string;
    urls: {
      regular: string;
    };
  }

  let searchterm = "";
  let photos: Photo[] = [];

  onMount(() => {
    console.log("hello");
    fetchImages();
  });

  const fetchImages = async () => {
    const response = await axios.get(
      `https://api.unsplash.com/search/photos?query=${
        searchterm || "random"
      }&client_id=YVO3cTRdF0nEUWXy0ALUH2bx2JVeVKkFIE35yzJ87yY`
    );
    photos = response.data.results;
  };

  const handleSubmit = async () => {
    if (!searchterm) {
      return;
    }
    await fetchImages();
    searchterm = "";
  };
</script>

<main>
  <div class="container">
    <div class="header">
      <h1 class="title">Online Image Gallery Search</h1>
      <div class="input-container">
        <input type="text" class="input" bind:value={searchterm} />
        <button class="button" on:click={handleSubmit}>Search</button>
      </div>
    </div>
    <div class="photos">
      {#each photos as photo, i (photo.id)}
        <img
          src={photo.urls.regular}
          alt={photo.alt_description}
          class="image"
          in:fly={{ y: 200, duration: 2000, delay: i * 100 }}
          out:fade
        />
      {/each}
    </div>
  </div>
</main>

<style>
  * {
    font-family: "Gill Sans", sans-serif;
  }
  .image {
    width: 400px;
    height: 250px;
    margin: 5px;
    border-radius: 5px;
  }
  .title {
    color: rgb(0, 0, 0);
    margin-bottom: 20px;
  }
  .photos {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  .container {
    width: 1230px;
    margin: 0 auto;
  }
  .header {
    text-align: center;
    font-size: 20px;
  }
  .input {
    padding: 10px;
    width: 400px;
    border-radius: 10px;
    outline: none;
    border: 1px solid rgb(230, 216, 216);
    font-size: 20px;
  }
  .button {
    padding: 10px;
    font-size: 20px;
    background-color: green;
    border-radius: 10px;
    border: none;
    color: white;
  }
  .input-container {
    margin-bottom: 40px;
  }
</style>
