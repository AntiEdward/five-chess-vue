<template>
    <div class="container">
        
        <canvas id="canvas" class="square" width="530" height="530"></canvas>
        <div class="sidebar">
            <el-button @click="init()">重新开始</el-button>
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
            winsResult: {
                horizontalResult: [],   //横线
                verticalResult: [],     //竖线

            },

            player: true    //true为黑子，false为白子
        }
    },
    mounted(){
        const _this = this
        //绘制棋盘
        this.canvasDom = document.getElementById('canvas')
        
        this.canvasDom.onclick = function(e){
            _this.drawStep(e)
        }

        this.drawSquare()
        //生成空白落子数组
        this.getSquareResult()
        //生成获胜数组
        this.getWinsResult()
    },
    methods: {
        //初始化
        init(){
            // window.location.reload()
            let ctx = this.canvasDom.getContext("2d");

            //清空画布
            ctx.clearRect(0, 0, 530, 530);
            //清空数组
            this.squareResult = []
            //初始化棋手
            this.player = true
            this.drawSquare()
            this.getSquareResult()

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
            //棋盘是15x15点阵，14x14网格

            //竖线
            for(let i = 0; i < 15; i++){
                ctx.moveTo(20 + i * 35, 20);
                ctx.lineTo(20 + i * 35, 510);
                // ctx.strokeStyle = '#f8f8f8'
                ctx.stroke();
            }

            //横线
            for(let i = 0; i < 15; i++){
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
                this.squareResult[x][y] = this.player ? 1 : -1
            }
            
            //转换为像素坐标
            x = x * 35  + 20
            y = y * 35  + 20
            // console.log(x, y)
           
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
            this.checkWins()
        },
        //生成空白落子数组
        getSquareResult(){
            const _this = this
            // 14 x 14
            for(let i = 0; i < 15; i++){
                let item = []
                for(let j = 0; j < 15; j++){
                    item.push(null)
                }
                _this.squareResult.push(item)
            }
        },
        //生成获胜数组
        getWinsResult(){
            let _this = this

            //横线
            for(let i = 0; i < 15; i++){
                let item = []
                for(let j = 0; j < 11; j++){
                    let item_sub = []
                    for(let k = 0; k < 5; k++){
                        let arr = [i, j + k]
                        item_sub.push(arr)
                    }
                    item.push(item_sub)
                }
                _this.winsResult.verticalResult.push(item)
            }
            //竖线
            for(let i = 0; i < 15; i++){
                let item = []
                for(let j = 0; j < 11; j++){
                    let item_sub = []
                    for(let k = 0; k < 5; k++){
                        let arr = [j + k, i]
                        item_sub.push(arr)
                    }
                    item.push(item_sub)
                }
                _this.winsResult.horizontalResult.push(item)
            }
            //正斜线
            
            //反斜线

            
            
        },
        //校验胜利
        checkWins(){
            let current = this.squareResult
            // check(this.winsResult.verticalResult)
            check(this.winsResult.horizontalResult)
            function check(arr){
                let wins = arr
                // console.log(wins)
                for(let i in wins){
                    let item = wins[i]
                    for(let j in item){
                        let item_sub = item[j]
                        // console.log(item_sub)
                        let check = []
                        for(let k in item_sub){
                            // console.log(item_sub[k])
                            let x = item_sub[k][0]
                            let y = item_sub[k][1]
                            check.push(current[x][y])
                        }
                        // console.log(check)
                        if(add(check) === 5){
                            //5个1
                            console.log('黑棋')
                            return
                        }else if(add(check) === -5){
                            //5个-1
                            console.log('白棋')
                            return
                        }
                    }
                }
            }
            
            function add(arr){
                let result = 0
                for(let item of arr){
                    result += item
                }
                return result
            }
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
    .sidebar{
        margin-top: 20px;
    }
</style>
