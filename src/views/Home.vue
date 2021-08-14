<template>
  <el-container>
    <el-header>Vit & EfficientNet model comparison</el-header>
    <el-main>
      <el-row :gutter="24">
        <!-- 筛选条件 -->
        <el-col :span="6" :offset="6">
          <div class="grid-content">
            <Selects
              :form="form"
              :selectConfig="selectConfig"
              @handle-change="handleSelectChange"
            />
          </div>
        </el-col>
        <!-- img0 -->
        <el-col :span="6">
          <div class="grid-content">
            <MyImage :url="img0" />
          </div>
        </el-col>
        <!-- imgs -->
        <el-col
          :span="6"
          :offset="i % 2 === 0 ? 6 : 0"
          v-for="(item, i) in imgs"
          :key="i"
        >
          <div class="grid-content">
            <MyImage :url="item.url" />
          </div>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
// @ is an alias to /src
import Selects from "../components/Selects.vue";
import MyImage from "../components/MyImage.vue";
export default {
  name: "Home",
  components: {
    Selects,
    MyImage,
  },
  computed: {
    selectConfig() {
      let InputSizeOptions = [];
      if (this.form.Model === "VIT") {
        InputSizeOptions = ["224x224"];
      } else if (this.form.Model === "EfficientNetB0") {
        InputSizeOptions = ["32x32", "64x64", "224x224"];
      }
      const config = [
        {
          model: "Model",
          options: ["VIT", "EfficientNetB0"],
        },
        {
          model: "DataSet",
          options: ["CIFAR10", "CIFAR100"],
        },
        {
          model: "InputSize",
          options: InputSizeOptions,
        },
        {
          model: "Preprocessing",
          options: ["Normalize", "None"],
        },
      ];

      return config;
    },
  },
  data() {
    return {
      form: {
        Model: "",
        DataSet: "",
        InputSize: "",
        Preprocessing: "",
      },
      img0: "",
      imgs: [
        {
          url: "",
          name: "",
        },
        {
          url: "",
          name: "",
        },
        {
          url: "",
          name: "",
        },
        {
          url: "",
          name: "",
        },
      ],
    };
  },
  methods: {
    handleSelectChange() {
      let { Model, DataSet, InputSize, Preprocessing } = this.form;
      if (Model && DataSet) {
        Model = Model === "EfficientNetB0" ? "EfficientB0" : Model;
        this.img0 = require(`@/assets/img/${DataSet}_${Model}/metric.png`);
        if (InputSize && Preprocessing) {
          const normalize = Preprocessing === "Normalize" ? "True" : "False";
          const resize = InputSize.split("x");
          this.imgs.forEach((item,i) => {
            item.url = require(`@/assets/img/${DataSet}_${Model}/resize_[${resize[0]}, ${resize[1]}]_normalize_${normalize}_${i}.png`);
            item.name = `resize_[${resize[0]}, ${resize[1]}]_normalize_${normalize}_${i}.png`
          })
        }
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.el-header{
    background-color: #c1c1c1;
    color: #333;
    text-align: center;
    line-height: 60px;
    font-size: 28px;
    box-shadow: 0 0px 20px rgba(0, 0, 0, 0.6);
  }
.grid-content {
  margin-bottom: 10px;
  height: 250px;
}
</style>
