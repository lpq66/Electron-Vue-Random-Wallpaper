<template>
    <div class="bg">
        <b-form-input class="query-input" v-model.number="query" :value="query" type="text" placeholder="Type in a word:"></b-form-input>
        <img class="imgr" :src="`https://wallhaven.now.sh/random?keyword=${query}`" alt="" @click="sab" crossOrigin="Anonymous">
        <button @click="sab">Set as background
        </button>
    </div>
</template>

<script>
  import Wallhaven from 'wallhaven-api'
  import download from 'image-downloader'
  import os from 'os'
  import path from 'path'
  import fs from 'fs'
  export default {
    data () {
      return {
        query: 'sao',
        width: window.screen.width,
        height: window.screen.height
      }
    },

    mounted () {
    },

    methods: {
      animebg () {
        const api = new Wallhaven()

        api.search('sao')
        .then(result => {
          console.log(result)
        })
      },
      testr (event) {
          let img = event.target.src
          console.log(img)
      },
      setasbg (event) { 
            let picturePath = path.join(os.homedir(), '/WWalpaper', 'background23.png')

            let imgSrc = document.querySelector('.imgr').src

            console.log(imgSrc)

            const options = {
               url: imgSrc, dest: picturePath      
            }
            download.image(options)
            .then(({ filename, image }) => {
                console.log('File saved to', filename)
            }).catch((err) => {
                throw err
            })
      },
      convertToBase64 (img) {
        console.log('converted')
        let canvas = document.createElement('canvas')
        canvas.width = img.width
        canvas.height = img.height
        let context = canvas.getContext('2d')
        context.drawImage(img, 0, 0, img.height, img.width)
        let dataURL = canvas.toDataURL('image/jpg')
        return dataURL.replace(/^data:image\/(png|jpg)base64,/, '')
        console.log(dataURL)
      },
      sab (event) {
        let base64Image = this.convertToBase64(event.target)
        let picturePath = path.join(os.homedir(), '/WWalpaper', 'background.png')
        let newImg = new Image()
        newImg.src = base64Image
        console.log(base64Image)
        fs.writeFile(picturePath, base64Image, 'base64', err => {
          if (err) {
            console.log(err)
          } else {
            console.log('completed!')
          }
        })
      }
    },

    computed: {}
  }
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
.bg {
  width: 100%;
  height: 100%;
  background: burlywood;
  position: relative;
}

.query-input {
  background: rgba(0, 0, 0, 0.45);
  color: #fff;
  margin: 0;
  padding: 0;
  padding-left: 1rem;
  border: 0;
  outline: 0;
  border-radius: 0;
  font-size: 2rem;
  position: absolute;
  width: 100%;
  padding: 1rem;
}
button {
  position: absolute;
  bottom: 0;
  border: 0;
  outline: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.45);
  color: #fff;
  padding: 2rem;
  font-size: 2rem;
}
</style>
