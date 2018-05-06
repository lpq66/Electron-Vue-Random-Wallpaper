<template>
  <div>
    <b-modal id="bgDone" v-model="CompleteBg" hide-footer size="sm" hide-header hide-header-close centered>
      <p>Wallpaper applied!</p>
      <b-btn class="mt-3" variant="outline-danger" block @click="CompleteBg = !CompleteBg">Close</b-btn>
    </b-modal>
    <p>Your screen size is {{size}}</p>
    <div class="mr-3"></div>
    <b-form-input v-model.number="numOfImg" :value="numOfImg" type="text" placeholder="How many pics to display:"></b-form-input>
    <div class="mr-3"></div>
    <b-alert show variant="dark" dismissible>Click on image to set it as a wallpaper</b-alert>
    <b-row>
      <b-col v-for="n in numOfImg" :key="n.id">
        <img :src="`https://source.unsplash.com/random/${size}?sig=${n}`" class="image" @click="setAsBackground" crossorigin>
      </b-col>
    </b-row>
    <div class="mr-3"></div>
    <b-row>
      <b-col>
        <img :src="`https://wallhaven.now.sh/random?keyword=sao`" alt="">
      </b-col>
    </b-row>
    <div class="mr-3"></div>
    <b-form-input v-model.number="numOfAnimeImg" :value="numOfAnimeImg" type="text" placeholder="How many pics to display:"></b-form-input>
    <b-row>
      <b-col v-for="n in numOfAnimeImg" :key="n.id">
        <img :src="`https://wallhaven.now.sh/random?keyword=sao`" class="image" crossorigin>
      </b-col>
    </b-row>
  </div>
</template>

<script>
  import os from 'os'
  import wallpaper from 'wallpaper'
  import fs from 'fs'
  import path from 'path'
  export default {
    data () {
      return {
        numOfImg: 2,
        numOfAnimeImg: 2,
        width: window.screen.width,
        height: window.screen.height,
        CompleteBg: false,
        wallpapers: [],
        errors: []
      }
    },

    mounted () {
    },

    methods: {
      
      convertToBase64 (img) {
        console.log('converted')
        let canvas = document.createElement('canvas')
        canvas.width = this.width
        canvas.height = this.height
        let context = canvas.getContext('2d')
        context.drawImage(img, 0, 0)
        let dataURL = canvas.toDataURL('image/jpg')
        return dataURL.replace(/^data:image\/(png|jpg)base64,/, '')
      },
      setAsBackground (event) {
        let base64Image = this.convertToBase64(event.target)
        let picturePath = path.join(os.homedir(), '/WWalpaper', 'background.png')
        let newImg = new Image()
        newImg.src = base64Image
        picturePath = path.normalize(picturePath)
        fs.writeFile(picturePath, base64Image, 'base64', err => {
          if (err) {
            console.log(err)
          } else {
            wallpaper.set(picturePath, { scale: 'stretch' }).then(() => {
              console.log(path.resolve(picturePath))
              this.CompleteBg = true
            })
          }
        })
      }
    },
    computed: {
      size () {
        return this.width + 'x' + this.height
      }
    }
  }
</script>

<style scoped>
.mr-3 {
  margin-bottom: 2rem;
}

img {
  cursor: pointer;
}

#bgDone p {
  color: #333;
  text-align: center;
}
</style>
