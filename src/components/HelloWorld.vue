<template>
    <div class="container">
        
        <canvas id="canvas" class="square" width="530" height="530"></canvas>
        <div>
            <div @click="init()">重新开始</div>
        </div>
    </div>
   
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    },
    data(){
        return{
            canvasDom: null,
            squareResult: [],
            winsResult: [],

            player: true    //true为黑子，false为白子
        }
    },
    mounted(){
        const _this = this
        //绘制棋盘
        this.canvasDom = document.getElementById('canvas')
        this.drawSquare()
        this.canvasDom.onclick = function(e){
            _this.drawStep(e)
        }
        //生成空白落子数组
        this.getSquareResult()
        //生成获胜数组
        this.getWinsResult()
    },
    methods: {
        //初始化
        init(){
            this.squareResult = []
            this.getSquareResult()
            this.player = true
        },
        //画棋盘
        drawSquare(){
            // let canvasDom = document.getElementById('canvas')
            let ctx = this.canvasDom.getContext("2d");
            ctx.beginPath();

            ctx.moveTo(20, 20);
            ctx.lineTo(510, 20);
            ctx.stroke();

            ctx.moveTo(20, 20);
            ctx.lineTo(20, 510);
            ctx.stroke();

            ctx.moveTo(510, 20);
            ctx.lineTo(510, 510);
            ctx.stroke();

            ctx.moveTo(20, 510);
            ctx.lineTo(510, 510);
            ctx.stroke();

            this.drawCell()
        },
        //画网格
        drawCell(){
            let ctx = this.canvasDom.getContext("2d");
            ctx.beginPath()
            //棋盘是14x14

            //竖线
            for(let i = 0; i < 14; i++){
                ctx.moveTo(20 + i * 35, 20);
                ctx.lineTo(20 + i * 35, 510);
                // ctx.strokeStyle = '#f8f8f8'
                ctx.stroke();
            }

            //横线
            for(let i = 0; i < 14; i++){
                ctx.moveTo(20, 20 + i * 35);
                ctx.lineTo(510, 20 + i * 35);
                // ctx.strokeStyle = '#f8f8f8'
                ctx.stroke();
            }
        },
        //落子
        drawStep(e){
            // console.log(e)

            //定位坐标位于那个格子
            let x = (e.offsetX - 20) / 35
            let y = (e.offsetY - 20) / 35
            
            x = Math.round(x)
            y = Math.round(y)
            if(this.squareResult[x][y]){
                return
            }else{
                this.squareResult[x][y] = '1'
            }
            
            //转换为像素坐标
            x = x * 35  + 20
            y = y * 35  + 20
            console.log(x, y)

            
            let ctx = this.canvasDom.getContext("2d");
            ctx.beginPath();
            let grd = ctx.createRadialGradient(x, y, 14, x, y, 0);
            if(this.player){
                grd.addColorStop(0, '#0a0a0a')
                grd.addColorStop(1, '#636766')
            }else{
                grd.addColorStop(0, '#d1d1d1')
                grd.addColorStop(1, '#f9f9f9')
            }
            
            ctx.fillStyle = grd
            ctx.arc(x, y, 14, 0, 2 * Math.PI);
            // ctx.fillStyle = this.player ? 'black' : 'grey';
            ctx.fill();
            this.player = !this.player
        },
        //生成空白落子数组
        getSquareResult(){
            const _this = this
            // 14 x 14
            for(let i = 0; i < 14; i++){
                let item = []
                for(let j = 0; j < 14; j++){
                    item.push(null)
                }
                _this.squareResult.push(item)
            }
        },
        //生成获胜数组
        getWinsResult(){
            let _this = this
            
            // for(){

            // }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .square{
        /* width: 500px;
        height: 500px; */
        border: 1px solid #ddd;
    }
</style>
