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

  .preloader-background {
    display: flex;
    align-items: center;
    justify-content: center;

    position: relative;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }

  h3 {
    text-align: center;
  }
</style>

{#if loading}
  <p />
{:else}
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
{/if}

{#if document.cookie !== ''}
  <div class="row">
    <div class="col s12">
      <CommentsForm on:commentCreated={addComment} {id} />
    </div>
  </div>
{/if}

<div class="row">
  {#if loading}
    <div class="preloader-background">
      <div class="preloader-wrapper big active">
        <div class="spinner-layer spinner-blue-only">
          <div class="circle-clipper left">
            <div class="circle" />
          </div>
          <div class="gap-patch">
            <div class="circle" />
          </div>
          <div class="circle-clipper right">
            <div class="circle" />
          </div>
        </div>
      </div>
    </div>
  {:else if comments.length === 0}
    <h3>No comments.</h3>
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
