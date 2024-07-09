# Steps we need to complete this task
To create a new web page that retrieves and displays the most recently added age from the MySQL database, you'll need to follow these steps:

## Step 1: Create a New PHP File
Add the following code to fetch_last_movement.php

## Step 2: Write the PHP Script
``` PHP
<?php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "robotmove";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$sql = "SELECT direction FROM movements ORDER BY id DESC LIMIT 1";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    echo json_encode(["status" => "success", "direction" => $row["direction"]]);
} else {
    echo json_encode(["status" => "error", "message" => "No movements found"]);
}

$conn->close();
?>
```

## Step 3: Link the New Page
You can add a link to fetch_last_movement.php from your existing form page (index.php) if you want easy navigation between the pages.
Add a Link to index.php
Modify your index.php file to include a link to fetch_last_movement.php:

``` HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="keywords" content="HTML, CSS" />
    <meta name="description" content="Controlling the movements of the robot" />
    <title>Controlling the robot movement</title>
    <link rel="stylesheet" href="styles.css" />
    <script>
      function sendMovement(direction, element) {
        var xhr = new XMLHttpRequest();
        xhr.open("POST", "send_movement.php", true);
        xhr.setRequestHeader(
          "Content-Type",
          "application/x-www-form-urlencoded"
        );
        xhr.onreadystatechange = function () {
          if (xhr.readyState === 4 && xhr.status === 200) {
            var response = JSON.parse(xhr.responseText);
            if (response.status === "success") {
              // Update last movement display
              document.getElementById("last-movement").innerText =
                "Last Movement: " + direction;
            } else {
              console.error("Error:", response.message);
            }
          }
        };
        xhr.send("direction=" + direction);

        element.classList.add("active");
        setTimeout(function () {
          element.classList.remove("active");
        }, 200);
      }
    </script>
  </head>
  <body>
    <div class="container">
      <div class="rectangle up" onclick="sendMovement('up', this)">Up</div>
      <div class="middle">
        <div class="rectangle left" onclick="sendMovement('left', this)">
          Left
        </div>
        <div class="rectangle center" onclick="sendMovement('stop', this)">
          Stop
        </div>
        <div class="rectangle right" onclick="sendMovement('right', this)">
          Right
        </div>
      </div>
      <div class="rectangle down" onclick="sendMovement('down', this)">
        Down
      </div>
    </div>
    <div id="last-movement" class="last-movement">Last Movement: None</div>
  </body>
</html>
```


