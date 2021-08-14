<template>
  <el-image style="width: 100%; height: 100%" :src="url" fit="contain">
    <div slot="error" class="image-slot">
      <i class="el-icon-picture-outline"></i>
    </div>
  </el-image>
</template>

<script>
export default {
  name: "my-image",
  props: {
    url: {
      type: [String, undefined],
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped>
::v-deep .image-slot {
  font-size: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: #f5f7fa;
  color: #909399;
}
</style>