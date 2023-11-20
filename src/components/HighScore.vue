<template>
    <h1>Your highscore!</h1>
    <p>{{ resultOf }}</p>
</template>
<script>
export default{
    props: ['resultOf'],
};
</script>

<style scoped>
p{
    text-align: center;
}
</style>