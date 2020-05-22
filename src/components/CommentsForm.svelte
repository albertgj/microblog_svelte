<script>
  import { createEventDispatcher } from "svelte";
  import axios from "axios";

  const dispatch = createEventDispatcher();

  export let id;

  let testo = "";
  let userId = "";
  let username = "";

  let loading = false;
  var d = new Date(),
    dformat =
      [d.getFullYear(), d.getMonth() + 1, d.getDate()].join("-") +
      " " +
      [d.getHours(), d.getMinutes(), d.getSeconds()].join(":");

  let comment;

  if (document.cookie !== "") {
    username = JSON.parse(atob(document.cookie.substring(4).split(".")[1]))[
      "sub"
    ];
    userId = JSON.parse(atob(document.cookie.substring(4).split(".")[1]))[
      "user_id"
    ];
  }
  const apiBaseUrl = "http://localhost:8080/api/v2/comments";
  let token = document.cookie.substring(4);

  function onSubmit(event) {
    event.preventDefault();

    if (testo.trim() === "") {
      return;
    }

    let myComment = {
      data: dformat,
      testo: testo,
      post: {
        id: id
      },
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
      data: myComment
    })
    .then(response => {
        comment = response.data["body"]["response"];
        dispatch("commentCreated", comment);
        testo = '';
    })
    .catch(error => console.log(error));

    loading = true;

    loading = false;
  }
</script>

{#if !loading}
  <form on:submit={onSubmit}>
    <div class="input-field">
      <label for="text">Testo</label>
      <input type="text" bind:value={testo} />
    </div>
    <div class="center-align">
      <button type="submit" class="waves-effect waves-light light-blue darken-4 btn">
        AGGIUNGI
      </button>
    </div>
  </form>
{:else}
  <div class="indeterminate" />
{/if}
