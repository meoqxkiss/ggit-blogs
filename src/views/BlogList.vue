<template>
    <div>
        <h1>BUNA!!!</h1>
        <h1>Blogul GGIT FD</h1>
        <img href="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTrlI5dK2yqyaC_loN9aR-Y9CT8DpiT85lgVw&s">
        <img href="https://preview.redd.it/r-curseddogs-v0-vhivlmk4dq391.jpg?width=640&crop=smart&auto=webp&s=dc3d10b68d18c2ae5231f61ab6f0bdf12abbc6ed">
        <div v-if="blogList.length > 0">
            <BlogItem v-for="blog in blogList" :key="blog.id" :id="blog.id" :user="blog.user" :post="blog.post"
                :likes="blog.likes" :tag="blog.tag" @like="likeHandler($event)" @dislike="dislikeHandler($event)" />
        </div>
        <div v-else>
            <p>Nici un post nu a fost creat</p>
        </div>
        <div>
            <input v-model="user" />
            <textarea v-model="post"></textarea>
            <select v-model="tag">
                <option value="general">General</option>
                <option value="flame">Flame</option>
                <option value="recommendation">Recommendation</option>
            </select>
            <button @click="submitFormHandler">Save</button>
        </div>
    </div>
</template>

<script>
import { myaxios } from '../axios';
import BlogItem from '../components/BlogItem.vue';
export default {
    components: { BlogItem },
    data() {
        return {
            user: "",
            post: "",
            tag: "",
            blogList: []
        }
    },
    methods: {
        submitFormHandler(event) {
            const newBlog = {
                user: this.user,
                post: this.post,
                tag: this.tag
            };

            //Trimitem la backend obiectul noi
            myaxios.post("/blog", newBlog).then((response) => {
                this.blogList = response.data.blogs
            })

            console.log(newBlog)
        },
        fetchBlogPosts() {
            myaxios.get("/blog")
                .then((response) => {
                    console.log(response.data.blogs)
                    this.blogList = response.data.blogs
                    // Assuming the response data is an array of blog posts

                })
                .catch((error) => {
                    console.error("Failed to fetch blog posts:", error);
                });
        },
        likeHandler(event) {
        console.log(event.itemId)
        myaxios.post(`/blog/${event.itemId}/like`).then((response) => {
            console.log(response.data.blogs)
            this.blogList = response.data.blogs
        })
        },
        dislikeHandler(event) {
            console.log(event.itemId)
            myaxios.post(`/blog/${event.itemId}/dislike`).then((response) => {
            console.log(response.data.blogs)
            this.blogList = response.data.blogs
        })
        }
    },
    mounted() {
        this.fetchBlogPosts();
    },
}


</script>

<style scoped>
h1 {
    color:rgb(116, 52, 20)
}
body {
    background-color: burlywood
}
</style>