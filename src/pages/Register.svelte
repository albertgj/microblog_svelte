<script>
  import axios from "axios";
  import { navigate, link } from "svelte-routing";

  let loading = false;
  const apiBaseUrl = "http://localhost:8080/api/v2/";
  let jwttoken;
  let username = "";
  let password = "";
  let regResponse = "";

  function onSubmit(event) {
    loading = true;
    event.preventDefault();
    if (username.trim() === "" || password.trim() === "") {
      return;
    }

    let role = ["user"];

    const persona = { username, password, role };

    axios({
      url: apiBaseUrl + "signup",
      method: "POST",
      data: persona
    })
      .then(response => {
        regResponse = response.data["body"]["message"];
      })
      .catch(function(error) {
        console.log(error);
        loading = false;
      });
  }
</script>

<style>
  #login-page {
    padding-top: 10%;
    width: 500px;
  }
</style>

<div id="login-page" class="row">
  <div class="center-align">
    <h5 class="indigo-text">Register a new account !</h5>
    <div class="section" />
  </div>

  <div class="col s12 z-depth-6 card-panel">
    <form class="login-form" on:submit={onSubmit}>
      <div class="row" />
      <div class="row">
        <div class="input-field col s12">
          <input
            class="validate"
            type="text"
            name="username"
            id="username"
            bind:value={username} />
          <label for="username">Enter your username</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input
            class="validate"
            type="password"
            name="password"
            id="password"
            bind:value={password} />
          <label for="password">Enter your password</label>
        </div>
      </div>
      <div class="row" />
      <div class="row">
        <div class="center-align">
          <button
            type="submit"
            name="btn_login"
            class="col s12 btn btn-large waves-effect indigo">
            Register
          </button>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s6 m6 l6">
            {regResponse}
        </div>
        <div class="input-field col s4" />
      </div>
    </form>
  </div>
</div>
