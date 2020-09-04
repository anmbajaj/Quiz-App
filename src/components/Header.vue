<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled>Question Box</b-nav-item>
            <b-nav-item disabled>Counters {{correctCount}}/{{totalCount}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props:{
        correctCount: Number,
        totalCount: Number
    }
}
</script>