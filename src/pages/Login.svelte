<script>
  import axios from "axios";
  import { navigate, link } from "svelte-routing";

  let loading = false;
  const apiBaseUrl = "http://localhost:8080/api/v2/";
  let jwttoken;
  let username = "";
  let password = "";
  //let tokenMilliseconds = JSON.parse(atob(document.cookie.substring(4).split('.')[1]))['exp'];
  //let dataToken = Date(tokenMilliseconds*1000);
  //let formatedData = dataToken.substring(0,2) + ", " + dataToken.substring(8, 10) + " " + dataToken.substring(4, 7) + " " + dataToken.substring(11, 15) + " " + dataToken.substring(16, 24) + " GMT";

  function onSubmit(event) {
    loading = true;
    event.preventDefault();

    if (username.trim() === "" || password.trim() === "") {
      return;
    }

    const persona = { username, password };

    axios({
      url: apiBaseUrl + "signin",
      method: "POST",
      data: persona
    })
      .then(response => {
        jwttoken = response.data["body"]["token"];
        let tokenMilliseconds = atob(jwttoken.split(".")[1])["exp"];
        let dataToken = Date(tokenMilliseconds * 1000);
        let formatedData =
          dataToken.substring(0, 2) +
          ", " +
          dataToken.substring(8, 10) +
          " " +
          dataToken.substring(4, 7) +
          " " +
          dataToken.substring(11, 15) +
          " " +
          dataToken.substring(16, 24) +
          " GMT";
        document.cookie =
          "jwt=" + jwttoken + ";expires=" + formatedData + ";path=/;";
        navigate("/", { replace: true });
        reloadPage();
      })
      .catch(function(error) {
        console.log(error);
        loading = false;
      });

    function reloadPage() {
      location.reload();
      return;
    }
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
    <h5 class="indigo-text">Please, login into your account</h5>
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
            Login
          </button>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s6 m6 l6">
          <p class="margin medium-small">
            <a href="/register" use:link>Register Now!</a>
          </p>
        </div>
        <div class="input-field col s6 m6 l6" />
      </div>
    </form>
  </div>
</div>
