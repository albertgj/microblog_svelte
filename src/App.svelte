<script>
    import "../node_modules/materialize-css/dist/css/materialize.min.css";
    import "../node_modules/materialize-css/dist/js/materialize.min.js";
    import Login from './pages/Login.svelte';
    import {Router, Route} from "svelte-routing";
    import Navbar from "./layouts/Navbar.svelte";
    import Home from './pages/Home.svelte';
    import About from './pages/About.svelte';
    import Comments from './pages/Comments.svelte';
    import Register from './pages/Register.svelte';
</script>

<Router>
    <Navbar />
    <div class="container">
        <Route path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="/login" component={Login}/>
        <Route path="/posts/:id/comments" let:params>
            <Comments id={params.id}/>
        </Route>
        <Route path="/register" component={Register}/>
    </div>
</Router>