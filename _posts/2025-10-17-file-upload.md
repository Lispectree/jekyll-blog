# High-level summary

File upload vulnerabilities occur when an application allows users to upload files without proper security checks. Attackers can exploit this to upload scripts or other malicious files, potentially executing code on the server. Learning about these flaws helps developers secure file upload features and prevent unauthorized access.

## Lab 1 — Remote Code Execution via Web Shell Upload

**What this teaches:** How uploading a PHP file can let you execute commands on the server.

**Simple beginner walkthrough:**

1. Log in and upload any image as your avatar.
2. In Burp, find the GET request for your avatar and send it to Repeater.
3. Create a file called exploit.php with a simple script to read Carlos's secret.
4. Upload this PHP file as your avatar.
5. Use Burp Repeater to GET the PHP file and observe that the server executes it. Submit the secret to complete the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/c10e6e163d1717c993e22c517da142ea97904a11/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB1%20PHOTO1.jpg)
The upload file functionality


![image alt](https://github.com/Lispectree/web-sec/blob/b0c57815d6b9c77680be2bef5d531fe2a0e4323a/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB1%20PHOTO2.jpg)
The php file was uploaded to this path


![image alt](https://github.com/Lispectree/web-sec/blob/0f169bec9b759024c56916d465368b664f04a384/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB1%20PHOTO3.jpg)
Load that contents





## Lab 2 — Web Shell Upload via Path Traversal

**What this teaches:** How directory traversal in filenames can let scripts be placed outside the normal folder.

**Simple beginner walkthrough:**

1. Upload any avatar image and locate the GET request in Burp Repeater.
2. Create exploit.php to read Carlos's secret.
3. Modify the POST request filename to "../exploit.php" and forward it.
4. URL-encode the traversal sequence (..%2f) and forward the request again.
5. GET the file in the browser and submit the secret.

![image alt](https://github.com/Lispectree/web-sec/blob/f207f04b8baecce883c29f7dc94890073ddb69dc/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB2%20PHOTO1.jpg)
The payload that gets the api key of Carlos doesn’t execute 
Indicating that the server doesn’t execute files that are uploaded by the client


![image alt](https://github.com/Lispectree/web-sec/blob/1abe727876de4b8a56d5675b01d1bacb5ba52dd2/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB2%20PHOTO2.jpg)
Change the path of the uploaded file by exploiting path traversal


## Lab 3 — Web Shell Upload via Extension Blacklist Bypass

**What this teaches:** How server checks on file extensions can be bypassed with a crafted .htaccess file.

**Simple beginner walkthrough:**

1. Upload any avatar image and find the GET request in Burp.
2. Create exploit.php to read Carlos's secret.
3. Upload a .htaccess file mapping .l33t to PHP.
4. Rename exploit.php to exploit.l33t and upload it.
5. GET exploit.l33t in Burp Repeater to read the secret and submit it.

   ![image alt](https://github.com/Lispectree/web-sec/blob/4b9dc8546d3f54b0f28d8b87d620f4dc6ba529c3/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB3%20PHOTO1.jpg)
   Php files are not allowed to be uploaded


   ![image alt](https://github.com/Lispectree/web-sec/blob/5f652b781348f7c35a280e3979826fa2db38f9b9/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB3%20PHOTO2.jpg)
The response discloses additional information like the server being an Apache server


   ![image alt](https://github.com/Lispectree/web-sec/blob/e9813bcb0342b97193fdfc083ade14dd6f426af2/web-security-labs/labs/file-upload/FILE%20UPLOAD%20LAB3%20PHOTO3.jpg)
   Create a daemon file (httpd) that will make an executable file work





## Lab 4 — Web Shell Upload via Obfuscated File Extension

**What this teaches:** How null bytes or hidden characters in filenames can bypass file type restrictions.

**Simple beginner walkthrough:**

1. Upload any avatar image and locate the GET request in Burp Repeater.
2. Create exploit.php to read Carlos's secret.
3. Modify the filename in the POST request to exploit.php%00.jpg and forward it.
4. GET exploit.php in Repeater and observe the secret.
5. Submit the secret to finish the lab.



