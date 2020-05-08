<script>
    import "../node_modules/materialize-css/dist/css/materialize.min.css";
    import "../node_modules/materialize-css/dist/js/materialize.min.js";
    import Login from './pages/Login.svelte';
    import {Router, Route} from "svelte-routing";
    import Navbar from "./layouts/Navbar.svelte";
    import Home from './pages/Home.svelte';
    import About from './pages/About.svelte';
    import Post from './pages/Post.svelte';
    import Comments from "./components/Comments.svelte";
    import {onMount} from "svelte";

    let res;
    let myJson;
    onMount(() => {
        let data = new Date
        let oraAttuale = [data.getHours(), data.getMinutes(), data.getSeconds()].join(':');

        //let tokenMilliseconds = JSON.parse(atob(document.cookie.substring(4).split('.')[1]))['exp'];
        //let oraJson = new Date(tokenMilliseconds*1000), oraToken = [oraJson.getHours(), oraJson.getMinutes(), oraJson.getSeconds()].join(':');
    });

</script>

<Router>

    <Navbar />
    <div class="container">
        <Route path="/about" component={About} />
        <Route path="/post" component={Post} />
        <Route path="/" component={document.cookie === '' ? Login : Home} />
        <Route path="/login" component={Login}/>
    </div>
</Router>

<style>

</style>