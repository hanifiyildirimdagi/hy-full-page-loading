<template>
    <div class="hy-full-page-loading" :class="{'start-loading':loading,'stop-loading':!loading}">
        <ul>
            <li v-for="(item, index) in pieces" :key="index">
                <span :style="pieceStyle(item,index)" :class="{'image':image != undefined}"></span>
            </li>
        </ul>
    </div>
</template>
<script>
const randomColor = (notIn = []) => {
    if(notIn == undefined || notIn == null || notIn.length == 0)
        return Math.floor(Math.random()*16777215).toString(16);
    let value = Math.floor(Math.random()*16777215).toString(16);
    while(notIn.filter(x=> x == value).length > 0){
        value = Math.floor(Math.random()*16777215).toString(16);
    }
    return value;
}
const defaultPieces = () => {
    let result = [];
    for(let i = 0; i<11; i++){
        result.push({
            color : '#'+(randomColor(['ffffff'])),
        });
    }
    return result;
}
export {defaultPieces,randomColor};
export default{
    name : 'hy-full-page-loading',
    props : {
        pieces : {
            type : Array || Number,
            reqired : false,
            default : () => defaultPieces()
        },
        loading : {
            type : Boolean,
            reqired : true
        },
        delay : {
            type : Number,
            reqired : false,
            default : 0.1
        },
        duration : {
            type : Number,
            reqired : false,
            default : 0.3
        },
        timingFunction : {
            type : String,
            reqired : false,
            default : 'linear'
        },
        image : {
            type : String,
            reqired : false,
            default : undefined
        }
    },
    computed : {
        pieceStyle : function(){
            return (piece,index) => {
                let style = {
                    'transition-property':'width, height',
                    'transition-delay':(this.delay * (index + 1)) + 's',
                    'transition-duration' : this.duration+'s',
                    'transition-timing-function':this.timingFunction
                };

                if(piece.color !== undefined && this.image === undefined)
                    style['background-color'] = piece.color;
                else
                    style['background-image'] = `url(${this.image})`;
                    let piecePosition = 100 / (typeof(this.pieces) == 'object' ? this.pieces.length : this.pieces);
                    style['background-position'] = `center -${(index * piecePosition)}vh`
                return style;
            }
        }
    }
}

</script>
<style scoped>
.hy-full-page-loading {
    display: flex;
    padding: 0;
    margin: 0;
    position: fixed;
    width: 100%;
    height: 100%;
    background: #fff;
    top: 0;
    left: 0;
}

.hy-full-page-loading>ul {
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
}

.hy-full-page-loading>ul>li {
    flex-grow: 1;
    position: relative;
    display: flex;
}
.hy-full-page-loading>ul>li>span{
    position: absolute;
    width: 0;
    top: 0;
    left: 0;
    height: 100%;
    transition: .3s linear;
}
.hy-full-page-loading>ul>li>span.image{
    background-repeat: no-repeat;
    background-size: cover;
}
.hy-full-page-loading.start-loading>ul>li>span{
    width: 100%;
}
.hy-full-page-loading.stop-loading>ul>li>span{
    width: 0%;
    right: 0;
    left:initial;
}
</style>