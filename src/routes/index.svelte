<script lang="ts">
    import Input from "$lib/Input.svelte";
    import SquareButton from "$lib/SquareButton.svelte";
    import Card from "$lib/Card.svelte";

    const baseUrl = "https://newsapi.org/v2/top-headlines"
    const apiKey = "cb39fc34dbb342a9bfdf7a2a524a9f22"
    let articlesList;
    let pageSize = 10;
    let pageNo = 1;
    let totalPages;
    let searchTerm;

    const getArticles = async (term, pageSize?, pageNo?) => {
        fetch(`${baseUrl}?apiKey=${apiKey}&pageSize=${pageSize}&page=${pageNo}&q=${term}`)
        .then(r => r.json())
        .then(data => {
            if (data.status === "error") {
                articlesList = null
                return
            }
            articlesList = data;
        })
    }

    const changePageSize = (amount, term) => {
        pageSize = amount;
        pageNo = 1;
        getArticles(term, pageSize, pageNo)
    }

    $: if (articlesList) {
        totalPages = Math.ceil(articlesList.totalResults/pageSize)
        console.log(totalPages)
    }

</script>

<h1 class="text-4xl font-extrabold text-neutral-300 text-center m-8">Get top-headlines from around the world!</h1>
<form class="bg-neutral-800 shadow-raised my-8 mx-12 p-4 rounded-lg" on:submit|preventDefault={() => getArticles(searchTerm, pageSize, pageNo)}>
    <div class="flex justify-around items-center text-neutral-300">
        <div>
            <Input bind:value={searchTerm} />
            <button type="submit" class="pointer bg-gradient-to-br from-green-400 to-blue-400 m-4 p-2 rounded-lg">🔎</button>
        </div>
        <div>
            <h2 class="text-lg font-extrabold mb-4">Results per page</h2>
            <div class="flex gap-3">
                <SquareButton active={pageSize === 5} on:click={() => { changePageSize(5, searchTerm)}}>5</SquareButton>
                <SquareButton active={pageSize === 10} on:click={() => { changePageSize(10, searchTerm)}}>10</SquareButton>
                <SquareButton active={pageSize === 20} on:click={() => { changePageSize(20, searchTerm)}}>20</SquareButton>
            </div>
        </div>
    </div>
</form>
<div class="mx-12">
    {#if !!articlesList}
        <div class="grid gap-3 md:grid-cols-3 lg:grid-cols-5">
            {#each articlesList.articles as article}
                <Card imgSrc={article.urlToImage} title={article.title} author=
                {article.author ? article.author : "Anon"} date={new Date(article.publishedAt).toDateString()} description={article.description ? article.description : "Unkown"} link={article.url} />
            {/each}
        </div>
        {#if totalPages > 1}
            <div class="flex gap-2 overflow-auto justify-center rounded-lg my-2 py-4 px-2 bg-neutral-800 shadow-raised">
                {#each [...Array(totalPages).keys()] as page}
                    <SquareButton 
                    active={page + 1 === pageNo}
                    on:click={() => {
                        pageNo = page + 1;
                        getArticles(searchTerm, pageSize, page + 1);
                        }}
                    >
                        {page + 1}
                    </SquareButton>
                {/each}
            </div>
        {/if}
    {/if}
</div>
