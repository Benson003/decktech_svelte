<script>
    import PocketBase from "pocketbase";
    import { onMount } from "svelte";
    import Block from "./block.svelte";

    const pb = new PocketBase("http://127.0.0.1:8090");
    let table = [];


    async function fetchData(){
        const result = await pb.collection("blog").getFullList({ expand:"user" });
        console.log(result)
        let tempTable = []
       result.forEach(result=>{
        tempTable.push(
            {
                id:result.id, 
                title:result.title,
                content:result.content,
                user:result.expand.user.username, 
            })
    });
    table = tempTable;
    }
    
    onMount(async ()=>{
        await fetchData();
    });
</script>
<main>
    <Block />
    
    <h1>Blog Site</h1>

{#each table as blog (blog.id)}
     <div>
        <h1>{blog.title}</h1>
        <p>{blog.content}</p>
        <small>Author: {blog.user}</small>
     </div>
{/each}
    
</main>