<template>
    <div>

        <h5>{{ user }}</h5>
        <p>{{ post }}</p>

        <div>
            <button @click="likeItem">Like</button>
            <p>{{ likes }}</p>
            <button @click="dislikeItem">Dislike</button>
        </div>
        
    </div>
</template>

<script>
    export default {
        props: {
            id: Number,
            user: String, //utilizator
            post: String, //postarea
            likes: Number,
            tag: String   //filtru de gen ...
        },


        emits: ['like', 'dislike'],

        methods: {
            likeItem() {
                this.$emit("like", {itemId: this.id})
            },

            dislikeItem() {
                this.$emit("dislike", {itemId: this.id})

            }
        },
    }
</script>

<style lang="scss" scoped>

</style>