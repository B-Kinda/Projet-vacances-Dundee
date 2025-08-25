<script>
    import PageAnneSophie from"./page/annesophie.svelte";
    let page = "accueil";
</script>

<main>
    <header>
        <nav class="navbar">
            <button class="navbar__accueil" aria-label="accueil"
                ><a
                    href="./App.svelte"
                    class="navbar__accueil-link"
                    aria-label="accueil">Accueil</a
                ></button
            >
            <button class="navbar__baptiste" aria-label="baptiste"
                ><a href="@" class="navbar__baptiste-link" aria-label="baptiste"
                    >Baptiste</a
                ></button
            >
            <button class="navbar__julien" aria-label="julien"
                ><a href="@" class="navbar__julien-link" aria-label="julien"
                    >Julien</a
                ></button
            >
            <button class="navbar__gael" aria-label="gael"
                ><a href="@" class="navbar__gael-link" aria-label="gael">Gaël</a
                ></button
            >
            <button on:click={() => page = "annesophie"} class="navbar__annesophie" aria-label="annesophie">Anne-Sophie</button>
                {#if page === "annesophie"}
                    <PageAnneSophie />
                {/if}
            <button class="navbar__richard" aria-label="richard"
                ><a href="@" class="navbar__richard-link" aria-label="richard"
                    >Richard</a
                ></button
            >
        </nav>
    </header>
    <div class="cards">
        <section class="cards__baptiste"></section>
        <section class="cards__julien"></section>
        <section class="cards__gael"></section>
        <section class="cards__annesophie"></section>
        <section class="cards__richard"></section>
    </div>
    <footer></footer>
</main>

<style>
    header {
        background-color: rgb(163, 236, 212);
        margin: 1rem 2rem;
        background-image: url();
    }
    nav {
        display: flex;
    }
    button {
        margin: 2rem 5rem;
        width: 165px;
        height: 65px;
    }
    a {
        text-transform: none;
        text-decoration: none;
        color: black;
    }
    .navbar__baptiste {
        background-image: url(../public/pasteque.jpeg);
        background-repeat: no-repeat;
        background-size: cover;
        border-radius: 0.9rem;
        border-style: none;
        border: 0.2rem solid;
        /* border-color: linear-gradient(to right, #d86348, #1e1e1e) 1; */
    }
</style>
