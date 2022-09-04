- <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script>
    function addvalues()
    {
      var a= document.getElementById('t1').value;
      var b= document.getElementById('t2').value;
   document.getElementById("op").innerHTML="Result="+(a+b);

    }
  </script>
</head>
<body>
 <input id="t1"  type="text"placeholder="Text 1"/>
 <br></br>

 <input id=" t2" type="text" placeholder="Text 2"/>
<br></br>
<button onclick="addValues()" id="btnAdd">Add</button>
<h2 id="op">Result= ABCXYZ</h2>
</body>
</html>
