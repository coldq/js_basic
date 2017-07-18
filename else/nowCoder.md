# 牛客网node.js输入获取

```
var readline = require('readline');  
     const rl = readline.createInterface({  
             input: process.stdin,  
             output: process.stdout  
     });  
 rl.on('line', function(line){  
    doSomething... 
 });
 
 
process.stdin.resume();
process.stdin.setEncoding('ascii');

var input = "";
var input_array = "";

process.stdin.on('data', function (data) {
    input += data;
});

process.stdin.on('end', function () {
    input_array = input.split("\n");
    var nLine = 0;

    while(nLine < input_array.length){
        var line = input_array[nLine++].trim();
        if(line === ''){
            continue;
        }
        var input_arrays = line.split(' ');
        var a = +input_arrays[0];
        var b = +input_arrays[1];
        console.log(a+b);
    }
});
```
