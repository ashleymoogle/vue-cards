<template>
  <div class="wrapper">
    <span @click="setMode" class="material-icons dark-mode">
    nights_stay
    </span>
    <div class="buttons">
      <div class="button" @click="sort('shuffle')">Shuffle</div>
      <div style="margin-bottom: 10px;">
        Sort by:
      </div>
      <div class="button" @click="sort('suits')">Suits</div>
      <div class="button" @click="sort('value')">Value</div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "SideBar",
    data () {
      return {
        mode: 'light'
      }
    },
    methods: {
      sort (by) {
        this.$emit('order', by)
      },
      setMode () {
        this.mode = this.mode === 'light' ? 'dark' : 'light'
        this.$emit('mode', this.mode)
      }
    }
  }
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 40px;
  min-width: 300px;
  .dark-mode {
    position: absolute;
    top: 30px;
    left: 30px;
    cursor: pointer;
    &:hover {
      color: lightgray;
    }
  }
  .buttons {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    .button {
      padding: 5px 10px;
      border-radius: 20px;
      max-width: 100px;
      text-align: center;
      margin-bottom: 10px;
      cursor: pointer;
      color: #fff;
      &:nth-child(1) {
        background: rgb(0,36,18);
        background: linear-gradient(90deg, rgba(0,36,18,0.39959733893557425) 0%, rgba(9,121,70,0.7777485994397759) 47%, rgba(0,255,194,1) 100%);
      }
      &:nth-child(3) {
        background: rgb(0,20,36);
        background: linear-gradient(90deg, rgba(0,20,36,0.4472163865546218) 0%, rgba(12,9,121,0.6320903361344538) 47%, rgba(0,131,255,1) 100%);
      }
      &:nth-child(4) {
        background: rgb(0,20,36);
        background: linear-gradient(90deg, rgba(0,20,36,0.4472163865546218) 0%, rgba(0,0,0,0.5172443977591037) 47%, rgba(0,0,0,0.4416141456582633) 100%);
      }
    }
  }
}
</style>