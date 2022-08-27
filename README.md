- 👋<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<html>
<body>

        <form onsubmit="saveToLocalStorage(event)">
            <label> Name</label>
            <input type="text" name="username"  required/>
            <label> EmailId</label>
            <input type="email" name="emailId"  required/>
            <label> Phone Number</label>
            <input type="tel" name="phonenumber" />
            <button> Submit </button>
        </form>
        <ul id='listOfitems'></ul>
        <script>
            function saveToLocalStorage(event) {
                event.preventDefault();
                const name = event.target.username.value;
                const email = event.target.emailId.value;
                const phonenumber = event.target.phonenumber.value;
                // localStorage.setItem('name', name);
                // localStorage.setItem('email', email);
                // localStorage.setItem('phonenumber', phonenumber)
                const obj = {
                    name,
                    email,
                    phonenumber
                }
                localStorage.setItem(obj.email, JSON.stringify(obj))
                showListofRegisteredUser(obj)
            }

            window.addEventListener('DOMContentLoaded', (event) => {
                Object.keys(localStorage).forEach(key => {
                    const user = JSON.parse(localStorage.getItem(key))
                    showListofRegisteredUser(user)
                })
            })

            function showListofRegisteredUser(user){
                const parentNode = document.getElementById('listOfitems');
                const createNewUserHtml = `<li id='${user.email}'>${user.name} - ${user.email} - ${user.phonenumber}
                                                <button onclick=deleteUser('${user.email}')>Delete</button>
                                            </li>
                                            `
                console.log(createNewUserHtml)
                parentNode.innerHTML +=  createNewUserHtml;
                console.log(parentNode.innerHTML)
            }

            function deleteUser(email) {
                localStorage.removeItem(email)
                removeItemFromScreen(email)
            }

            function removeItemFromScreen(email){
                const parentNode = document.getElementById('listOfitems');
                const elem = document.getElementById(email)
                parentNode.removeChild(elem);
            }
        </script>


    </body>
</html> 
