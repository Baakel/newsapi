<script>
    export let imgSrc;
    export let title;
    export let description;
    export let link;
    export let author;
    export let date;
</script>

<div class="bg-neutral-800 text-neutral-300 rounded overflow-hidden mb-3 relative shadow-raised">
    <img 
        class="w-full h-32 sm:h-48 object-cover"
        src={imgSrc}
        alt="📰"
    />
    <div class="m-4 overflow-auto">
        <a href={link} class="mx-2 my-4">
            <span class="text-lg font-bold">{title}</span>
        </a>
        <p class="mx-2 my-4 text-sm">{description}</p>
        <div class="text-neutral-500 text-xs flex justify-evenly">
            <p>Story by: {author}</p>
            <p>{date}</p>
        </div>
    </div>
</div>