title: Use riot.js to create time count down
date: 2015-01-30 16:37:00
tags:
---
最近在研究[Riot.js](https://muut.com/riotjs)，同时有个项目用到时间倒计时，于是
写了一个tag。
```html countdown.tag
<count-down>
    <div>{time}</div>

    var oneDay = 86400000;
    var oneHour = 3600000;
    var oneMinute = 60000;
    var oneSecond = 1000;

    

    this.now = new Date().getTime();

    tick(){
        var future = new Date(this.root.getAttribute('data-future').replace("-","/").replace("-","/")).getTime();
        future = isNaN(future) ? 0 : future;
        
        
        var diff = future - this.now;

        var days = Math.floor(diff/oneDay);
        var hours = Math.floor((diff - days*oneDay)/oneHour);
        var minutes = Math.floor((diff - days*oneDay - hours*oneHour)/oneMinute);
        var seconds = Math.floor((diff - days*oneDay - hours*oneHour - minutes*oneMinute)/oneSecond);

        minutes = minutes<10?"0"+minutes:minutes;
        seconds = seconds<10?"0"+seconds:seconds;

        var timeString
        if(diff<=oneSecond){
        days = 0;hours = 0;minutes = "00";seconds = "00";
        clearInterval(timer)
        }

        timeString = days+"日 "+hours+":"+minutes+":"+seconds;

        this.update({time: timeString});
        this.now += oneSecond;
    }

    var timer = setInterval(this.tick,1000);
    this.on('unmount', function() {
    clearInterval(timer)
    });

</count-down>
```
###编译
需要先把countdown.tag编译成countdown.js
``` bash
$ roit countdown.tag js/countdown.js
```


###引入文件
```html
<script src="../riot.js"></script>
<script src="js/countdown.js"></script>
```

###Useage
```html
<count-down data-future="2015-02-02 0:10:00">
</count-down>
```