<script>

  export default {
    data() {
      return {
        elapsed: 0,
        duration: 15*1000,
      }
    },
    created() {
      let lastTime = performance.now()
      const update = () => {
        const time = performance.now()
        this.elapsed += Math.min(time - lastTime, this.duration - this.elapsed)
        lastTime = time
        this.handle = requestAnimationFrame(update)
      }
      update()
    },
    unmounted() {
      cancelAnimationFrame(this.handle)
    }
  }

</script>

<template>

  <div class="elapsed">
    <p>Elapsed Time: </p><br />
    <progress :value="elapsed/duration"></progress>
    <p>{{(elapsed/1000).toFixed(2)}} s</p>
  </div>

  <br />

  <div class="duration">
    <p>Duration Time: </p><br />
    <input type="range" v-model="duration" min="1" max="30000"> <br />
    <p>{{(duration/1000).toFixed(2)}} s</p>
  </div>

  <br />

  <button @click="elapsed = 0">Reset</button>
  
</template>

<style>

  .elapsed, .duration {
    line-height: 0px;
  }

</style>
