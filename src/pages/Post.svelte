<script>
    import Comments from "../components/Comments.svelte";
    import { onMount } from "svelte";
    import axios from "axios";

    export let id;
    const apiBaseUrl =
            "http://localhost:8080/api/v2/posts/";
    let post;
    let token = document.cookie.substring(4);

    $: post = axios.get(apiBaseUrl + id, {headers: {'Authorization': `Bearer ${token}`}});


</script>

<style>

</style>

<Comments post={post}/>