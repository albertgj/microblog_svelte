<script>
  import { Link, links } from "svelte-routing";
  let tokenMilliseconds;
  let d;
  let username;

  if (document.cookie === "") {
  } else {
    tokenMilliseconds = JSON.parse(
      atob(document.cookie.substring(4).split(".")[1])
    )["exp"];
    d = new Date(tokenMilliseconds * 1000);
    username = JSON.parse(atob(document.cookie.substring(4).split(".")[1]))[
      "sub"
    ];
  }

  document.addEventListener("DOMContentLoaded", function() {
    var elems = document.querySelectorAll(".sidenav");
    var instances = M.Sidenav.init(elems, 0.5);
  });

  function reloadPage() {
    location.reload();
    return;
  }

  function logoutBtn() {
    document.cookie =
      "jwt= " +
      document.cookie.substring(4) +
      "; expires = Thu, 01 Jan 1970 00:00:00 GMT";
    reloadPage();
  }
</script>

<style>
  nav {
    background-color: black;
  }

  .myBtn {
    background-color: black;
    color: white;
    border: 0;
    box-shadow: none;
    border-radius: 0px;
  }
</style>

<nav class="nav-extended">
  <div class="nav-wrapper">
    <div class="container">
      <a
        href="/placeholder"
        noroute
        data-target="mobile-demo"
        class="sidenav-trigger">
        <i class="material-icons">menu</i>
      </a>
      <Link to="/">
        <span class="brand-logo">Microblog</span>
      </Link>
      <ul id="nav-mobile" class="right hide-on-med-and-down">
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        &nbsp;
        {#if document.cookie !== ''}
          <button class="myBtn" on:click={logoutBtn}>Logout</button>
          &nbsp;&nbsp; Hello, {username}
        {:else}
          <li>
            <Link to="/login">Login</Link>
          </li>
        {/if}
      </ul>
    </div>
  </div>
</nav>

<ul class="sidenav" id="mobile-demo">
  <li>
    <Link to="/">Home</Link>
  </li>
  <li>
    <Link to="/about">About</Link>
  </li>
  {#if document.cookie !== ''}
    <p />
  {:else}
    <li>
      <Link to="/login">Login</Link>
    </li>
  {/if}
</ul>
