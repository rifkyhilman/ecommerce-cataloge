<template>
    <div class="container-card">
        <img src="../assets/images/sad-face.svg" />
    </div>
</template>

<script>
export default {
    name: "UnavailablePaage",
}
</script>

<style>

    .container-card {
        display: grid;
        grid-template-columns: 30% auto;
        gap: 16px;
        width: 80%;
        height: 70vh;
        z-index: 2;
        background-color: var(--white);
        border-radius: 10px;
        padding: 48px 32px;
        box-shadow: 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -2px rgba(0,0,0,.1);
    }

</style>