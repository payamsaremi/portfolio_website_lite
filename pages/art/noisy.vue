<template>
  <div id="container">
        <canvas @mousemove="getMousePos"></canvas>
  </div>
</template>

<script>
import { makeNoise3D } from "open-simplex-noise";
export default {
    data(){
        return {
            canvas:'',
            ctx:'',
            mouseX:0,
            mouseY:0,
            width:0,
            height:0,
            simplex:null,
            noise:0,
            now:0,
            shadowValue:0.8,
            stepValue:0.01,
            minValue:0.01,
            maxValue:1
        }
    },
    methods:{
        sliderValue(value){
            this.shadowValue = value
        },
        reset(){
            this.width = this.canvas.width = window.innerWidth
            this.height = this.canvas.height = window.innerHeight
            console.log('window reset')
        },
        getMousePos(event){
            try{
                this.mouseX = event.pageX
                this.mouseY = event.pageY
            }catch{
                console.log('havnt recieved the mouse event')
            }
        },
        setup(){
            this.canvas = document.querySelector("canvas");
            this.ctx = this.canvas.getContext("2d");
            this.width = this.canvas.width = window.innerWidth
            this.height = this.canvas.height = window.innerHeight
            this.simplex = new makeNoise3D();
            window.addEventListener("resize", this.reset);
        },
        draw(timestamp){    
            this.now = timestamp       
            this.drawCircle()
            this.ctx.fillStyle = `rgba(0,0,0,${this.shadowValue})`;
            this.ctx.fillRect(0, 0, this.width, this.height);
            this.ctx.strokeStyle = "white";
            window.requestAnimationFrame(this.draw)
            for(let i = 20; i < Math.min(this.width,this.height) / 2 - 40; i += 10) {
                this.drawCircle(i);
            }
            console.log(this.shadowValue)
        },
        drawCircle(r){
            this.ctx.beginPath()
            let point,x,y
            let deltaAngle = Math.PI * 2 / 400
            for(let angle = 0; angle < Math.PI * 2; angle+=deltaAngle){
                point = this.calcPoint(angle,r)
                x = point[0]
                y = point[1]
                this.ctx.lineTo(x, y)
                // this.ctx.arc(x, y, 20, 0, Math.PI*2, true)
            }
            this.ctx.closePath()
            this.ctx.stroke()

        },
        calcPoint(angle,r){
            let noiseFactor = this.mouseX / this.width * 50
            let zoom = this.mouseY / this.height * 200
            let x = Math.cos(angle) * r + this.width / 2
            let y = Math.sin(angle) * r + this.height / 2
            let n = (this.simplex(x/zoom, y/zoom, this.now/2000)) * noiseFactor
            x = Math.cos(angle) * (r + n) + this.width / 2;
            y = Math.sin(angle) * (r + n) + this.height / 2;
            return [x, y];
        },
        random(min,max){
            let num = Math.floor(Math.random() * (max - min + 1) + min)
            return num
        }
    },
    mounted(){
        this.setup()
        this.draw()
    }
}
</script>

<style>
#container{
    position: relative;
}
#container canvas, .slider {
    position:absolute;
}
/* canvas{
    margin: 1rem;
} */
</style>