lang-translation
================

A php application which can display the word and its translation in other language. It can also perform operations like insert, modify, delete.
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
</head>
<body>

<?php
$con=mysqli_connect("localhost:3307","root","root","translation");
// Check connection
if (mysqli_connect_errno()) {
  echo "Failed to connect to MySQL: " . mysqli_connect_error();
}

$result = mysqli_query($con,"SELECT id,word,meaning FROM vocabulary");
echo mysqli_num_rows($result);

echo "<table border='1'>
<tr>
<th>No</th>
<th>Word</th>
<th>Meaning</th>
</tr>";

while($row = mysqli_fetch_array($result)) {
 
 echo "<tr>";
    echo "<td>" . "{$row['id']}   " . "</td>";
     echo "<td>" . "{$row['word']}  " . "</td>";
      echo "<td>" . "{$row['meaning']}" . "</td>";
         //"--------------------------------<br>";
     echo "</tr>";
}
echo "<center><b><h1>Vocabulary</h1></b></center>";

mysqli_close($con);
?>

</body>
</html>

<!DOCTYPE html>
<html>
<head>
<title>Add New Record in MySQL Database</title>
<meta charset="utf-8">
</head>
<body>
<?php
if(isset($_POST['add']))
{
$dbhost = 'localhost:3307';
$dbuser = 'root';
$dbpass = 'root';
$conn = mysql_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
  die('Could not connect: ' . mysql_error());
}

if(! get_magic_quotes_gpc() )
{
   $word = addslashes ($_POST['word']);
   $meaning = addslashes ($_POST['meaning']);
   $type = addslashes ($_POST['type']);
}
else
{
   $word = $_POST['word'];
   $meaning = $_POST['meaning'];
   $type = $_POST['type'];
}

$sql = "INSERT INTO vocabulary ".
       "(word,meaning,type) ".
       "VALUES ".
       "('$word','$meaning','$type')";
mysql_select_db('translation');
$retval = mysql_query( $sql, $conn );
if(! $retval )
{
  die('Could not enter data: ' . mysql_error());
}
echo "Entered data successfully\n";
mysql_close($conn);
}
else
{
?>
<form method="post" action="<?php $_PHP_SELF ?>">
<table width="600" border="0" cellspacing="1" cellpadding="2">
<tr>
<td width="250"><h3>Word</h3></td>
<td>
<input name="word" type="text" id="word">
</td>
</tr>
<tr>
<td width="250"><h3>Meaning</h3></td>
<td>
<input name="meaning" type="text" id="meaning">
</td>
</tr>
<tr>
<td width="250"><h3>Type</h3></td>
<td>
<input name="type" type="text" id="type">
</td>
</tr>
<tr>
<td width="250"> </td>
<td> </td>
</tr>
<tr>
<td width="250"> </td>
<td>
<input name="add" type="submit" id="add" value="Add vocabulary">
</td>
</tr>
</table>
</form>
<?php
}
?>
</body>
</html>
