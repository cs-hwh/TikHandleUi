<template>
    <div>
        <a-row>
            <a-col :span="3">
                <side-bar></side-bar>
            </a-col>
            <a-col :span="21">
                <div class="index-video-container">
                    <transition name="fade" mode="out-in">
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
                'test3.mp4'
                // 添加更多视频URL
            ],
            currentIndex: 0
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
            } else if (event.key === 'ArrowDown') {
                this.showNextVideo()
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
    transform: translateY(0); /* 初始时平移元素，将视频移出屏幕 */
}

.fade-enter-active,
.fade-leave-active {
    transition: transform 0.5s;
}
.fade-enter,
.fade-leave-to {
    transform: translateY(100%);
}
</style>