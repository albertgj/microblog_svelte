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
    let res;
    let myJson;

</script>

<Router>

    <Navbar />
    <div class="container">
        <Route path="/about" component={About} />
        <Route path="/post" component={Post} />
        <Route path="/" component={document.cookie == '' ? Login : Home} />
    </div>
</Router>

<style>

</style>