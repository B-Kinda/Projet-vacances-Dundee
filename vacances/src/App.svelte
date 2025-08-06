<script>
</script>

<main>
    <header>
        <nav class="navbar">
            <button class="navbar__acceuil" aria-label="acceuil"
                ><a
                    href="./App.svelte"
                    class="navbar__acceuil-link"
                    aria-label="acceuil">Acceuil</a
                ></button
            >
            <button class="navbar__baptiste" aria-label="baptiste"
                ><a href="@" class="navbar__baptiste-link" aria-label="baptiste"
                ></a></button
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
            <button class="navbar__annesophie" aria-label="annsophie"
                ><a
                    href="@"
                    class="navbar__annesophie-link"
                    aria-label="annsophie">Anne-Sophie</a
                ></button
            >
        </nav>
    </header>
    <div class="cards">
        <section class="cards__baptiste"></section>
        <section class="cards__julien"></section>
        <section class="cards__gael"></section>
        <section class="cards__annesophie"></section>
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
        background-image: url(../public/Cassette\ 00.png);
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center;
        border-radius: 0.9rem;
        border-style: none;
        border: 0.2rem solid;
    }
    .navbar__baptiste:hover {
        background-image: url(../public/pasteque.jpeg);
        background-repeat: no-repeat;
        background-size: cover;
    }
</style>
