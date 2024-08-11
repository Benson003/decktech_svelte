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
    export let identifier; 
    onMount(async ()=>{
        await fetchData()
       
        let div = document.getElementById("clicked")
        div.addEventListener('click',(e)=>{
        let tag = e.target.dataset.id;
        identifier = tag;
    });

});



</script>
<main>
    <Block />
    <h1>Projects Site</h1>
    {#each table as project (project.id)}
    <div class = "container w-1/6 h-1/3 m-4 rounded-md bg-slate-300 p-3" id = "clicked" >
         <img src="{project.projectImage}" alt="Resourse not found" class=" aspect-[4/3] md:aspect-[16/9] rounded" data-id={project.id}>
         <h3 class="font-bold" data-id="{project.id}">{project.title }</h3>
         <p data-id="{project.id}">{project.content}</p>
    </div>
    {/each}

</main>