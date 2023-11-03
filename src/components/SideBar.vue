<template>
    <div class="sidebar">
        <div class="user">
            <a-avatar :size="64" icon="user" />
            <p>不才无华</p>
        </div>
        <div class="menu userInfo">
            <div
                v-for="item in userInfoItems"
                :key="item.id"
                @click="selectMenuItem(item.id)"
                @mouseover="hoverMenuItem(1, item.id)"
                @mouseleave="unhoverMenuItem(1)"
                :class="{ active: item.active, hovered: item.hovered }"
            >
                {{ item.label }}
            </div>
        </div>

        <div class="menu">
            <div
                v-for="item in menuItems"
                :key="item.id"
                @click="selectMenuItem(item.id)"
                @mouseover="hoverMenuItem(2, item.id)"
                @mouseleave="unhoverMenuItem(2)"
                :class="{ active: item.active, hovered: item.hovered }"
            >
                {{ item.label }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SideBar',
    data() {
        return {
            userInfoItems: [
                { id: 21, label: '关注', active: false, hovered: false },
                { id: 22, label: '喜欢', active: false, hovered: false },
                { id: 23, label: '收藏', active: false, hovered: false }
            ],
            menuItems: [
                { id: 1, label: '推荐', active: false, hovered: false },
                { id: 2, label: '知识', active: false, hovered: false },
                { id: 3, label: '游戏', active: false, hovered: false },
                { id: 4, label: '体育', active: false, hovered: false },
                { id: 5, label: '美食', active: false, hovered: false },
                { id: 6, label: '二次元', active: false, hovered: false },
                { id: 7, label: '旅游', active: false, hovered: false }
            ]
        }
    },
    methods: {
        selectMenuItem(itemId) {
            this.userInfoItems.forEach((item) => {
                item.active = item.id === itemId
            })

            this.menuItems.forEach((item) => {
                item.active = item.id === itemId
            })
        },
        hoverMenuItem(type, itemId) {
            if (type === 1) {
                this.userInfoItems.forEach((item) => {
                    item.hovered = item.id === itemId
                })
            } else {
                this.menuItems.forEach((item) => {
                    item.hovered = item.id === itemId
                })
            }
        },
        unhoverMenuItem(type) {
            if (type === 1) {
                this.userInfoItems.forEach((item) => {
                    item.hovered = false
                })
            } else {
                this.menuItems.forEach((item) => {
                    item.hovered = false
                })
            }
        }
    }
}
</script>

<style>
.sidebar {
    width: 100%;
    height: 100%;
    background-color: rgb(42, 22, 38);
    color: white;
    border-radius: 10px; /* 设置圆角半径 */
}
.userInfo {
    margin: 20px auto;
}
.menu {
    display: flex;
    flex-direction: column;
}

.menu div {
    margin: 0 auto;
    width: 80%;
    padding: 10px;
    cursor: pointer;
    transition: background-color 0.2s, border 0.2s;
}
.menu div.active {
    background-color: rgb(50, 40, 53);
    border-radius: 10px; /* 设置圆角半径 */
    backdrop-filter: blur(70px); /* 设置模糊效果 */
    font-weight: bold; /* 设置文本加粗 */
}

.menu div.hovered {
    background-color: rgb(50, 40, 53);
    border-radius: 10px; /* 设置圆角半径 */
    backdrop-filter: blur(70px); /* 设置模糊效果 */
    font-weight: bold; /* 设置文本加粗 */
}
</style>