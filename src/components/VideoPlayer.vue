<template>
   <div>
        <div>
            <a-row>
                <a-col :span="6"></a-col>
                <a-col :span="12">
                        <!--
                            play:播放已开始事件
                            pause：播放已暂停
                            ended：视频停止播放
                        -->
                        <video class="video-main" ref="videoPlayer" :src="videoUrl" @play="onPlay" @pause="onPause" @ended="onEnded"></video>
                </a-col>
                <a-col :span="6"></a-col>
            </a-row>   
           
        
        <div class="controls">
        <button @click="togglePlay">{{ isPlaying ? 'Pause' : 'Play' }}</button>
        <span> {{ formattedTime }} / {{ formattedDuration }}</span>
        <input type="range" v-model="currentTime" :max="duration" @input="seekTo">

            <a-popover  :overlayStyle="{width :'20px'}">
                <template slot="content">
                    <p style="width:8px;margin:1px -12px;font-size:12px;text-align: center;">{{volume}}</p>
                    <p style="width:8px;height:100px;margin:0 -10px;padding:0;">
                        <a-slider style="margin:1px 1px;padding:0;" vertical v-model="volume" :max="100" :min="0"  @afterChange="adjustVolume"  />
                    </p>    
                </template>
                <sound-icon   :style="{ fontSize: '24px' }" :volume="volume"/>
            </a-popover>

            <a-popover  >
                <template slot="content" >
                    <p  :class="{ 'red-text': speed === 0.75 }" @click="changePlaybackRate(0.75)">0.75x</p>
                    <p  :class="{ 'red-text': speed === 1.0 }" @click="changePlaybackRate(1.0)">1.0x</p>
                    <p  :class="{ 'red-text': speed === 1.25 }" @click="changePlaybackRate(1.25)">1.25x</p>
                    <p  :class="{ 'red-text': speed === 1.5 }" @click="changePlaybackRate(1.5)">1.5x</p>
                    <p  :class="{ 'red-text': speed === 2.0 }" @click="changePlaybackRate(2.0)">2.0x</p>
                </template>
                <span  >{{speed}}x</span>
            </a-popover>
        </div>
    </div>
  </div>
</template>

<script>
import SoundIcon from '@/components/SoundIcon'
export default {
    name : 'VideoPlayer',
    props: {
        videoUrl: String,
    },
    components: {
        SoundIcon
    },
  data() {
    return {
      isPlaying: false,//视频是否播放
      currentTime: 0,//当前播放时间点
      duration: 0,//视频时长
      videoElement: null,//视频元素
      volume: 0,//音量初始为1
      speed: 1,//视频倍速
    };
  },
  mounted() {
    this.videoElement = this.$refs.videoPlayer;
    //当视频播放时间发生变化时,获取当前视频播放时间点
    this.videoElement.addEventListener('timeupdate', this.updateTime);
    //获取视频的时长
    this.videoElement.addEventListener('loadedmetadata', this.setDuration);
    this.videoElement.volume=0
  },
  computed:{
    //格式化视频时间为00:00
    formattedTime(){
      const minutes = Math.floor(this.currentTime / 60);
      const seconds = Math.floor(this.currentTime % 60);
      return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    },
    formattedDuration(){
        const minutes = Math.floor(this.duration / 60);
      const seconds = Math.floor(this.duration % 60);
      return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    }
  },
  methods: {
    //点击播放按钮时触发的事件
    togglePlay() {
      if (this.isPlaying) {
        this.videoElement.pause();
      } else {
        this.videoElement.play();
      }
    },
    onPlay() {
      this.isPlaying = true;
    },
    onPause() {
      this.isPlaying = false;
    },
    onEnded() {
      this.isPlaying = false;
    },
    updateTime() {
      this.currentTime = this.videoElement.currentTime;
    },
    setDuration() {
      this.duration = this.videoElement.duration;
    },
    //滑动视频进度条触发的事件
    seekTo() {
      this.videoElement.currentTime = this.currentTime;
    },
     adjustVolume() {
      // 调整音量的事件处理器
      this.videoElement.volume = this.volume/100;
    },
    changePlaybackRate(rate){
    this.videoElement.playbackRate = rate
    this.speed = rate
  },

    
  },
  beforeDestroy() {
    this.videoElement.removeEventListener('timeupdate', this.updateTime);
    this.videoElement.removeEventListener('loadedmetadata', this.setDuration);
  },
  
}
</script>
    
<style>
.controls {
  display: flex;
  align-items: center;
  margin-top: 10px;
}
.code-box-demo .ant-slider {
  margin-bottom: 16px;
}
.red-text {
  color: red; 
}
.video-main{
    height: 80vh;
}
</style>