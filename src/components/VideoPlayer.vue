<template>
    <div>
        <!--
      play:播放已开始事件
      pause：播放已暂停
      ended：视频停止播放
      -->

        <div class="video-container">
            <video
                preload="metadata"
                autoplay
                class="video-main"
                ref="videoPlayer"
                :src="videoUrl"
                @play="onPlay"
                @pause="onPause"
                @ended="onEnded"
            ></video>
            <div class="video-button">
                <a-avatar
                    :size="50"
                    src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png"
                />
                <a-icon :style="{ fontSize: '48px' }" type="heart" />
                <a-icon
                    @click="showDrawer"
                    :style="{ fontSize: '48px' }"
                    type="message"
                />
                <a-icon :style="{ fontSize: '48px' }" type="star" />
            </div>
            <a-drawer
                title="评论"
                placement="right"
                :width="400"
                :closable="false"
                :visible="visible"
                :get-container="false"
                :wrap-style="{ position: 'absolute' }"
                @close="onClose"
            >
                <video-comment></video-comment>
            </a-drawer>
            <div>
                <input
                    class="video-progress"
                    type="range"
                    v-model="currentTime"
                    :max="duration"
                    @input="seekTo"
                />
            </div>
            <div class="video-controls">
                <a-icon
                    class="video-play-button"
                    :style="{ fontSize: '24px' }"
                    @click="togglePlay"
                    v-if="isPlaying"
                    type="pause"
                />
                <a-icon
                    class="video-play-button"
                    :style="{ fontSize: '24px' }"
                    @click="togglePlay"
                    v-else
                    type="caret-left"
                />
                <span class="video-time">
                    {{ formattedTime }} / {{ formattedDuration }}</span
                >
                <a-popover
                    class="video-volume"
                    :overlayStyle="{ width: '20px' }"
                >
                    <template slot="content">
                        <p
                            style="
                                width: 8px;
                                margin: 1px -12px;
                                font-size: 12px;
                                text-align: center;
                            "
                        >
                            {{ volume }}
                        </p>
                        <p
                            style="
                                width: 8px;
                                height: 100px;
                                margin: 0 -10px;
                                padding: 0;
                            "
                        >
                            <a-slider
                                style="margin: 1px 1px; padding: 0"
                                vertical
                                v-model="volume"
                                :max="100"
                                :min="0"
                                @afterChange="adjustVolume"
                            />
                        </p>
                    </template>
                    <sound-icon
                        :style="{ fontSize: '24px' }"
                        :volume="volume"
                    />
                </a-popover>

                <a-popover class="video-speed">
                    <template slot="content">
                        <p
                            :class="{ 'red-text': speed === 0.75 }"
                            @click="changePlaybackRate(0.75)"
                        >
                            0.75x
                        </p>
                        <p
                            :class="{ 'red-text': speed === 1.0 }"
                            @click="changePlaybackRate(1.0)"
                        >
                            1.0x
                        </p>
                        <p
                            :class="{ 'red-text': speed === 1.25 }"
                            @click="changePlaybackRate(1.25)"
                        >
                            1.25x
                        </p>
                        <p
                            :class="{ 'red-text': speed === 1.5 }"
                            @click="changePlaybackRate(1.5)"
                        >
                            1.5x
                        </p>
                        <p
                            :class="{ 'red-text': speed === 2.0 }"
                            @click="changePlaybackRate(2.0)"
                        >
                            2.0x
                        </p>
                    </template>
                    <span>{{ speed }}x</span>
                </a-popover>
            </div>
        </div>
    </div>
</template>

<script>
import SoundIcon from '@/components/SoundIcon'
import VideoComment from '@/components/VideoComment'
export default {
    name: 'VideoPlayer',
    props: {
        videoUrl: String
    },
    components: {
        SoundIcon,
        VideoComment
    },
    data() {
        return {
            isPlaying: false, //视频是否播放
            currentTime: 0, //当前播放时间点
            duration: 0, //视频时长
            videoElement: null, //视频元素
            volume: 0, //音量初始为1
            speed: 1, //视频倍速
            visible: false
        }
    },
    mounted() {
        this.videoElement = this.$refs.videoPlayer
        //当视频播放时间发生变化时,获取当前视频播放时间点
        this.videoElement.addEventListener('timeupdate', this.updateTime)
        //获取视频的时长
        this.videoElement.addEventListener('loadedmetadata', this.setDuration)
        this.videoElement.volume = 0
    },
    computed: {
        //格式化视频时间为00:00
        formattedTime() {
            const minutes = Math.floor(this.currentTime / 60)
            const seconds = Math.floor(this.currentTime % 60)
            return `${String(minutes).padStart(2, '0')}:${String(
                seconds
            ).padStart(2, '0')}`
        },
        formattedDuration() {
            const minutes = Math.floor(this.duration / 60)
            const seconds = Math.floor(this.duration % 60)
            return `${String(minutes).padStart(2, '0')}:${String(
                seconds
            ).padStart(2, '0')}`
        }
    },
    methods: {
        //点击播放按钮时触发的事件
        togglePlay() {
            if (this.isPlaying) {
                this.videoElement.pause()
            } else {
                this.videoElement.play()
            }
        },
        onPlay() {
            this.isPlaying = true
        },
        onPause() {
            this.isPlaying = false
        },
        onEnded() {
            this.isPlaying = false
        },
        updateTime() {
            this.currentTime = this.videoElement.currentTime
        },
        setDuration() {
            this.duration = this.videoElement.duration
        },
        //滑动视频进度条触发的事件
        seekTo() {
            this.videoElement.currentTime = this.currentTime
        },
        adjustVolume() {
            // 调整音量的事件处理器
            this.videoElement.volume = this.volume / 100
        },
        changePlaybackRate(rate) {
            this.videoElement.playbackRate = rate
            this.speed = rate
        },
        showDrawer() {
            this.visible = true
        },
        onClose() {
            this.visible = false
        }
    },
    beforeDestroy() {
        this.videoElement.removeEventListener('timeupdate', this.updateTime)
        this.videoElement.removeEventListener(
            'loadedmetadata',
            this.setDuration
        )
    }
}
</script>
    
<style>
.video-controls {
    display: flex;
    align-items: center;
    margin-top: 10px;
}
.video-play-button {
    flex-basis: 50px;
}
.video-time {
    flex-basis: 120px;
}
.video-volume {
    flex-basis: 30px;
}
.video-speed {
    flex-basis: 30px;
    font-size: 16px;
}
.code-box-demo .ant-slider {
    margin-bottom: 16px;
}
.red-text {
    color: red;
}

.video-button {
    float: right;
    height: 300px; /* 设置高度为 100% */
    display: flex; /* 使用 Flexbox 布局 */
    flex-direction: column; /* 垂直排列子元素 */
    justify-content: space-around; /* 垂直居中 */
    margin-top: 200px;
    margin-right: 40px;
}
.video-container {
    border: 2px solid #333; /* 边框样式和颜色 */
    border-radius: 10px; /* 圆角半径，可以根据需要调整 */
    overflow: hidden;
    position: relative;
}

.video-main {
    height: 80vh;
}
[type='range'] {
    -webkit-appearance: none;
    appearance: none;
    margin: 0;
    outline: 0;
    background-color: transparent;
    width: 100%;
}
[type='range']::-webkit-slider-runnable-track {
    height: 4px;
    background: #eee;
}
[type='range' i]::-webkit-slider-container {
    height: 20px;
    overflow: hidden;
}
[type='range']::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: #4e4c4c;
    border: 1px solid transparent;
    margin-top: -8px;
    border-image: linear-gradient(#4e4c4c, #4e4c4c) 0 fill / 8 20 8 0 / 0px 0px
        0 2000px;
}
</style>