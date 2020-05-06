<script>
    import axios from "axios";
    import { navigate } from "svelte-routing";

    let loading = false;
    const apiBaseUrl =
            "http://localhost:8080/api/v2/";
    let jwt;
    let username = '';
    let password = '';

    function onSubmit(event) {
        loading = true;
        event.preventDefault();


        if(username.trim() === '' || password.trim() === '') {
            return;
        }

        const persona = {username, password};

        axios({
            url: apiBaseUrl + 'authenticate',
            method:'POST',
            data: persona
        }).then((response) => {
            jwt = response.data;
            document.cookie = "jwt="+jwt.jwttoken;
            navigate("/", {replace: true});
        }).catch(function(error) {
            console.log(error)
        });

        loading = true;
    }
</script>

<style>
</style>

<div class="row">
    <div class="col s6">
        <form on:submit={onSubmit}>
            <div class="center-align">
                <div class="input-field col s12">
                    <input type="text" bind:value={username}>
                    <label for="text">Username</label>
                </div>
                <div class="input-field col s12">
                    <input type="password" bind:value={password}>
                    <label for="password">Password</label>
                </div>
            </div>
            <div class="center-align">
                <button type="submit" class="waves-effect waves-light btn">LOGIN</button>
            </div>
        </form>
    </div>
</div>
