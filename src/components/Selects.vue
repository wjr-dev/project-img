<template>
  <el-form v-if="_form" ref="form" :model="_form">
    <el-form-item v-for="item in selectConfig" :key="item.model">
      <el-select
        style="width: 100%"
        v-model="_form[item.model]"
        :placeholder="`请选择${item.model}`"
        @change="$emit('handle-change')"
      >
        <el-option
          v-for="option in item.options"
          :key="option"
          :label="option"
          :value="option"
        ></el-option>
      </el-select>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  name: "Selects",
  model: {
    prop: "form",
    event: "change",
  },
  props: {
    form: {
      type: Object,
      default() {
        return null;
      },
    },
    selectConfig:[]
  },
  computed: {
    _form: {
      get() {
        return this.form;
      },
      set(newVal) {
        this.$emit("change", newVal);
      },
    },
  },
  data() {
    return {
      
    };
  },
};
</script>

<style>
</style>