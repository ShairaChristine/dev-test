<template>
    <div>
        <h3>
        A simple CRUD app
        </h3>


        <div v-if="loading">Loading...</div>
        <div v-else>
            <input
                type="text"
                v-model="msg"
                placeholder="Input new topic"
            />
            <button class="button" @click="addData">Create</button>
            
            <Item 
                v-for="(item, index) in paginatedData"
                :key="index">

                <template #heading>
                    <div v-if="item.isEditing">
                        <input v-model="item.name" />
                        <button @click="saveEdit(item)">Save</button>
                    </div>
                    <div class="justify" v-else>
                        <span>{{ item.name }}</span>
                        <div>
                            <button @click="editItem(item)">Edit</button>
                            <button class="delete" @click="deleteItem(index)">Delete</button>
                        </div>
                    </div>
                </template>

                <template #comments>
                    <ul>
                        <li v-for="(comment, commentIndex) in item.comments" :key="commentIndex">
                        <div v-if="comment.isEditing">
                            <input v-model="comment.comment" />
                            <button @click="saveCommentEdit(item, comment)">Save</button>
                        </div>
                        <div class="justify" v-else>
                            <div>
                                <p>{{ comment.comment }}</p>
                                <span>by: {{ comment.by }} &nbsp; date: {{ formatDate(comment.date) }}</span>
                            </div>
                            <div class="comments-btn">
                                <button @click="editComment(comment)">Edit</button>
                                <button class="delete" @click="deleteComment(item, commentIndex)">Delete</button>
                            </div>
                        </div>
                        </li>
                    </ul>
                    <span v-if="!item.comments.length"><em>No comments</em></span>
                    <div class="justify">
                        <input v-model="item.newComment" placeholder="Add a comment" />
                        <button @click="addComment(item)">Add Comment</button>
                    </div>
                </template>
            </Item>
            
            <div class="pagination">
                <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
                <span>Page {{ currentPage }} of {{ totalPages }}</span>
                <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
            </div>
            
        </div>
    </div>
</template>

<script>
import List from './List.vue';
import Item from './Item.vue';
import axios from 'axios';

export default {
    components: {
        List,
        Item
    },
    data() {
      return {
        exampleData: [],
        msg: '',
        loading: true,
        currentPage: 1,
        itemsPerPage: 20
      };
    },
    computed: {
        totalPages() {
            return Math.ceil(this.exampleData.length / this.itemsPerPage);
        },
        paginatedData() {
            const start = (this.currentPage - 1) * this.itemsPerPage;
            const end = start + this.itemsPerPage;
            return this.exampleData.slice(start, end);
        }
    },
    mounted() {
        axios.get('https://atillc.blob.core.windows.net/data-collector/icode/test-data/topics.json')
        .then(response => {
            this.exampleData = response.data.topics; 
            this.loading = false;
        })
        .catch(error => {
            console.error('Error fetching data:', error);
            this.loading = false;
        });
    },
    methods: {
        addData() {
            let id = this.exampleData.length + 1; // Correctly increment the length

            const next = { id, name: this.msg, newComment: '', comments: [] };
            this.exampleData.unshift(next);
            console.log(117,'Wrapper.vue ===   ===', next);
        },
        formatDate(dateString) {
            const options = { year: 'numeric', month: 'long', day: 'numeric' };
            const date = new Date(dateString);
            return date.toLocaleDateString('en-US', options);
        },
        editItem(item) {
            item.isEditing = true;
        },
        saveEdit(item) {
            item.isEditing = false;
        },
        deleteItem(index) {
            this.exampleData.splice(index, 1);
        },
        editComment(comment) {
            comment.isEditing = true;
        },
        saveCommentEdit(item, comment) {
            comment.isEditing = false;
        },
        deleteComment(item, commentIndex) {
            item.comments.splice(commentIndex, 1);
        },
        addComment(item) {
            if (item.newComment.trim()) {
                item.comments.push({
                    comment: item.newComment,
                    by: 'Anonymous',
                    date: new Date().toISOString(),
                    isEditing: false
                });
                item.newComment = '';
            }
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
        },
        prevPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
        }
        },

}
</script>

<style>

</style>