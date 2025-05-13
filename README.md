<?php
session_start();
if (isset($_SESSION['admin_logged_in'])) {
  header("Location: admin_dashboard.php");
  exit();
}
?>

<!DOCTYPE html>
<html>
<head>
  <title>Admin Login</title>
</head>
<body>
  <h2>Admin Login</h2>
  <form method="POST" action="admin_login.php">
    <input type="text" name="username" placeholder="Username" required><br><br>
    <input type="password" name="password" placeholder="Password" required><br><br>
    <button type="submit">Login</button>
  </form>

  <?php
  if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = $_POST['username'];
    $password = $_POST['password'];

    // Replace with your desired username/password
    if ($username === 'admin' && $password === '1234') {
      $_SESSION['admin_logged_in'] = true;
      header("Location: admin_dashboard.php");
      exit();
    } else {
      echo "<p style='color:red;'>Invalid credentials!</p>";
    }
  }
  ?>
</body>
</html>


<?php
session_start();
if (!isset($_SESSION['admin_logged_in'])) {
  header("Location: admin_login.php");
  exit();
}

$conn = new mysqli("localhost", "root", "", "topupdb");

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

$sql = "SELECT * FROM orders ORDER BY order_time DESC";
$result = $conn->query($sql);
?>

<!DOCTYPE html>
<html>
<head>
  <title>Admin Dashboard</title>
  <style>
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
  </style>
</head>
<body>
  <h2>Admin Dashboard</h2>
  <p><a href="admin_logout.php">Logout</a></p>

  <table>
    <tr>
      <th>ID</th>
      <th>UID</th>
      <th>Package</th>
      <th>Payment Method</th>
      <th>TrxID</th>
      <th>Time</th>
    </tr>
    <?php while($row = $result->fetch_assoc()) { ?>
    <tr>
      <td><?= $row['id'] ?></td>
      <td><?= $row['uid'] ?></td>
      <td><?= $row['package'] ?></td>
      <td><?= $row['payment_method'] ?></td>
      <td><?= $row['trx_id'] ?></td>
      <td><?= $row['order_time'] ?></td>
    </tr>
    <?php } ?>
  </table>
</body>
</html>

<?php
session_start();
session_destroy();
header("Location: admin_login.php");
exit();
