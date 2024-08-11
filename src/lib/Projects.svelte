<script>
    import Block from "./block.svelte";
    import PocketBase from "pocketbase"
    import {onMount} from "svelte"
    

    const pb = new PocketBase("http://127.0.0.1:8090");
    let table = []

    async function fetchData() {
        const projects = await pb.collection("projects").getFullList();
        let tempTable = [];
        projects.forEach(projects => {

            const imageUrl = pb.files.getUrl(projects,projects.projectImage)
            console.log(imageUrl)
            tempTable.push(
                {
                    id:projects.id,
                    title:projects.title,
                    content:projects.content,
                    projectImage:imageUrl,
                }
            )
        });

        table = tempTable;
    }

    onMount(async ()=>{
        await fetchData()
    });

</script>
<main>
    <Block />
    <h1>Projects Site</h1>
    {#each table as project (project.id)}
    <div>
        <img src="{project.projectImage}" alt="Resourse not found">
         <h3>{project.title }</h3>
         <p>{project.content}</p>
    </div>
    {/each}

</main>