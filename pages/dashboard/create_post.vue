<template>
  <div>
    <div>
    <span>Upload to Cloudinary</span>
    <input
      type="file"
      accept=".jpeg,.jpg,.png,image/jpeg,image/png"
      aria-label="upload image button"
      @change="selectFile"
    />
  </div>
  </div>
</template>

<script>
export default {
  data(){
    return {

    }
  },
  methods:{
    async selectFile(e){
      const file = e.target.files[0];

      if(!file) return;

      const readData = (f) => new Promise((res) => {
        const reader = new FileReader();
        reader.onloadend = () => res(reader.result);
        reader.readAsDataURL(f);
      });

      //read data
      const data = await readData(file);

      const instance = this.$cloudinary.upload(data, {
        folder:'payam',
        uploadPreset: 'hjkbxn58'
      })

      console.log('uploaded')
    }
  }
}
</script>

<style>

</style>