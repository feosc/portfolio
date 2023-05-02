<template>
    <div class="home">
        <Container class="align">
            <Slogan class="slogan" />
            <Form />
        </Container>
    </div>
</template>

<style scoped>
.home {
    height: auto;
    width: 100%;
    padding: 3rem;
}

.align {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 2rem;
    align-items: start;
}

.slogan {
    top: 0;
    /* Alinha o Slogan no topo */
}
</style>