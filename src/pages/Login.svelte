<script>
    import axios from "axios";
    import { navigate } from "svelte-routing";

    let loading = false;
    const apiBaseUrl =
            "http://localhost:8080/api/v2/";
    let jwt;
    let username = '';
    let password = '';
    //let tokenMilliseconds = JSON.parse(atob(document.cookie.substring(4).split('.')[1]))['exp'];
    //let dataToken = Date(tokenMilliseconds*1000);
    //let formatedData = dataToken.substring(0,2) + ", " + dataToken.substring(8, 10) + " " + dataToken.substring(4, 7) + " " + dataToken.substring(11, 15) + " " + dataToken.substring(16, 24) + " GMT";

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
            let jwttoken = jwt.jwttoken;
            let tokenMilliseconds = atob(jwttoken.split('.')[1])['exp'];
            alert(tokenMilliseconds);
            let dataToken = Date(tokenMilliseconds*1000);
            let formatedData = dataToken.substring(0,2) + ", " + dataToken.substring(8, 10) + " " + dataToken.substring(4, 7) + " " + dataToken.substring(11, 15) + " " + dataToken.substring(16, 24) + " GMT";
            document.cookie = "jwt="+jwttoken+"; expires="+formatedData+";path=/;";
            reloadPage();
            navigate("/", {replace: true});
        }).catch(function(error) {
            console.log(error)
            loading = false;
        });
    }

    function reloadPage() {
        location.reload();
        return false;
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
