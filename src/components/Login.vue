<template>
    <div class="login_box">
        <router-view></router-view>
    </div>
</template>

<script>
export default {
}
</script>

<!--scoped 表示当前组件下有效-->
<style lang="less" scoped>
    .login_box{
        height: 500px;
        background-color: red;
    }
</style>
