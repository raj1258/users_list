<template>
    <div 
        class="virtual-container" 
        :style="`height:${height}px`"
        @scroll.prevent="onScrollContainer"
    >
        <div
        class="virtual"
        :style="`height:${virtualListHeight}px`"
        >
            <div 
                v-for="(user) in visibleUsers"   
                :key="user.id"
                class="item"
                :style="`top:${(user.index) * itemHeight}px`"
            >
                    <div class="personal ">
                        <div class="bold">{{user.FirstNameLastName}}</div>
                        <div>{{user.JobTitle}} <span class="contact">@ {{user.Company}}</span></div>
                    </div>
                    <div class="contact">
                        <div class="bold">Contact details</div>
                        <div>Email: {{user.Email}}</div>
                        <div>Phone: {{user.Phone}}</div>
                    </div>
            </div>
        </div>
    </div>
</template>

<script>
import { ITEM_HEIGHT, CACHE_ITEMS } from './Configs';

// component renders only those users which are supposed to be visibe when scroll is at a specific position.
export default {
    name: 'UsersList',
    props: ['users', 'height'],
    data() {
        return {
            scrollTop: 0,
            itemHeight: ITEM_HEIGHT,
        }
    },
    computed: {
        visibleUsers() {
            return this.users
                .map((user, index) => ({...user, index}))
                .filter((user, index) => index >= this.startIndex && index <= this.endIndex);
        },
        virtualListHeight() {
            return ITEM_HEIGHT* this.users.length;
        },
        startIndex() {
            return Math.floor(this.scrollTop/ITEM_HEIGHT) - CACHE_ITEMS;
        },
        endIndex() {
            return Math.floor((this.scrollTop + this.height) / ITEM_HEIGHT) + CACHE_ITEMS;
        },
    },
    methods: {
        onScrollContainer(e) {
            this.scrollTop = e.currentTarget.scrollTop;
        },
    }
}
</script>
<style scoped>
.virtual-container {
    overflow-y: scroll;
    position: relative;
    margin-top: 20px;
}

.virtual {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.item {
    position: absolute;
    padding: 8px 0;
    display: flex;
    align-items: center;
    border-bottom: 1px #eaeaea solid;
}

.personal {
    margin-right: 150px;
}

.personal>div:first-child {
    padding-bottom: 8px
}

.contact {
    color: #808080;
}

.bold {
    font-weight: 600;
}
</style>
