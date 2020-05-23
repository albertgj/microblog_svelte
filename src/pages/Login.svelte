<script>
  import axios from "axios";
  import { navigate, link } from "svelte-routing";
  import Account from "svelte-material-icons/Account.svelte";

  let loading = false;
  const apiBaseUrl = "http://localhost:8080/api/v2/";
  let jwttoken;
  let username = "";
  let password = "";
  let loginResp = "";

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
        loginResp = "Bad Credentials. Retry !";
        loading = false;
      });

    function reloadPage() {
      location.reload();
      return;
    }
  }
</script>

<style>
  .input-field input[type="date"]:focus + label,
  .input-field input[type="text"]:focus + label,
  .input-field input[type="email"]:focus + label,
  .input-field input[type="password"]:focus + label {
    color: #3f50b5;
  }
  .input-field input[type="text"]:focus,
  .input-field input[type="password"]:focus {
    border-bottom: 2px solid #3f50b5;
    box-shadow: none;
  }
  .input-field label {
    left: 0;
  }
  form {
    margin: 30px auto;
    align-items: center;
    justify-content: center;
    width: 45%;
  }
  .btn-large {
    height: 45px;
    line-height: 45px;
  }
  .m-b-none {
    margin-bottom: 0;
  }
  .padder-x {
    padding: 0 !important;
  }
  .m-md {
    margin-top: 10px;
    margin-bottom: 20px;
  }
  .register {
    font-weight: bold;
  }
  h2 {
    margin-top: 50px;
    font-weight: 400;
    letter-spacing: 2px;
    padding: 20px;
  }
  @media (max-width: 700px) {
    form {
      width: 60%;
    }
  }
  @media (max-width: 530px) {
    form {
      width: 70%;
    }
  }
  @media (max-width: 450px) {
    form {
      width: 80%;
    }
  }
</style>

<div class="container">
  <div class="row m-b-none">
    <h2 class="center-align grey-text darken-4">
      <Account size={'50px'} color={"black"} width={'50px'} />
    </h2>
    <form on:submit={onSubmit}>
      <div class="input-field">
        <input
          class="validate"
          type="text"
          name="username"
          id="username"
          bind:value={username} />
        <label for="username">Enter your username</label>
      </div>
      <div class="input-field col s12 padder-x">
        <input
          class="validate"
          type="password"
          name="password"
          id="password"
          bind:value={password} />
        <label for="password">Enter your password</label>
      </div>
      <button type="submit" class="col s12 btn waves-effect waves-light btn btn-large m-md indigo">Login</button>
      <div>
        <label>
          <h6>
            <a class="grey-text lighten-4 register" use:link href="/register">
              Register
            </a>
          </h6>
        </label>
        <p class="center-align">{loginResp}</p>
      </div>
    </form>
  </div>
</div>
