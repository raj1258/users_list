<template>
    <div class="container">
        <img class="logo" alt="Users book" src="https://notion-emojis.s3-us-west-2.amazonaws.com/v0/svg-twitter/1f4d2.svg">
        <h1> User Management</h1>
        <div class="search-box">
            <input 
                type="search"
                :value="searchText"
                placeholder="Search Users by name, job, company etc..."
                @input="onChangeText" 
            />
        </div>
        <div v-if="filteredUsers.length">
            <div class="numberUsers bold">{{filteredUsers.length}} Users</div>
            <user-list 
                :users="filteredUsers"
                :height="300" 
             />
        </div>
        <div v-else>
            Your search - <span class="bold">{{ this.searchText }}</span> - did not match any users.<br>
            Make sure you search is correct. 
        </div>
    </div>
</template>

<script>
import UserList from './UsersList.vue';
import { objects as users } from './users';

export default {
    name: 'Home',
    components: {
        UserList,
    },
    data() {
        return {
            searchText: '',
        }
    },
    computed: {
        filteredUsers() {
            return this.searchText.length ? users.filter(user => {
                let found = false;
                Object.values(user).forEach(value => {
                    if (value.toLowerCase().includes(this.searchText.toLowerCase())) {
                        found = true;
                        return;
                    }
                })
                return found;
            }) : users;
        },
    },
    methods: {
        onChangeText(e) {
            this.searchText = e.target.value;
        }
    }
}
</script>
<style scoped>
.container {
    margin: 16px;
}

.search-box {
    height: 28px;
    width: 280px;
    margin-left: auto;
}

.search-box>input {
    height: 28px;
    width: 280px;
}

.numberUsers {
    text-align: end;
    padding-top: 8px;
}

.bold {
    font-weight: 600;
}
</style>
