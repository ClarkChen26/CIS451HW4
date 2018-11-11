<?php

include('connectionData.txt');

$conn = mysqli_connect($server, $user, $pass, $dbname, $port)
or die('Error connecting to MySQL server.');

?>

<html>
<head>
  <title>Another Simple PHP-MySQL Program</title>
  </head>
  
  <body bgcolor="white">
  
  
  <hr>
  
  
<?php
  
$state = $_POST['state'];

$state = mysqli_real_escape_string($conn, $state);
// this is a small attempt to avoid SQL injection
// better to use prepared statements

$query = "select fname, lname, ifnull(orders.order_num, 'no order') as order_num, orders.order_date as o_date, newchart.items_amount as num_of_item, newchart.total_amount_spent as amount
from customer left join orders using(customer_num) left join (select sum(quantity) as items_amount, order_num, sum(total_price) as total_amount_spent from items
group by order_num) as newchart using (order_num) Where lname = ";
$query = $query."'".$state."' ORDER BY 2;";

?>

<p>
The query:
<p>
<?php
print $query;
?>

<hr>
<p>
Result of query:
<p>

<?php
$result = mysqli_query($conn, $query)
or die(mysqli_error($conn));

print "<pre>";
while($row = mysqli_fetch_array($result, MYSQLI_BOTH))
  {
    print "\n";
    print "$row[fname] $row[lname] $row[order_num] $row[amount] $row[num_of_item] $row[o_date]";
  }
print "</pre>";

mysqli_free_result($result);

mysqli_close($conn);

?>

<p>
<hr>

<p>
<a href="findCustState.txt" >Contents</a>
of the PHP program that created this page. 	 
 
</body>
</html>
	  