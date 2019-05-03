<template>
    <b-button 
        variant="light" 
        :to="`/game/${tableNumbers}`"
    >
    <h5>
    <div v-if="tableNumbers == 13">
        <span style="margin-left: 53px"><b>Advance Mode</b> </span>
        <span style="margin-left: 100px">High Score: {{scores.highscore}} </span>
    </div>
    <div v-else>
        <span style="margin-right: 90px">Multiplication Table: <b>{{tableNumbers}}</b></span>
        <span>High Score: {{scores.highscore}} </span>
    </div>
    </h5>
    </b-button > 
</template>

<script>
export default {
    name: "Table",

    // props pass by the parent 
    props: [
        "scores",
        "tableNumbers"
    ],
}
</script>
<style scoped>

</style>




