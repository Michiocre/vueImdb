<template>
    <div class="card shadow-sm h-100">
        <img loading="lazy" class="card-img-top myImage" :src="src" alt="Card image cap">
        <div class="card-body">
            <h5 class="card-title">{{title}}</h5>
            <span class="h6">Year</span>
            {{year}}
            <br>
            <span class="h6 mt-1">IMDb Rating</span>
            {{rating}}
        </div>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: ['src', 'title', 'year', 'rating'],
    components: {
    }
}
</script>

<style scoped>
.myImage {
    height: 300px;
    object-fit: cover;
}
</style>