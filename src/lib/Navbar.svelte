<script>
    import "./Navbar.css"
    import {Router,Route,Link} from 'svelte-routing';
    import Home from "./Home.svelte";
    import Blog from "./Blog.svelte";
    import Members from "./Members.svelte";
    import Projects from "./Projects.svelte";
    import About from "./About.svelte";


    export const customLinks = [
        {name:"Home", href:"/",id:0},
        {name:"Projects", href:"/projects",id:1},
        {name:"Blog", href:"/blog",id:2},
        {name:"Members", href:"/members",id:3},
        {name:"About", href:"/about",id:4},
    ]
</script>
<main>
    <div class="container">
        <ul class="navbar">
            {#each customLinks as link (link.id)}
                <li class="nav_link"><a href="{link.href}">{link.name}</a></li>
            {/each}
        </ul>
    </div>

    <Router>
        <Route path="/" component="{Home}"/>
        <Route path="/projects" component="{Projects}"/>
        <Route path="/blog" component="{Blog}"/>
        <Route path="/members" component="{Members}"/>
        <Route path="/about" component="{About}"/>
    </Router>
</main>
<style>

</style>