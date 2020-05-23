<script>
  import { createEventDispatcher } from "svelte";
  import { onMount } from "svelte";
  import axios from "axios";
  const dispatch = createEventDispatcher();

  let titolo = "";
  let text = "";
  let username = "";
  let userId = "";
  let token = document.cookie.substring(4);

  let loading = false;

  var d = new Date(),
    dformat =
      [d.getFullYear(), d.getMonth() + 1, d.getDate()].join("-") +
      " " +
      [d.getHours(), d.getMinutes(), d.getSeconds()].join(":");

  let post;

  if (document.cookie !== "") {
    username = JSON.parse(atob(token.split(".")[1]))["sub"];

    userId = JSON.parse(atob(token.split(".")[1]))["user_id"];
  }

  const apiBaseUrl = "http://localhost:8080/api/v2/posts";

  function onSubmit(event) {
    event.preventDefault();

    if (text.trim() === "" || titolo.trim() === "") {
      return;
    }

    loading = true;

    let myPost = {
      titolo: titolo,
      data: dformat,
      text: text,
      user: {
        id: userId,
        username: username
      }
    };

    //POST
    axios({
      url: apiBaseUrl,
      method: "POST",
      headers: {
        Authorization: `Bearer ${token}`
      },
      data: myPost
    })
      .then(response => {
        post = response.data["body"]["response"];
        dispatch("postCreated", post);
      })
      .catch(error => console.log(error));

    loading = false;
  }
</script>

<style>

</style>

{#if !loading}
  <form on:submit={onSubmit}>
    <div class="input-field center-align">
      <label for="titolo">Titolo</label>
      <input type="text" bind:value={titolo} />
      <!-- Two way binding -->
    </div>
    <div class="input-field">
      <label for="text">Testo</label>
      <input type="text" bind:value={text} />
    </div>
    <div class="center-align">
      <button
        type="submit"
        class="waves-effect waves-light light-blue darken-4 btn">
        AGGIUNGI
      </button>
    </div>
  </form>
{:else}
  <div class="indeterminate" />
{/if}
