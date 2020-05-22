<script>
  import { Link, links, navigate } from "svelte-routing";
  import Menu from "svelte-material-icons/Menu.svelte";
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
    var instances = M.Sidenav.init(elems, 200);
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
    navigate("/", { replace: true });
    reloadPage();
  }
</script>

<style>
  nav {
    background-color: black;
  }
</style>

<nav class="nav-extended">
  <div class="nav-wrapper">
    <div class="container">
      <a
        href="/"
        noroute
        data-target="mobile"
        class="sidenav-trigger">
        <i class="material-icons">
          <Menu />
        </i>
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
          <li>
            <a
              href="/placeholder"
              noroute
              class="sideMyBtn"
              on:click={event => {
                event.preventDefault();
                logoutBtn();
              }}>
              Logout
            </a>
          </li>
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

<ul class="sidenav sidenav-close" id="mobile">
  <li>
    <Link to="/">Home</Link>
  </li>
  <li>
    <Link to="/about">About</Link>
  </li>
  {#if document.cookie !== ''}
    <li>
      <a
        href="/placeholder"
        noroute
        class="sideMyBtn"
        on:click={event => {
          event.preventDefault();
          logoutBtn();
        }}>
        Logout
      </a>
    </li>
  {:else}
    <li>
      <Link to="/login">Login</Link>
    </li>
  {/if}
</ul>
