<script>
  import {onMount} from "svelte";
  import { links } from "svelte-routing";
  import axios from "axios";
  import {Router, Route} from "svelte-routing";
  import PostForm from '../components/PostForm.svelte';
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

  if(document.cookie !== '') {
    parsedToken = JSON.parse(atob(token.split(".")[1]))
  }

  onMount(() => {
    axios.get(apiBaseUrl).then(response => {
      posts = response.data["body"]["response"];
    });
  });

  function editPost(post) {
    console.log(post);
  }

  function deletePost(id) {
    if (confirm("Are you sure")) {
      axios({
        url: apiBaseUrl + `/${id}`,
        method: 'DELETE',
        headers: {
          Authorization: "Bearer " + `${token}`
        }
      }).then(() => {
        posts = posts.filter(p => p.id !== id);
      }).catch(function (error) {
        console.log(error)
      });
    }
  }

  function addPost({detail: post}) {
    posts = [post, ...posts];
  }
</script>

<style>
  .delete-btn {
    color: red !important;
  }
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

  h3 {
    text-align: center;
  }
</style>



{#if document.cookie !== '' && parsedToken["ROLES"][0] === 'ROLE_ADMIN'}
  <div class="row">
    <div class="col s12">
      <PostForm on:postCreated={addPost}/>
    </div>
  </div>
{/if}

<div class="row">
  {#if posts.length===0}
    <h3 class="center-align">NO CONTENT...</h3>
  {:else}
    {#each posts as post (post.id)}
      <div class="row">
        <div class="card">
          <div class="card-content">
            <p class="card-title">Title: {post.titolo}</p>
            <p class="timestamp"><Calendar size={24} {width} viewBox={viewBoxCalendar}/> {post.data}</p>
            <p class="content">{post.text}</p>
            <br>
            
            <p><Account {color} {width} {height} {viewBox}/> <strong>{post.user.username}</strong></p>
          </div>
          <div class="card-action">
            <a href="/posts/{post.id}/comments" use:links>VIEW COMMENTS</a>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>