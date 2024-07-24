<template>
    <div>
        <h1>Blogul GGIT FD</h1>

        <div v-if="blogList.length > 0">
            <BlogItem v-for="blog in blogList" :key="blog.id" :id="blog.id" :user="blog.user" :post="blog.post"
                :likes="blog.likes" :tag="blog.tag" @like="likeHandler($event)" @dislike="dislikeHandler($event)" />

        </div>
        <div v-else>
            <p>Nici un post nu a fost creat</p>
        </div>
        <div>

        </div>
        <input v-model="user" />
        <textarea v-model="post"></textarea>
        <select v-model="tag">
            <option value="general">General</option>
            <option value="flame">Flame</option>
            <option value="recomandation">Recomandation</option>

        </select>

        <button @click="submitFormHandler">Save</button>
    </div>
</template>

<script>
import BlogItem from '../components/BlogItem.vue';
import { myaxios } from '../axios'

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
            }

            //trimitem la backand
            myaxios.post("/blog", newBlog).then((data) => {
                this.blogList = data.data.blogs
            })
        },
        fetchBlogPosts() {
            myaxios.get("/blog").then(
                (data) => {
                    console.log(data.data.blogs)
                    this.blogList = data.data.blogs
                }
            )

        },
        likeHandler(event) {
            myaxios.post(`/blog/${event.itemId}/like`).then((data) =>{
                console.log(data.data.blogs)
                this.blogList = data.data.blogs
            })

        },
        dislikeHandler(event) {
            myaxios.post(`/blog/${event.itemId}/dislike`).then((data) =>{
                console.log(data.data.blogs)
                this.blogList = data.data.blogs
            })
        },
    },
    mounted() {
        this.fetchBlogPosts();
    }
}
</script>

<style scoped></style>