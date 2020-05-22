<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import CommentsForm from "../components/CommentsForm.svelte";

  export let id;

  let token = document.cookie.substring(4);
  let parsedToken = "";
  
  if (document.cookie !== "") {
    parsedToken = JSON.parse(atob(token.split(".")[1]));
  }

  const apiBaseUrl = "http://localhost:8080/api/v2/posts";
  let comments = [];

  onMount(() => {
    axios
      .get(apiBaseUrl + "/" + id + "/comments")
      .then(response => {
        comments = response.data["body"]["response"];
      })
      .catch(error => console.log(error));
  });

  function addComment({ detail: comment }) {
    comments = [comment, ...comments];
  }
</script>

<style>
  .card .card-content .card-title {
    margin-bottom: 0;
  }

  .card .card-content p.timestamp {
    color: #999;
    margin-bottom: 10px;
  }

  .row .note {
    text-align: center;
    margin: 50px 0px 0px auto;
    display: block;
    font-size: 17px; 
  }
</style>

{#if document.cookie !== ''}
  <div class="row">
    <div class="col s12">
      <CommentsForm on:commentCreated={addComment} {id} />
    </div>
  </div>
{/if}

<div class="row">
  {#if comments.length === 0}
    <div>
      <p class="note">IT SEEMSE THAT THIS POST DOESN'T HAVE ANY COMMENTS.</p>
      <br />
      {#if document.cookie !== ''}
        <p class="note">BE THE FIRST ONE TO COMMENT ON THIS POST</p>
      {:else}
        <p class="note">
          IF YOU WANT TO BE THE FIRST ONE TO COMMENT LOGIN OR REGISTER
        </p>
      {/if}
    </div>
  {:else}
    {#each comments as comment (comment.id)}
      <div class="card">
        <div class="card-content">
          <p class="card-title">{comment.testo}</p>
          <p class="timestamp">{comment.data}</p>
          <p>Comment written by: <strong>{comment.user.username}</strong></p>
        </div>
      </div>
    {/each}
  {/if}
</div>
