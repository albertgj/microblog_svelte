<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import CommentsForm from "../components/CommentsForm.svelte";
  import Account from "svelte-material-icons/Account.svelte";
  import Calendar from "svelte-material-icons/Calendar.svelte";

  export let id;

  let token = document.cookie.substring(4);
  let parsedToken = "";
  let post = {};
  let loading = false;
  let postUsername = "";

  //ICON CUSTOMIZATION
  let color = "blue";
  let size = 30;
  let width = 20;
  let height = 35;
  let viewBox = "2 0 20 4";
  let viewBoxCalendar = "-2 -2 25 14";
  //-----------------

  if (document.cookie !== "") {
    parsedToken = JSON.parse(atob(token.split(".")[1]));
  }

  const apiBaseUrl = "http://localhost:8080/api/v2/posts";
  let comments = [];

  onMount(() => {
    loading = true;
    //GETTING POST
    axios
      .get(apiBaseUrl + "/" + id)
      .then(response => {
        post = response.data["body"]["response"];
        postUsername = post.user.username;
        loading = false;
      })
      .catch(error => console.log(error));

    //GETTING COMMENTS OF POST
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
  p.testo {
    margin-bottom: 0;
  }

  p.timestamp {
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

{#if !loading}
  <div class="row">
    <div class="card">
      <div class="card-content">
        <p class="card-title">Title: {post.titolo}</p>
        <p class="timestamp">
          <Calendar size={24} {width} viewBox={viewBoxCalendar} />
          {post.data}
        </p>
        <p class="content">{post.text}</p>
        <br />
        <p>
          <Account {color} {width} {height} {viewBox} />
          <strong>{postUsername}</strong>
        </p>
      </div>
    </div>
  </div>
{:else}
  <div class="indeterminate" />
{/if}

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
      <ul class="collection">
        <li class="collection-item">
          <p class="testo">{comment.testo}</p>
          <p class="timestamp">{comment.data}</p>
          <p>
            Comment written by:
            <strong>{comment.user.username}</strong>
          </p>
        </li>
      </ul>
    {/each}
  {/if}
</div>
