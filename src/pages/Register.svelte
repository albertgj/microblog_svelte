<script>
  import axios from "axios";
  import { navigate, link } from "svelte-routing";
  import AccountPlus from "svelte-material-icons/AccountPlus.svelte";

  let loading = false;
  const apiBaseUrl = "http://localhost:8080/api/v2/";
  let jwttoken;
  let username = "";
  let password = "";
  let password2 = "";
  let regResponse = "";

  function onSubmit(event) {
    loading = true;
    event.preventDefault();
    if (username.trim() === "" || password.trim() === "") {
      return;
    } else if (password !== password2) {
      regResponse = "The passwords don't match. Try again !"
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
      <AccountPlus size={'50px'} color={'black'} width={'50px'} />
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
      <div class="input-field col s12 padder-x">
        <input
          class="validate"
          type="password"
          name="passwordC"
          id="passwordC"
          bind:value={password2} />
        <label for="passwordC">Confirm your password</label>
      </div>
      <button
        type="submit"
        class="col s12 btn waves-effect waves-light btn btn-large m-md indigo">
        Register
      </button>
      <div class="center-align">
        {regResponse}
      </div>
    </form>
  </div>
</div>
