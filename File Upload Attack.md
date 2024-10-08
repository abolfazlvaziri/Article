![1*NAYTg3CUOsytlmlibzJ8jg](https://github.com/user-attachments/assets/33ce1a95-25b5-463a-8153-eebc492031d7)

File upload vulnerabilities represent one of the most critical security risks faced by web applications. As businesses increasingly rely on user generated content and file sharing, the potential for exploitation has grown significantly. Attackers can leverage insecure file upload functionalities to execute various malicious activities, including the execution of arbitrary code, data theft, and server compromise.

### Where File Upload Vulnerability Found?

File uploads Vulnerability can be found in various parts of web applications, such as profile, support & report, and feedback sections. Users may upload files like profile images, reports, and suggestions.

### How Can We Upload a Shell?

We can upload a shell using various techniques. Generally, main types of restrictions that can be bypassed:

#### File Extension 

Common extensions include: `php, phtml, php5, php7`, and variations like `Php, PHP, PhP`. In the upload section, we can use Burp Suite Intruder to fuzz file extensions, allowing us to easily identify additional valid extensions. This technique can facilitate the injection of malicious files containing payloads.

#### **File Content (Signature)**

We can modify the file content by removing existing data and inserting a JavaScript payload. Additionally, we can append a payload to the end of a file’s content, such as a PNG file. For example:

```php
"<?php system('id');?>" >> file.png
```

#### **Content Type**

The content type can be manipulated to any type, including `image/png`, `text/plain`, and `text/html`. 

#### **Double Extension**

Double extensions can be utilized to bypass restrictions, such as:

- file.txt.php 
- file.png.php 
- file.php.#.jpg 
- file.png%00.php 
- file.php%0a.png

#### **AddHandler**

When uploading files, it’s essential to search for the appropriate PHP handler and test each one individually. Sometimes, a web server may not execute code because the handler does not support the file extension. In such cases, we can redefine the handler. For example, by adding the following line below the Content-Type:

```http
AddHandler application/x-httpd-php .avn
```

This allows us to upload a file with the `.avn` extension, enabling it to execute as PHP code.

#### **mod_rewrite**

In Apache, the `mod_rewrite` module allows for URL rewriting, and it recognizes the presence of the `.htaccess` file for various configurations, including setting directory permissions. By default, directory listing is often disabled with Options `-Indexes`.

If we upload a `.htaccess` file and modify its content to:

```http
Options +Indexes
```

We can enable directory listing, effectively changing the response status from 403 (Forbidden) to 200 (OK). Additionally, we can combine this with the AddHandler trick. The final upload would look like this:

```http
Content-Disposition: form-data; name="file"; filename=".htaccess"  
Content-Type: text/plain  
  
Options +Indexes
```

This approach allows us to exploit directory listings while utilizing the AddHandler technique.

Understanding file upload vulnerabilities is crucial for effective penetration testing. By mastering these techniques, you can enhance your skills and contribute to stronger security practices. Stay vigilant and keep learning as the cybersecurity landscape evolves.

---

GitHub: [https://github.com/abolfazlvaziri](https://github.com/abolfazlvaziri)  
Instagram: [https://instagram.com/abolfazlvaziriofficial](https://instagram.com/abolfazlvaziriofficial)  
Telegram Channel: [https://t.me/AVN_COMMUNITY](https://t.me/AVN_COMMUNITY)  
YouTube: [https://www.youtube.com/@abolfazlvaziri](https://www.youtube.com/@abolfazlvaziri)
