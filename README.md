**Google Sheet Integration For PHP\
\
 1.Set Up A Project In The Google Cloud Platform Console:**

-   ***Go to the Google Cloud Platform Console
    (***[***https://console.cloud.google.com/***](https://console.cloud.google.com/)***)***
-   Create a new project or select an existing one.
  
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/1.png)

-   After Create Project ENABLE APIS AND SERVICES
       
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/2.png)

-   ENABLE APIS AND SERVICES Click And Select Google Sheet API Enable

![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/3.png)
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/4.png)

-   After Enable Google Sheet API Create Credentials

![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/5.png)
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/6.png)


-    Add Or Remove Scope
    
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/7.png)
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/8.png)


-   After Save And Continue

![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/9.png)

-   After Click Create Credential
  
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/10.png)

-   Select Application Type
  
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/11.png)
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/12.png)

-   Download Cedentials json File

![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/13.png)

**2. Create Spreadsheet**

-   Create a new spreadsheet in [Google
    Drive](https://drive.google.com/)
-   Save the ****file ID ****from address bar.
-   For e.g :
    [https://docs.google.com/spreadsheets/d/](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)[1omK33](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*[/edit\#gid=0](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)
-   your\_spreadsheet\_id :
    [1omK33](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

****

 **3. Install The Google Client Library For PHP**

-   *Y*ou can install the library using Composer, a popular PHP
    package manager. Run the following command in your project director
-   composer require google/apiclient:\^2.0 -->Run this command Your
    Project

 **4. Authenticate With Google Sheets API Using OAuth 2.0:**

-   Load the JSON credentials file you downloaded in step 1.
-    Set up a redirect URI for your OAuth 2.0 credentials
    (e.g., http://127.0.0.1:8000/Integration).
-   Use the following code to authenticate and authorize the API

```
<?php
require_once __DIR__ . '/vendor/autoload.php';
$client = new Google\_Client();
$client->setAuthConfig('path\_to\_credentials\_file.json');
$client->addScope(Google\_Service\_Sheets::SPREADSHEETS);
$client->setRedirectUri('http://127.0.0.1:8000/Integration');
if (!isset(\$\_GET\['code'\]))
{
    $authUrl = \$client->createAuthUrl();
    header('Location: ' . \$authUrl);
     exit;
}
else
{
     $client->fetchAccessTokenWithAuthCode($_GET['code']);
     $accessToken = $client->getAccessToken();
     // Save $accessToken for future use

}

Note: Replace 'path\_to\_credentials\_file.json' with the actual path to
your credentials file.
?>
```
**5.Google Sheets Data Fetch:**

-   Once you have obtained the access token, you can use it to make
    requests to the Google Sheets API.
-   Here's an example of reading data from a spreadsheet
```
 <?php
 require_once __DIR__ . '/vendor/autoload.php';
 $client = new Google_Client();
 $client->setAuthConfig('path_to_credentials_file.json');
 $client->addScope(Google_Service_Sheets::SPREADSHEETS);
 $client->setAccessToken($accessToken);
 $service = new Google_Service_Sheets($client);
 $spreadsheetId = 'your_spreadsheet_id';
 $range = 'Sheet1';
 $response = $service->spreadsheets_values->get($spreadsheetId, $range);
 $values = $response->getValues();
 if (empty($values)) 
 {
      echo 'No data found.';
 } 
 else 
 {
      foreach ($values as $row) 
  {
   foreach ($row as $cell) 
   {
              echo $cell . "\t";
           }
            echo "<br>";
      }
 }
```

**6. Google Sheets Data Append (Insert):**

```
$client = new Google_Client();
$client->setAuthConfig('path_to_credentials_file.json');
$client->addScope(Google_Service_Sheets::SPREADSHEETS);
$client->setAccessToken($accessToken);
$service = new Google_Service_Sheets($client);
$spreadsheetId = 'your_spreadsheet_id';
$range = 'Sheet1';
     $updateBody = new Google_Service_Sheets_ValueRange([
        'range' => 'Sheet1',
        'majorDimension' => 'COLUMNS',
        'values' => [
          ['test2'],['testing_new2'],['testinf@gmail.com2']
                 ]
   ]);
   $result = $service->spreadsheets_values->append($spreadsheetId,$range,$updateBody,
        ['valueInputOption' => "RAW"]
   );
   if(isset($result['spreadsheetId']))
   {
       echo "success";
   }
   else
   {
       echo "failure";
   }
```

**7. Google Sheets Data Update (Edit):**

```
$client = new Google_Client();
$client->setAuthConfig('path_to_credentials_file.json');
$client->addScope(Google_Service_Sheets::SPREADSHEETS);
$client->setAccessToken($accessToken);
$service = new Google_Service_Sheets($client);
$spreadsheetId = 'your_spreadsheet_id';
$range = 'Sheet1';
$values = [
    ['testing first name ', 'testing last name','newtesting@gmail.com'],
];
$body = new \Google_Service_Sheets_ValueRange([
    'values' => $values
]);
$params = [
    'valueInputOption' => 'RAW'
];
$result = $service->spreadsheets_values->update($spreadsheetId, $range, $body, $params);
if(isset($result['spreadsheetId']))
{
    echo "success";
}
else
{
    echo "failure";
}
```
