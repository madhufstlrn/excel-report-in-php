# excel-report-in-php
<?php

$filename = "excelfilename";         //File Name
  
$sql = "select * from candidate_party_master a
inner join parliamentary_constituency_master on parlimentary_const_id=a.parliamentary_constituency_id
inner join party_master b on b.party_id=a.party_id";

//execute query 
$result = mysql_query($sql) or die("Couldn't execute query:<br>" . mysql_error(). "<br>" . mysql_errno());    
$file_ending = "xls";
//header info for browser
header("Content-Type: application/xls");    
header("Content-Disposition: attachment; filename=$filename.xls");  
header("Pragma: no-cache"); 
header("Expires: 0");
/*******Start of Formatting for Excel*******/   
//define separator (defines columns in excel & tabs in word)
$sep = "\t"; //tabbed character
//start of printing column names as names of MySQL fields
for ($i = 0; $i < mysql_num_fields($result); $i++) {
echo mysql_field_name($result,$i) . "\t";
}
print("\n");    
//end of printing column names  
//start while loop to get data
    while($row = mysql_fetch_row($result))
    {
        $schema_insert = "";
        for($j=0; $j<mysql_num_fields($result);$j++)
        {
            if(!isset($row[$j]))
                $schema_insert .= "NULL".$sep;
            elseif ($row[$j] != "")
                $schema_insert .= "$row[$j]".$sep;
            else
                $schema_insert .= "".$sep;
        }
        $schema_insert = str_replace($sep."$", "", $schema_insert);
        $schema_insert = preg_replace("/\r\n|\n\r|\n|\r/", " ", $schema_insert);
        $schema_insert .= "\t";
        print(trim($schema_insert));
       print "\n";
    }   
?>
---------------------------------------------------------------------------

<?php
 
  $xls_filename = 'export_'.date('Y-m-d').'.xls'; // Define Excel (.xls) file name
   
  /***** DO NOT EDIT BELOW LINES *****/
  // Create MySQL connection
  $sql = "
select * from candidate_party_master a
inner join parliamentary_constituency_master on parlimentary_const_id=a.parliamentary_constituency_id
inner join party_master b on b.party_id=a.party_id ";
  
  $result = @mysql_query($sql) or die("Failed to execute query:<br />" . mysql_error(). "<br />" . mysql_errno());
   
  // Header info settings
  header("Content-Type: application/xls");
  header("Content-Disposition: attachment; filename=$xls_filename");
  header("Pragma: no-cache");
  header("Expires: 0");
   
  /***** Start of Formatting for Excel *****/
  // Define separator (defines columns in excel &amp; tabs in word)
  $sep = "\t"; // tabbed character
   
  // Start of printing column names as names of MySQL fields
  for ($i = 0; $i<mysql_num_fields($result); $i++) {
    echo mysql_field_name($result, $i) . "\t";
  }
  print("\n");
  // End of printing column names
   
  // Start while loop to get data
  while($row = mysql_fetch_row($result))
  {
    $schema_insert = "";
    for($j=0; $j<mysql_num_fields($result); $j++)
    {
      if(!isset($row[$j])) {
        $schema_insert .= "NULL".$sep;
      }
      elseif ($row[$j] != "") {
        $schema_insert .= "$row[$j]".$sep;
      }
      else {
        $schema_insert .= "".$sep;
      }
    }
    //$schema_insert = str_replace($sep."$", "", $schema_insert);
    //$schema_insert = preg_replace("/\r\n|\n\r|\n|\r/", " ", $schema_insert);
    $schema_insert .= "\t";
    print(trim($schema_insert));
    print "\n";
  }
?>
