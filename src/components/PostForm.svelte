<script>
    import { createEventDispatcher } from 'svelte';
    import axios from "axios";

    const dispatch = createEventDispatcher();

    const apiBaseUrl =
            "http://localhost:8080/api/v2/posts";
    let token = document.cookie.substring(4);
    let titolo = '';
    let text = '';
    let data = '2020-03-28 22:17:00';
    let usernames = JSON.parse(atob(document.cookie.substring(4).split(".")[1]))["sub"];

    let persona = {
        username: 'aaa',
        password: 'aaa'
    }

    let posts;
    let post;
    let loading = false;

    function onSubmit(event) {
        event.preventDefault();

        if(titolo.trim() === '' || text.trim() === '') {
            return;
        }

        loading = true;

        axios.
        get(apiBaseUrl, {
            headers: {
                'Authorization': `Bearer ${token}`
            }
        })
                .then(response => {
                    posts = response.data["body"]["response"];
                });

        const newPost = { titolo, text, persona, data}

        axios({
            url: apiBaseUrl,
            method:'POST',
            headers : {
                Authorization : "Bearer " + `${token}`
            },
            data: newPost
        }).then((response) => {
            post= response.data
            dispatch('postCreated', post);

        }).catch(function(error) {
            console.log(error)
        });

        titolo = text = '';
        loading = false;
    }
</script>

<style>
    form {
        margin: 50px;
    }

    .progress {
        margin: 100px 0;
    }
</style>

{#if !loading}
    <form on:submit={onSubmit}>
        <div class="input-field center-align">
            <label for="titolo">Titolo</label>
            <input type="text" bind:value={titolo}> <!-- Two way binding -->
        </div>
        <div class="input-field">
            <label for="text">Testo</label>
            <input type="text" bind:value={text}>
        </div>
        <div class="center-align">
            <button type="submit" class="waves-effect waves-light btn">AGGIUNGI</button>
        </div>
    </form>
{:else}
    <div class="indeterminate"></div>
{/if}
