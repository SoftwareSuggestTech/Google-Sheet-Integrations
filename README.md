**Google Sheet Integration For PHP\
\
 1.Set up a project in the Google Cloud Platform Console:**

-   ***Go to the Google Cloud Platform Console
    (***[***https://console.cloud.google.com/***](https://console.cloud.google.com/)***)***
-   Create a new project or select an existing one.
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/1.png)

-   After Create Project ENABLE APIS AND SERVICES
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/2.png)
-   ENABLE APIS AND SERVICES Click And Select Google Sheet API Enable
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/3.png)
-   After Enable Google Sheet API Create Credentials
![alt text](https://google-sheet-integrations.s3.us-east-2.amazonaws.com/Google+Sheet+API+Intrigation/4.png)
-   Add Or Remove Scope

-   After Save And Continue

-   After Click Create Credential

-   Select Application Type

-   Download Cedentials json File

**2. Create spreadsheet**

-   Create a new spreadsheet in [Google
    Drive](https://drive.google.com/)
-   Save the ****file ID ****from address bar.
-   For e.g :
    [https://docs.google.com/spreadsheets/d/](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)[1omK33](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*[/edit\#gid=0](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)
-   your\_spreadsheet\_id :
    [1omK33](https://docs.google.com/spreadsheets/d/1omK338NT3DQQvaWPNSnqEZJ1yNep0jmruc5e012w7/edit#gid=0)\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

****

** 3.Install the Google Client Library for PHP**

-   *Y*ou can install the library using Composer, a popular PHP
    package manager. Run the following command in your project director
-   composer require google/apiclient:\^2.0 --&gt;Run this command Your
    Project

 **4.Authenticate with Google Sheets API using OAuth 2.0:**

-   Load the JSON credentials file you downloaded in step 1.
-    Set up a redirect URI for your OAuth 2.0 credentials
    (e.g., http://127.0.0.1:8000/intrigation).
-   Use the following code to authenticate and authorize the API

&lt;?php

require\_once \_\_DIR\_\_ . '/vendor/autoload.php';

\$client = new Google\_Client();

\$client-&gt;setAuthConfig('path\_to\_credentials\_file.json');

\$client-&gt;addScope(Google\_Service\_Sheets::SPREADSHEETS);

\$client-&gt;setRedirectUri('http://127.0.0.1:8000/intrigation');

if (!isset(\$\_GET\['code'\]))

 {

 \$authUrl = \$client-&gt;createAuthUrl();

 header('Location: ' . \$authUrl);

 exit;

} else {

 \$client-&gt;fetchAccessTokenWithAuthCode(\$\_GET\['code'\]);

 \$accessToken = \$client-&gt;getAccessToken();

 // Save \$accessToken for future use

}

Note: Replace 'path\_to\_credentials\_file.json' with the actual path to
your credentials file.

**5. Google Sheets data Fetch(Get):**

-   Once you have obtained the access token, you can use it to make
    requests to the Google Sheets API.
-   Here's an example of reading data from a spreadsheet

 &lt;?php

require\_once \_\_DIR\_\_ . '/vendor/autoload.php';

\$client = new Google\_Client();

\$client-&gt;setAuthConfig('path\_to\_credentials\_file.json');

\$client-&gt;addScope(Google\_Service\_Sheets::SPREADSHEETS);

\$client-&gt;setAccessToken(\$accessToken);

\$service = new Google\_Service\_Sheets(\$client);

\$spreadsheetId = 'your\_spreadsheet\_id';

\$range = 'Sheet1';

\$response = \$service-&gt;spreadsheets\_values-&gt;get(\$spreadsheetId,
\$range);

\$values = \$response-&gt;getValues();

if (empty(\$values))

{

 echo 'No data found.';

}

else

{

 foreach (\$values as \$row)

{

foreach (\$row as \$cell)

{

 echo \$cell . "\\t";

 }

 echo "&lt;br&gt;";

 }

}

**6. Google Sheets data Append (Insert):**

****\$client = new Google\_Client();

\$client-&gt;setAuthConfig('path\_to\_credentials\_file.json');

\$client-&gt;addScope(Google\_Service\_Sheets::SPREADSHEETS);

\$client-&gt;setAccessToken(\$accessToken);

** ** \$service = new Google\_Service\_Sheets(\$client);

 \$spreadsheetId = 'your\_spreadsheet\_id';

 \$range = 'Sheet1';

 \$updateBody = new Google\_Service\_Sheets\_ValueRange(\[

 'range' =&gt; 'Sheet1',

 'majorDimension' =&gt; 'COLUMNS',

 'values' =&gt; \[

 \['test2'\],\['testing\_new2'\],\['testinf@gmail.com2'\]

 \]

 \]);

 \$result =
\$service-&gt;spreadsheets\_values-&gt;append(\$spreadsheetId,\$range,\$updateBody,

 \['valueInputOption' =&gt; "RAW"\]

 );

 if(isset(\$result\['spreadsheetId'\]))

 {

 echo "success";

 }

 else

 {

 echo "failure";

 }

**7. Google Sheets data Update (Edit):**

** **\$client = new Google\_Client();

 \$client-&gt;setAuthConfig('path\_to\_credentials\_file.json');

 \$client-&gt;addScope(Google\_Service\_Sheets::SPREADSHEETS);

 \$client-&gt;setAccessToken(\$accessToken);

** **\$service = new Google\_Service\_Sheets(\$client);

 \$spreadsheetId = 'your\_spreadsheet\_id';

 \$range = 'Sheet1';

 \$values = \[

 \['testing first name ', 'testing last name','newtesting@gmail.com'\],

 \];

 \$body = new \\Google\_Service\_Sheets\_ValueRange(\[

 'values' =&gt; \$values

 \]);

 \$params = \[

 'valueInputOption' =&gt; 'RAW'

 \];

 \$result =
\$service-&gt;spreadsheets\_values-&gt;update(\$spreadsheetId, \$range,
\$body, \$params);

 if(isset(\$result\['spreadsheetId'\]))

 {

 echo "success";

 }

 else

 {

 echo "failure";

 }
