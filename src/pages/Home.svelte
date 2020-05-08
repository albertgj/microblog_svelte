<script>
  import {onMount} from "svelte";
  import {Link} from "svelte-routing";
  import axios from "axios";
  import {Router, Route} from "svelte-routing";
  import Post from '../pages/Post.svelte';
  import PostForm from '../components/PostForm.svelte'
  import Comments from "../components/Comments.svelte";

  const apiBaseUrl =
          "http://localhost:8080/api/v2/posts";
  let posts = [];
  let token = document.cookie.substring(4);
  let comments = [];

  onMount(() => {
    axios.get(apiBaseUrl, {
      headers: {
        'Authorization': `Bearer ${token}`
      }
    }).then(response => {
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

  document.addEventListener('DOMContentLoaded', function () {
    var elems = document.querySelectorAll('.modal');
    var instances = M.Modal.init(elems, 0.5);
  });

  function getComments(id) {
    axios.get(apiBaseUrl + `/${id}` + '/comments', {
      headers: {
        'Authorization': `Bearer ${token}`
      }
    }).then(response => {
      comments = response.data["body"]["response"];
    });
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

  h3 {
    text-align: center;
  }
</style>
{#if document.cookie !== '' }
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
            <p class="card-title">{post.titolo}</p>
            <p class="timestamp">{post.data}</p>
            <p>{post.text}</p>
            <p>Post created by: {post.persona.username}</p>
          </div>
          <div class="card-action">
            <button data-target="modal1" class="btn-flat modal-trigger" on:click={() => getComments(post.id)}>VIEW</button>
            <a href="#" on:click={() => editPost(post)}>Edit</a>
            <a href="#" on:click={() => deletePost(post.id)} class="delete-btn">
              Delete
            </a>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>

<!-- Modal Structure -->
<div id="modal1" class="modal">
  <div class="modal-content">
    <h4>Comments</h4>
    {#if comments.length === 0}
      <h3>NO COMMENTS IN THIS POST</h3>
    {:else}
      {#each comments as comment}
        <p>comment.id</p>
      {/each}
    {/if}
  </div>
  <div class="modal-footer">

  </div>
</div>