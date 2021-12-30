<template>
  <div>
    <a-menu
        style="width: 100px"
    >
        <a-menu-item key="7">
          Option 7
        </a-menu-item>
        <a-menu-item key="8">
          Option 8
        </a-menu-item>
        <a-menu-item key="9">
          Option 9
        </a-menu-item>
        <a-menu-item key="10">
          Option 10
        </a-menu-item>
    </a-menu>
  </div>
</template>

<script>
export default {
  methods: {
    handleClick(e) {
      console.log('handleClick', e);
    }
  }
}
</script>