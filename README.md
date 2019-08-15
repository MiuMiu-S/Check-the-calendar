# Check-the-calendar
#### 写一个签到日历
最近项目上写了一个签到页面,其中有一个功能设计的是点击签到，可以查看签到日历，稍微麻烦点的是连续签到的日期间有横线连接，现在总结记录一下：

![效果](https://upload-images.jianshu.io/upload_images/3888312-24aec7e94943a8c3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 需求
1. 只显示当前月份的签到日历（其实基于目前的代码扩展部分可以实现全年的日历）
2. 后台提供已经签到天数




#### 具体代码

##### 第1步：HTML布局
最后的展示效果的html代码
```
        <ul>
			<li>日</li>
			<li>一</li>
			<li>二</li>
			<li>三</li>
			<li>四</li>
			<li>五</li>
			<li>六</li>
			<li></li>
			<li></li>
			<li></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="succe">28<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="succe">28<i></i></p></li>
			<li><p class="failed">1<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="failed">2<i></i></p></li>
			<li><p class="succe">28<i></i></p></li>
			<li><p class="succe">28<i></i></p></li>
			<li><p class="succe">28<i></i></p></li>
			<li><p class="succe">28<i></i></p></li>
			<li><p>29<i></i></p></li>
			<li><p>30<i></i></p></li>
			<li><p>31<i></i></p></li>
		</ul>
```
第2步：Css布局
此处css处理了当连续签到时两个日期之间需要有黄色线条连接，日历四周不能有额外的黄线线条，下面css代码就是针对此要求的处理。
```
.ck-bg .cale-warp ul li:nth-of-type(7n) p.succe i{
    background:transparent;
}
.ck-bg .cale-warp ul li:last-of-type p.succe i{
    background:transparent;
}
.ck-bg .cale-warp ul li:nth-of-type(1),
.ck-bg .cale-warp ul li:nth-of-type(2),
.ck-bg .cale-warp ul li:nth-of-type(3),
.ck-bg .cale-warp ul li:nth-of-type(4),
.ck-bg .cale-warp ul li:nth-of-type(5),
.ck-bg .cale-warp ul li:nth-of-type(6),
.ck-bg .cale-warp ul li:nth-of-type(7){
    background:rgba(248,248,248,1);
    font-size: 0.24rem;
    color:rgba(127,127,127,1);
}
```
其他代码
```
.ck-bg .cale-warp ul{margin:0 auto;width:5.95rem;height:5.1rem;overflow:hidden}
.ck-bg .cale-warp ul li{display:flex;justify-content:center;align-items:center;float:left;position:relative;width:0.85rem;height:0.85rem;line-height:0.85rem;text-align:center;font-size:0.28rem;color:#000}
.ck-bg .cale-warp ul li p{width:0.56rem;height:0.56rem;line-height:0.56rem;text-align:center;border-radius:100%}
.ck-bg .cale-warp ul li p i{position:absolute;top:0.34rem;left:-.145rem;background:rgb(255,255,255);width:0.42rem;height:0.18rem;right:unset}
.ck-bg .cale-warp ul li p.succe{background:rgba(248,190,69,1);color:#fff}
.ck-bg .cale-warp ul li p.succe i{position:absolute;top:0.365rem;right:-0.2rem;background:rgba(248,190,69,1);width:0.4rem;height:0.12rem;left:unset}
.ck-bg .cale-warp ul li p.failed:after{content:"！";color:red;position:absolute;top:-.05rem;right:0;font-weight:bold}
```
#### 第3步：代码
如果当前年份能被4整除但是不能被100整除或者能被400整除，即可确定为闰年，返回1，否则返回0
```
function isLeap(year) {
    return year % 4 == 0 ? (year % 100 != 0 ? 1 : (year % 400 == 0 ? 1 : 0)) : 0;
}
```
```
//获取当前时间
var nowYear = 2018;
var nowMonth = 7;
var nowDay = 31;
var nowweekDay = 2;
//从后台获取签到日期
var signDate = [0,1,2,3,4,5,6,7,8,9,,18,19,23,24,25,26,27,28,29,30,31];
```
获取每个月的天数
```
var days_per_month = new Array(31, 28 + isLeap(nowYear), 31, 30, 31, 30, 31, 31, 30, 31, 30, 31);
```
```
var dateHtml = "<li>日</li><li>一</li><li>二</li><li>三</li><li>四</li><li>五</li><li>六</li>";
```
计算当月1号的位置
```
var blank = nowweekDay - (nowDay % 7) + 1;
if(blank<0) blank = 7 + blank; 
for(var i = 0; i < blank; i++) {
	dateHtml += "<li></li>";
}
```
拼接
```
var dateHtmlp = "";
for(var j = 1; j <= days_per_month[nowMonth - 1]; j++) {
	for(var n = 1; n < signDate.length; n++) {
		if(j == signDate[n]) {
			dateHtmlp = "<p class='succe'>" + j + "<i></i></p>";
			break
		} else {
			if(j < signDate[signDate.length - 1]) {
				dateHtmlp = "<p class='failed'>" + j + "<i></i></p>";
			} else {
				dateHtmlp = "<p>" + j + "<i></i></p>";
			}
		}
	}
	dateHtml += "<li>" + dateHtmlp + "</li>";
}
```
