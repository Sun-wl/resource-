//常用方法


//设置字体自适应
window.addEventListener('resize', __resize, false);
__resize();
function __resize() {
  var currClientWidth = document.documentElement.clientWidth;
  //这里是设置屏幕的最大和最小值时候给一个默认值
  if (currClientWidth > 1200) currClientWidth = 1200;
  if (currClientWidth < 320) currClientWidth = 320;
  fontValue = (currClientWidth/1200*100).toFixed(2);
  // $('html').css('font-size',fontValue + 'px');
  document.documentElement.style.fontSize = fontValue + 'px';
};


//input(onkeyup方法)保留两位小数  >0
function fix2(obj){
    obj.value = obj.value.replace(/[^\d.]/g,""); //清除"数字"和"."以外的字符
    obj.value = obj.value.replace(/^\./g,""); //验证第一个字符是数字
    obj.value = obj.value.replace(/\.{2,}/g,"."); //只保留第一个, 清除多余的
    obj.value = obj.value.replace(".","$#$").replace(/\./g,"").replace("$#$",".");
    obj.value = obj.value.replace(/^(\-)*(\d+)\.(\d\d).*$/,'$1$2.$3'); //只能输入两个小数
}
//input(onkeyup方法)保留整数  >0
function fix0(obj){
    obj.value = obj.value.replace(/[^\d.]/g,""); //清除"数字"和"."以外的字符
    obj.value = obj.value.replace(/^\./g,""); //验证第一个字符是数字
    obj.value = obj.value.replace(/\.{2,}/g,"."); //只保留第一个, 清除多余的
    obj.value = obj.value.replace(".","$#$").replace(/\./g,"").replace("$#$",".");
    obj.value = obj.value.replace(/^(\-)*(\d+)\.*$/,'$1$2'); //只能输入整数
}

//排序（从小到大）
function compare(key){
    //返回比较器函数
    return function(a,b){
        if(a[key]<b[key]){
            return -1;
        }else if(a[key]>b[key]){
            return 1;
        }else{
            return 0;
        }
    }
}

//获取url参数（http://...?id=1&type=2）
function getUrlParam() {
    var obj = {}
    var params = ''
    if(window.location.search){
        params = window.location.search.substr(1).split('&')
        params.forEach(function(item){
            var a = item.split('=')
            obj[a[0]] = a[1]
        })
    }
    return obj
}

//获取url参数（http://.../1）
function urlParam() {
    var params = window.location.pathname.split('/')
    return params[params.length-1]
}


//上传图片前选择图片并预览图片
$('#uploaderInput').change(function (e){
    var src, url = window.URL || window.webkitURL || window.mozURL, file = $('#uploaderInput')[0].files[0];
    if(file.size>500000){
        alert("请上传小于500kb的图片");
        return
    }
    if(url){
        src = url.createObjectURL(file);
    }else{
        src = e.target.result;
    }

    $('#uploaderInput').siblings().css({'margin':'10px 0','width':'300px','height':'150px','background':'url('+src+') no-repeat','background-size': 'contain'});

})

//时间格式
 function parseTime(time, cFormat) {
   if (arguments.length === 0) {
     return null
   }
   const format = cFormat || '{y}-{m}-{d} {h}:{i}:{s}'
   let date
   if (typeof time === 'object') {
     date = time
   } else {
     if (('' + time).length === 10) time = parseInt(time) * 1000
     date = new Date(time)
   }
   const formatObj = {
     y: date.getFullYear(),
     m: date.getMonth() + 1,
     d: date.getDate(),
     h: date.getHours(),
     i: date.getMinutes(),
     s: date.getSeconds(),
     a: date.getDay()
   }
   const time_str = format.replace(/{(y|m|d|h|i|s|a)+}/g, (result, key) => {
     let value = formatObj[key]
     if (key === 'a') return ['一', '二', '三', '四', '五', '六', '日'][value - 1]
     if (result.length > 0 && value < 10) {
       value = '0' + value
     }
     return value || 0
   })
   return time_str
 }

function formatTime(time, option) {
   time = +time * 1000
   const d = new Date(time)
   const now = Date.now()

   const diff = (now - d) / 1000

   if (diff < 30) {
     return '刚刚'
   } else if (diff < 3600) { // less 1 hour
     return Math.ceil(diff / 60) + '分钟前'
   } else if (diff < 3600 * 24) {
     return Math.ceil(diff / 3600) + '小时前'
   } else if (diff < 3600 * 24 * 2) {
     return '1天前'
   }
   if (option) {
     return parseTime(time, option)
   } else {
     return d.getMonth() + 1 + '月' + d.getDate() + '日' + d.getHours() + '时' + d.getMinutes() + '分'
   }
 }
