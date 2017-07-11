<template>
  <div class="md-avatar-content" :class="[themeClass]">
    <slot></slot>
  </div>
</template>

<style lang="scss" src="./mdAvatar.scss"></style>

<script>
  import theme from '../../core/components/mdTheme/mixin';

  export default {
    name: 'md-avatar',
    mixins: [theme]
  };
</script>
