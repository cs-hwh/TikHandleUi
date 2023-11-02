<template>
  <div>
    <video-player videoUrl="test.mp4"></video-player>
  </div>
</template>

<script>
import VideoPlayer from '@/components/VideoPlayer.vue'

export default {
    name: 'IndexView',
    components: {
       VideoPlayer 
    }
}
</script>

<style>

</style>