# site-web
c'est un petit site web de restaurant
#page-index.php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>restaurant xyz</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header><h1>restaurant xyz</h1>
    <nav><ul>
        <li><a href="#home">accueil</a></li>
        <li><a href="#menu">menu</a></li>
        <li><a href="#contact">contact</a></li>
</ul>
</nav>
</header>
<section id="home">
    <h2>bienvenue chez restaurant xyz </h2>
    <p>decouvrez nos plats delicieux faits maison.</p>
</section>
<?php
           $con=new mysqli($servername="localhost",$username="root",$password="",$dbname="restaurant_db");
           if($con->connect_error){
               die("erreur de connexion:".$con->connect_error);
   
           }
           $sql="select*from menu";
           $reslt=$con->query($sql);
           if($reslt == false){
            echo"erreur dans la requete:".$con->error;
           }

           ?>

<section id="menu" >
    <h2>Notre menu</h2>
    <div style="display:flex; flex-wrap:wrap; gap:20px;">
          
        <?php
        $sql="select* from menu";
        $reslt=$con->query($sql);
        if($reslt->num_rows>0){
            while($row=$reslt->fetch_assoc()){
                echo"<div style='border:1px solid #ddd; padding:15px; text-align:center;width:200px;box-shadow:2px 2px 8px rgba(0,0,0,0.1);'><img src='{$row['image']}' alt='{$row['name']}' style='width:100%;height:auto; margin-bottom:10px;'>
                <p style='font-weight:bold; margin:0;'>{$row['name']}</p>
                <p style='color:#555;'>{$row['price']}
                £</p>
                </div>";
            }
        } else{
                echo"<p> class='no-data'>aucun plat trouvé.</p>";
            }
        
        ?>
         
    </div>
        </section>
<?php
        $con->close();
        ?>
<section id="contact">
    <h2>Contactez-nous</h2>
    <form action="contact.php" method="post">
        <label for="name">Nom:</label><input type="text" id="name" name="name" required>
        <label for="email">Email:</label><input type="email" id="email" name="email" required>
        <label for="message">message:</label><textarea id="message" name="message" required></textarea>
        <button type="submit">envoyer </button>
    </form>
    </section>
    <footer>
        <p>2024 restaurant xyz.Tous droits réservés.</p>
    </footer>

    
</body>
</html>
#page-contact.php
<?php
if($_SERVER['REQUEST_METHOD']=='POST'){
    $name=htmlentities($_POST['name']);
    $email=htmlentities($_POST['email']);
    $message=htmlentities($_POST['message']);
    if(!empty($name) && !empty($email)&& !empty($message)){
        echo"<h3>message recu:</h3>";
        echo"<table border='1' style='border-collapse:collapse;width:50%;text-align:left;'>";
        echo"<tr><th style='padding:8px;background-color:#f2f2f2;'>champ</th><th style='padding:8px;background-color:#f2f2f2;'>valeur</th></tr>";

        echo"<tr><td style='padding:8px;'>name</td><td style='padding:8px;'>$name</td></tr>";
        echo"<tr><td style='padding:8px;'>email</td><td style='padding:8px;'>$email</td></tr>";
        echo"<tr><td style='padding:8px;'>message</td><td style='padding:8px;'>$message</td></tr>";
        echo"</table>";
    }
    else{
        "<p style='color:red;>veuillez remplir tous les champs</p>";
    }
}
else{
    echo"<p style='color:red;'>methode non autoriséé</p>";

}

?>
page-style.css
body{
    font-family: Arial, Helvetica, sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
header{
    background-color: #333;
    color: white;
    padding: 10px 20px;
    text-align:center;
}
nav ul{
    list-style-type:none;
    padding: 0;
    display: flex;
    justify-content:center;
}
nav ul li{
    margin: 0 15px;

}
nav ul li a{
    color: white;
    text-decoration: none;
}
section{
    padding: 20px;
}
section#menu div p{
    margin: 5px 0;

}
#menu img{
    border-radius:10px;
    margin: 10px 0;
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
.no-data{
    color: red;
    font-weight: bold;
    text-align: center;
    margin-top: 10px;
}
footer{
    text-align: center;
    padding: 10px;
    background: #333;
    color: white;
}
