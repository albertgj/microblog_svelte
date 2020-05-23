<script>
  import { onMount } from "svelte";
  import { links } from "svelte-routing";
  import axios from "axios";
  import { Router, Route } from "svelte-routing";
  import PostForm from "../components/PostForm.svelte";
  import Account from "svelte-material-icons/Account.svelte";
  import Calendar from "svelte-material-icons/Calendar.svelte";

  const apiBaseUrl = "http://localhost:8080/api/v2/posts";
  //ICON CUSTOMIZATION
  let color = "blue";
  let size = 30;
  let width = 20;
  let height = 35;
  let viewBox = "2 0 20 4";
  let viewBoxCalendar = "-2 -2 25 14";
  //-----------------
  let posts = [];
  let token = document.cookie.substring(4);
  let parsedToken = "";
  let loading = false;
  let username = "";

  if (document.cookie !== "") {
    parsedToken = JSON.parse(atob(token.split(".")[1]));
    username = parsedToken["sub"];
  }

  onMount(() => {
    loading = true;

    axios.get(apiBaseUrl).then(response => {
      posts = response.data["body"]["response"];
      loading = false;
    });
  });

  function addPost({ detail: post }) {
    posts = [post, ...posts];
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

  .card .content {
    font-size: 20px;
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
  {#if document.cookie !== '' && parsedToken['ROLES'][0] === 'ROLE_ADMIN'}
    <div class="row">
      <div class="col s12">
        <PostForm on:postCreated={addPost} />
      </div>
    </div>
  {/if}
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
  {:else if posts.length === 0}
    <h3 class="center-align">No content.</h3>
  {:else}
    {#each posts as post (post.id)}
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
              <strong>{post.user.username}</strong>
            </p>
          </div>
          <div class="card-action">
            <a href="/posts/{post.id}/comments" use:links>VIEW COMMENTS</a>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
