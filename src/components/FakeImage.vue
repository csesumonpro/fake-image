<template>
  <div>
    <el-container>
      <el-header>
        <el-row>
          <el-col :span="12">Fake Image Generator</el-col>
          <el-col :span="12">
            <div class="github">
              <a
                style="text-decoration:none;color:#fff"
                href="http://youtube.com/fullstackweb"
                target="_blank"
              >Youtube</a>
            </div>
          </el-col>
        </el-row>
      </el-header>
      <el-main>
        <el-row>
          <el-col :span="12" :offset="6">
            <el-form label-width="150px" size="mini" label-position="left" class="image-form">
              <el-form-item label="Width">
                <el-input-number @change="generateImage" :max="9999" v-model="image.width"></el-input-number>
              </el-form-item>
              <el-form-item label="Height">
                <el-input-number @change="generateImage" :max="9999" v-model="image.height"></el-input-number>
              </el-form-item>
              <el-form-item label="Text Color">
                <el-color-picker @change="generateImage" v-model="image.textcolor"></el-color-picker>
              </el-form-item>
              <el-form-item label="Background Color">
                <el-color-picker @change="generateImage" v-model="image.bgcolor"></el-color-picker>
              </el-form-item>
              <el-form-item label="Font Size">
                <el-slider
                  @change="generateImage"
                  :min="10"
                  :max="200"
                  v-model="image.textSize"
                  show-input
                ></el-slider>
              </el-form-item>
              <el-form-item label="Font Style">
                <el-radio-group @change="generateImage" v-model="image.textFontStyle">
                  <el-radio label="Normal"></el-radio>
                  <el-radio label="Bold"></el-radio>
                  <el-radio label="Italic"></el-radio>
                </el-radio-group>
              </el-form-item>
              <el-form-item label="Data URI">
                <el-input type="textarea" v-model="inputDataUrl" readonly="readonly"></el-input>
              </el-form-item>
            </el-form>
          </el-col>
        </el-row>
      </el-main>
    </el-container>
    <el-row>
      <div style="margin:0 auto">
        <input v-show="false" type="text" v-model="inputDataUrl" readonly />
        <img alt="Preview Image" id="imagePreview" v-show="false" />
      </div>
    </el-row>
    <a
      style="text-decoration:none"
      :href="inputDataUrl"
      v-if="downloadValidate"
      :download="`fake-image-${image.width}_${image_height}`"
    >
      <el-button type="primary" style="margin:20px auto;display:block">Download Image</el-button>
    </a>
  </div>
</template>
<script>
export default {
  name: "FakeImage",
  data() {
    return {
      inputDataUrl: "",
      image: {
        width: 600,
        height: 200,
        bgcolor: "#ddd",
        textcolor: "#000",
        text: "sumon",
        textSize: 20,
        textFontStyle: "normal"
      }
    };
  },
  computed: {
    downloadValidate() {
      if (this.image.height > 10 && this.image.width > 10) {
        return true;
      }
      return false;
    }
  },
  methods: {
    downloadImage() {
      console.log(this.inputDataUrl);
    },
    generateImage() {
      const canvasElement = this.createPlaceholderCanvas();
      const dataUrl = canvasElement.toDataURL();
      const imagePreview = document.getElementById("imagePreview");
      this.inputDataUrl = dataUrl;
      imagePreview.src = dataUrl;
      imagePreview.style.display = "block";
      imagePreview.style.maxWidth = `${this.image.width}px`;
    },
    createPlaceholderCanvas() {
      const element = document.createElement("canvas");
      const context = element.getContext("2d");
      element.width = this.image.width;
      element.height = this.image.height;

      // Fill in the background
      context.fillStyle = this.image.bgcolor;
      context.fillRect(0, 0, element.width, element.height);

      // Place the text
      context.font =
        this.image.textFontStyle + " " + this.image.textSize + "px sans-serif";
      context.fillStyle = this.image.textcolor;
      context.textAlign = "center";
      context.textBaseline = "middle";
      context.fillText(
        `${this.image.width}x${this.image.height}`,
        element.width / 2,
        element.height / 2
      );
      return element;
    }
  },
  mounted() {
    this.generateImage();
    // const params = new URL(location.href).searchParams.get('size')
    // console.log(params);
  }
};
</script>
<style >
.el-header {
  top: 0;
  background-color: #0c6be9;
  color: rgb(245, 241, 241);
  line-height: 25px;
  font-size: 25px;
  padding: 17px 10px;
  border: 2px solid rgb(63, 5, 5);
  border-radius: 50px 20px;
}
.github {
  text-align: right;
}
.image-form .el-form-item__label {
  font-size: 15px;
  font-weight: bold;
  color: #000000;
}
</style>