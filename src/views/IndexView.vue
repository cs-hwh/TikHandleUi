<template>
    <div>
        <a-row>
            <a-col :span="3">
                <side-bar></side-bar>
            </a-col>
            <a-col :span="21">
                <div class="index-video-container">
                    <transition :name="schema" mode="out-in">
                        <video-player
                            :key="currentVideoUrl"
                            class="video"
                            ref="videoPlayer"
                            :videoUrl="currentVideoUrl"
                        ></video-player>
                    </transition>
                </div>
            </a-col>
        </a-row>
    </div>
</template>

<script>
import VideoPlayer from '@/components/VideoPlayer.vue'

import SideBar from '@/components/SideBar.vue'
export default {
    name: 'IndexView',
    components: {
        VideoPlayer,
        SideBar
    },
    data() {
        return {
            videos: [
                'test1.mp4',
                'test2.mp4',
                'test3.mp4',
                'http://s3hnoo5fc.hn-bkt.clouddn.com/test.mp4?e=1699063377&token=Sb8CA99fxd1AOa4TPhy7GY9T00VpHG8McMGmQe_z:lunBxizHA16Q3PSQv0KDHOJffjY='
                // 添加更多视频URL
            ],
            currentIndex: 0,
            schema: 'down'
        }
    },
    computed: {
        currentVideoUrl() {
            return this.videos[this.currentIndex]
        }
    },
    mounted() {
        // 监听键盘上下键事件
        document.addEventListener('keydown', this.handleKeyDown)
    },
    beforeDestroy() {
        // 在组件销毁前移除事件监听
        document.removeEventListener('keydown', this.handleKeyDown)
    },
    methods: {
        handleKeyDown(event) {
            if (event.key === 'ArrowUp') {
                this.showPreviousVideo()
                this.schema = 'up'
            } else if (event.key === 'ArrowDown') {
                this.showNextVideo()
                this.schema = 'down'
            }
        },
        showPreviousVideo() {
            if (this.currentIndex > 0) {
                this.currentIndex--
            }
        },
        showNextVideo() {
            if (this.currentIndex < this.videos.length - 1) {
                this.currentIndex++
            }
        }
    }
}
</script>

<style>
.index-video-container {
    position: relative;
    overflow: hidden;
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
}

.video {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
.up-leave-active,
.up-enter-active,
.down-enter-active,
.down-leave-active {
    transition: transform 0.5s;
}
.up-leave-to {
    transform: translateY(100%);
}
.down-leave-to {
    transform: translateY(-100%);
}
</style>