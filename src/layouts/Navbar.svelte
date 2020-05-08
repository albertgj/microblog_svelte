<script>
  import { Link } from "svelte-routing";
  let tokenMilliseconds;
  let d;
  let username;

  if(document.cookie === '') {

  }else {
    tokenMilliseconds = JSON.parse(atob(document.cookie.substring(4).split('.')[1]))['exp'];
    d = new Date(tokenMilliseconds*1000);
    username = JSON.parse(atob(document.cookie.substring(4).split('.')[1]))['sub'];
  }


</script>

<style>
  nav {
    background-color: black;
  }
</style>



<nav>
  <div class="nav-wrapper">
    <div class="container">
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
        {#if !(d >  Date().toLocaleString())}
          &nbsp; Welcome back, {username}
        {:else}
            <li>
              <Link to="/login">Login</Link>
            </li>
        {/if}
      </ul>
    </div>
  </div>
</nav>
