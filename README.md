# 前端试题

> 请clone代码并，完成下面三个题目，完成后提交Pull request


1. 请用递归的方式遍历树形数据结构中的每一个节点 

    ```
    const options = [
       {
           "value": "zhejiang",
           "label": "Zhejiang",
           "children": [
               {
                   "value": "hangzhou",
                   "label": "Hangzhou",
                   "children": [
                       {
                           "value": "xihu",
                           "label": "West Lake"
                       }
                   ]
               }
           ]
       },
       {
           "value": "jiangsu",
           "label": "Jiangsu",
           "children": [
               {
                   "value": "nanjing",
                   "label": "Nanjing",
                   "children": [
                       {
                           "value": "zhonghuamen",
                           "label": "Zhong Hua Men"
                       }
                   ]
               }
           ]
       }
    ]
    ```

    function diffOption(opt){
        for(let i =0;i<opt.length;i++){
            if(opt[i].children){
                console.log(opt[i])
                diffOption(opt[i].children)
            }else{
                console.log(opt[i])
            }
        }
    }

    diffOption(options)

2. 实现一个简单的画板，要求画板可以支持鼠标滑动绘制简单的线，canvas
<canvas id="canvasBg" width="1800" height="900"></canvas>
<script>
    let isDown = false;
    let points = [];
    let beginPoint = null;
    let canvas=document.getElementById('canvasBg')
    const ctx = canvas.getContext('2d');
    ctx.strokeStyle = 'red';
    ctx.lineWidth = 1;
    ctx.lineJoin = 'round';
    ctx.lineCap = 'round';

    canvas.addEventListener('mousedown', down, false);
    canvas.addEventListener('mousemove', move, false);
    canvas.addEventListener('mouseup', up, false);
    canvas.addEventListener('mouseout', up, false);

   function down(evt) {
       isDown = true;
       const { x, y } = getPos(evt);
       points.push({x, y});
       beginPoint = {x, y};
   }

   function move(evt) {
       if (!isDown) return;

       const { x, y } = getPos(evt);
       points.push({x, y});

       if (points.length > 3) {
           const lastTwoPoints = points.slice(-2);
           const controlPoint = lastTwoPoints[0];
           const endPoint = {
               x: (lastTwoPoints[0].x + lastTwoPoints[1].x) / 2,
               y: (lastTwoPoints[0].y + lastTwoPoints[1].y) / 2,
           }
           drawLine(beginPoint, controlPoint, endPoint);
           beginPoint = endPoint;
       }
   }

   function up(evt) {
       if (!isDown) return;
       const { x, y } = getPos(evt);
       points.push({x, y});
       if (points.length > 3) {
           const lastTwoPoints = points.slice(-2);
           const controlPoint = lastTwoPoints[0];
           const endPoint = lastTwoPoints[1];
           drawLine(beginPoint, controlPoint, endPoint);
       }
       beginPoint = null;
       isDown = false;
       points = [];
   }

   function getPos(evt) {
       return {
           x: evt.clientX,
           y: evt.clientY
       }
   }

   function drawLine(beginPoint, controlPoint, endPoint) {
       ctx.beginPath()
       ctx.moveTo(beginPoint.x, beginPoint.y);
       ctx.quadraticCurveTo(controlPoint.x, controlPoint.y, endPoint.x, endPoint.y);
       ctx.stroke();
   }
</script>
3. 设计Autocomplete组件（又叫搜索组件、自动补全组件）时，需要考虑什么问题
 1>一个页面有多个autocomplete组件，是否需要传入使用这个组件的字段;
 2>需要保存的数据是校验通过后的数据，也就是输入完成blur之后再操作保存的逻辑;
 3>校验规则，加上正则匹配；
 4>补齐内容是否需要保存等。
