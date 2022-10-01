-//css



*{

    margin: 0px;

    padding: 0px;

    box-sizing: border-box;

}

body{

     font-family: "Roboto", sans-serif;

     height: 100vh;

     display:flex;

     justify-content: center;

}

.app{

    position: fixed;

    width:100%;

    height:100%;

    max-width: 600px;

    background: blue;

    border-left: 1px solid  #eee;

    border-right: 1px solid #eee;

}

.app > .screen{

    display: none;



}

 .app > .screen.active{

    display: block;

    width: 100%;

    height: 100%;

 }

 .screen .form{

    position:absolute;

    top: 50%;

    left:50%;

    transform: translate(-50%,-50%);

    width:80%;

    max-width:400px;

 }

 .screen .form-input{

    width:100%;

    margin:2px 0px;

 }

 .screen h2{

 margin-bottom: 20px;

 font-size: 30px;

 color: #111;

 border-bottom:4px solid #555;

 padding:  5px 0px;

 display: inline-block;



 }



 .screen .form-input lable{

 display: block;

   margin-bottom: 5px;

 }

.screen .form-input input{

     width: 100%;

     padding: 10px;

     border: 1px solid #555;

     font-size: 16px;

}



.screen .form-input button{

     padding: 10px 20px;

     background: #111;

      color: #eee;

       font-size: 16px;

       cursor: pointer;

        outline: none;

         border: none;



}

 

 .chat-screen .header{

     background: #111 ;

     height:50px;

      display: flex;

      justify-content:  space-between;

      align-items: center;

       padding: 0px 20px;

 }

  .chat-screen .header .logo{

     font-size: 10px;

      color: #eee;

      font-weight: 600;

  }



   .chat-screen .header button{

     padding: 5px 10px;

      border: 1px solid #eee;

      background: transparent;

      color: #eee;

      font-size: 15px;

      cursor: pointer;

      outline: none;

    }



    .chat-screen .message{

         width: 100%;

         height:  calc(100%  - 100px);

          background:  blue;

           overflow: auto;

    }

     .chat-screen .message .message{

         display: flex;

          padding: 10px;

     }



     .chat-screen .message .message > div{

         max-width:  80%;

          background:  #fff;

           box-shadow:  0px 0px 20px 5px  rgba( 0, 0,0,0, 0.5);

           padding: 10px;

     }

     .chat-screen .message .message.my-message{

        justify-content: flex-end;

         

     }

     .chat-screen .message .message.other-message{



         justify-content: flex-start;

     }



     .chat-screen .message .message .name{

 font-size: 13px;

  color: #555;

   margin-bottom: 5px ;

     }



      .chat-screen .message .message .text{

         word-wrap: break-word;



      }



      .chat-screen .message .update {

         text-align: center;

          padding: 10px;

           font-style: italic;

      }

      .chat-screen .typebox{

        width:100%;

        height: 50px;

        display: flex;



      }

      .chat-screen .typebox input{

        

 flex:1;

  height: 50px;

   font-size:  18px;



      }



       .chat-screen .typebox button{

         width: 80px;

         height: 100%;

          background:  #111;

           color:#eee;

            font-size: 16px;

             outline: none;

              border:none;

               cursor: pointer;

       }



//  databases



// Get the text field that we're going to track

let field = document.getElementById("field");



// See if we have an autosave value

// (this will only happen if the page is accidentally refreshed)

if (sessionStorage.getItem("autosave")) {

  // Restore the contents of the text field

  field.value = sessionStorage.getItem("autosave");

}



// Listen for changes in the text field

field.addEventListener("change", () => {

  // And save the results into the session storage object

  sessionStorage.setItem("autosave", field.value);

});







javascript

const  express=require("express");

 const path= express("path");

 const app = express();

 const server= require("http").createServer(app);



 const io= require("socket.io")(server);

 app.use(express.static(path.join(__dirname+"/pubic")));



 io.o("connection", function( socket){

socket.on("newuser", function(username){

    socket.broadcast.emit("update", username+"joined the conversation");

});

socket.on("exituser",function(username){

    socket.broadcast.emit("update", username+"left the conversation")

});

socket.on("chat", function(message){

    socket.broadcast.emit("chat", message);

});

 });

 server.listen(5000);





code.js



(function()

{

    const app=document.querySelector(".app");

    const socket= io();

     let uname;

     app.querySelector(".join-screen  #join-user").addEventListener("click", function(){

        let username=app.querySelector(".join-screen username").value;

        if(username.length==0){

            return;

        }



        socket.emit("newuser", username);

        uname = username;

        app.querySelector(".join-screen").classList.remove("active");

        app.querySelector(".chat-screen").classList.add("active");



     });

     app.querySelector(".chat-screen  #send-message").addEventListener("click",function(){

 let message = app.querySelector("chat-screen #message-input").value;

 if(message.length==0){

    return;

 }

 renderMessage("my",{

    username:uname,

    text:message





 });

 socket.emit("chat",{

    username: uname,

    text: message

 });

 app.querySelector(".chat-screen #message-input").value="";

     });

app.querySelector(".chat-screen #exit-chat").addEventListener("click", function(){

    socket.emit("exituser",uname);

    window.location.href=window.location.href;

});



socket.on("update" ,function(update){

    renderMessage("update", update);



});

socket.on("update" ,function(update){

    renderMessage("other", message);



});

     function renderMessage(type,message){

        let messageContained= app.querySelector(".chat-screen .message");

        if(type=="my"){

            let el= document.createElement("div");

            el.setAttribute("class", "message my-message");

            el.innerHTML=`

            <div>

            <div class="name">you</div>

            <div class="text">${message.text}</div>

            </div>

            `;

messageContained.appendChild(el);

        }

            else if(type=="other"){

                let el= document.createElement("div");

                el.setAttribute("class", "message other-message");

                el.innerHTML=`

                <div>

                <div class="name">you</div>

                <div class="text">${message.username}</div>

                <div class="text">${message.text}</div>

                </div>

                `;

    messageContained.appendChild(el);

            }

             else if(type=="update"){

                let el= document.createElement("div");

                el.setAttribute("class", "update");

                el.innerHTML=message;

    messageContained.appendChild(el);

             }

           

             

             // scroll chat  to end

        messageContainer.scrollTop = messageContainer.scrollheight - messageContainer.clientHeight;



     }

})();







//index.html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Document</title>

    <link rel="stylesheet" href="style.css">     

</head>

<body>

    <div class="app">

        <div class="screen join-screen active">

            <div class=" form">

                <h2>join chatroom</h2>

                <div class="form-input">

                    <lable>username</lable>

                    <input type="text" id="username">

                </div>

                <div class="form-input"></div>

                <button id="join-user">join</button>

            </div>

        </div>

<div class="screen chat-screen">

    <div class="header">

        <div class="logo">chatroom</div>

        <button id="exit-chat">E xit</button>

    </div>

<div class="message">

    <!--dummy message-->

 <div class="message my-message">

    <div>

 <div class="name">you</div>

 <div class="text">hi</div>

</div>

</div>

<div class="update">

     abc is join the conversion room

</div>

<div class="message other-message">

    <div>

       <div class="name">abs</div>

 <div class="text">hi</div>

    </div>

</div>

<div class="typebox">

    <input type="text" id="message-input">

<button id="send-message">send</button>

</div>

</div>

</div>

    </div>



    <script type="text/javascript" scr="socket.io/socket.io.js"></script>

    <script type="text/javascript" src="code.js"></script>

</body>

</html>
