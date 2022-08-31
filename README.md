<!DOCTYPE html>

<html lang="en">

<head>

  <meta charset="UTF-8">

  <meta http-equiv="X-UA-Compatible" content="IE=edge">

  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Document</title>

</head>

<body>

  

</body>

<script>







const posts=[

  {  title: 'Post One', body: 'this is post one'},

  { title: ' Post Two', body: ' this is post two'}

];

function getPosts(){

  setTimeout(()=>{

    let output='';

    posts.forEach((post, index)=>{

      output+=`<li>${post.title}</li>`;

    });

    document.body.innerHTML=output;

  }, 1000);



}





//  function createPost(post,callback){

//   setTimeout(() => {

//     posts.push(post);

//     callback();

//   },2000);

// }

//  getPosts();

// createPost({ title:'post three', body: 'this is post three'});



function createPost(post){

  return new Promise((resolve, reject)=>{

    setTimeout(()=>{

      posts.push(post);

      const error=false;

      if(!error){

        resolve();



      

      }else {

        reject ('error : Something went wrong');

    }



    }, 2000);

  });

}



// createPost({title:'post three', body:'this is post three'})

// .then(getPosts);

// .catch(err => console.log(err));



// promise.all

const promise1 = Promise.resolve('Hello World');

const promise2=10;

const promise3= new Promise((resolve, reject)=>setTimeout(resolve, 2000, 'Goodbye'));

 

const promise4=fatch('https://jsonplaceholder.typicode.com/users').then(res=>res.json());

Promise.all([promise1, promise2, promise3, promise4]).then(values=>console.log(values))

</script>



</html>
