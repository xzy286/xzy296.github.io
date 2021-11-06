<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!-- 网页名字 -->
    <title>自动聊天机器人</title>
    <!-- css样式文件 -->
    <link rel="stylesheet" href="style.css">
    <!-- jquery -->
    <script src="jquery-3.4.1.min.js"></script>
    <!-- java script处理逻辑，建立与服务器的websocket连接 -->
    <script>
    var socket;
    if ("WebSocket" in window) {
      var ws = new WebSocket("ws://127.0.0.1:8080");
      socket = ws;
      ws.onopen = function() {
        console.log('连接成功');
      };
      // 获取到服务端返回的信息
      ws.onmessage = function(evt) {
        var received_msg = evt.data;
        var txt="<div class=\"message-item message-item--left\">  <img class=\"avatar\" src=\"./img/girl.png\" alt=\"头像\"><div class=\"message-bubble\">"+ received_msg +"</div> </div>";  
        $(".message-list").append(txt);
        document.getElementById("mes").value="";
      };
      ws.onclose = function() {
        alert("断开了连接");
      };
    } else {
      alert("浏览器不支持WebSocket");
    }

    // 信息发送到服务端
    function sendMeg(){
      var message=document.getElementById("mes").value;
      var txt="<div class=\"message-item message-item--right\">  <img class=\"avatar\" src=\"./img/boy.png\" alt=\"头像\"><div class=\"message-bubble\">"+ message +"</div> </div>";  
        $(".message-list").append(txt);
      socket.send(message);
    }
  </script>
</head>
<body>
    <section class="chat-page show-selector">
        <header>
            <div class="nav-back">
                <i class="icon icon-back"></i>
                <span>聊天</span>
            </div>
            <h1>python - AI机器人</h1>
            <div class="nav-person">
                <i class="icon icon-person"></i>
            </div>
        </header>
        <main>
            <div class="message-list">
                <div class="message-item message-item--left">
                    <img class="avatar" src="./img/girl.png" alt="头像">
                    <div class="message-bubble">你住的 巷子里 我租了一间公寓</div>
                </div>
                <div class="message-item message-item--left">
                    <img class="avatar" src="./img/girl.png" alt="头像">
                    <div class="message-bubble">为了想与你不期而遇</div>
                </div>
                <div class="message-item message-item--right">
                    <img class="avatar" src="./img/boy.png" alt="头像">
                    <div class="message-bubble">高中三年 我为什么 为什么不好好读书 没考上跟你一样的大学</div>
                </div>
            </div>
        </main>
        <footer>
            <input type="text" class="text-input" id="mes">
            <button class="send-button" onclick="sendMeg(); ">发送</button>
        </footer>
    </section>
</body>
</html>
