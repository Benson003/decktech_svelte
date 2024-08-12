<script>
    import PocketBase from "pocketbase";
    import { onMount } from "svelte";
    import Block from "./block.svelte";

    const pb = new PocketBase("http://127.0.0.1:8090");
    let table = [];


    async function fetchData(){
        const result = await pb.collection("blog").getFullList({ expand:"user" });
        let tempTable = []
       result.forEach(result=>{
        tempTable.push(
            {
                id:result.id, 
                title:result.title,
                content:result.content,
                user:result.expand.user.username, 
                first_name:result.expand.user.first_name,
                last_name:result.expand.user.last_name,
            })
    });
    table = tempTable;
    console.log(tempTable);
    }
    
    onMount(async ()=>{
        await fetchData();
    });
</script>
<main class="md:justify-center lg:justify-center">
    <Block />
    
    <h1>Blog Site</h1>

{#each table as blog (blog.id)}
     <div class="container rounded-md bg-slate-400 m-5 p-2 w-auto h-auto md:w-11/12">
        <h1 class="text-center font-bold text-2xl">{blog.title}</h1>
        <p class="text-lg text-center">{blog.content}</p>
        <p class="text-right text-slate-700 text-sm">Author: {blog.first_name} {blog.last_name}</p>
     </div>
{/each}
    
</main>