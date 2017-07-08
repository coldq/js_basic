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
```
