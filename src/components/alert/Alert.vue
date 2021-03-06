<template>
  <transition name="fade">
    <div class="notification alert" :class="[typeClass, hasIcon]" v-show="isShow">
      <button v-if="closable" class="delete" @click="handleClose"></button>
      <div class="title" v-if="title">{{ title }}</div>
      <div class="wrap-icon" v-if="iconClass"><i :class="['fa', `fa-${iconClass}`, faSpin]"></i></div>
      <div class="notification-content"><slot></slot></div>
    </div>
  </transition>
</template>
<script>
export default {
  props: {
    type: {
      type: String,
      default: '',
    },
    title: {
      type: String,
    },
    closable: {
      type: Boolean,
      default: false,
    },
    onClose: {
      type: Function,
      default() {},
    },
    icon: {
      type: String,
    },
    animated: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      isShow: true,
    };
  },

  computed: {
    typeClass() {
      if (this.type === 'loading') return 'is-info';
      return this.type ? `is-${this.type}` : null;
    },
    hasIcon() {
      return this.iconClass ? 'has-icon' : null;
    },
    faSpin() {
      if (this.type === 'loading') return 'fa-spin';
      return this.animated ? 'fa-spin' : null;
    },
    iconClass() {
      if (this.icon) return this.icon;

      if (this.type === 'info') {
        return 'info-circle';
      } else if (this.type === 'success') {
        return 'check-circle';
      } else if (this.type === 'warning') {
        return 'exclamation-triangle';
      } else if (this.type === 'danger') {
        return 'times-circle';
      } else if (this.type === 'loading') {
        return 'spinner';
      }
      return this.icon;
    },
  },

  methods: {
    handleClose() {
      this.isShow = false;
      this.onClose();
      setTimeout(() => {
        this.$destroy();
        this.$el.remove();
      }, 100);
    },
  },
};
</script>
<style lang="scss">
.alert {
  position: relative;
  min-width: 240px;
  backface-visibility: hidden;
  transform: translate3d(0, 0, 0);
  pointer-events: all;
  &.has-icon{
    padding-left: 45px;
  }
  .title{
    margin-bottom: 5px;
    font-weight: 500;
    font-size: 16px;
  }
  .notification-content{
    margin-top: 0px;
  }
}
.wrap-icon{
  position: absolute;
  left: 10px;
  top: 50%;
  margin-top: -11px;
  width: 25px;
  height: 25px;
  i{
    font-size: 21px;
  }
}
</style>
