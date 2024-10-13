# **File Path Traversal: Simple Case**

## **What is Path Traversal?**
Path traversal, also known as directory traversal, is a web vulnerability that allows an attacker to access files and directories stored outside the web root folder. This can happen when an application does not properly sanitize user input, leading to exposure of sensitive files like `/etc/passwd`.

In this specific lab, the vulnerability allows attackers to access files on the server by manipulating the file path input.

## **Using Burp Suite for Path Traversal**
You can exploit this vulnerability using the Burp Suite tool, which allows you to intercept and manipulate HTTP requests. In this case, Burp Suite helps in changing the file path within the request to retrieve the `/etc/passwd` file.

1. **Capture the request using Burp Suite's Proxy tab.**
2. **Identify the vulnerable parameter (the file path).**
3. **Modify the file path** by replacing the original file (e.g., `product.jpg`) with the path traversal payload (`../../../../../../etc/passwd`).
4. **Send the request through Burp Suite’s Repeater tool** and verify the response. The contents of `/etc/passwd` will be displayed if the attack is successful.

## **Key Concepts**
- **Vulnerability:** Path traversal
- **Target File:** `/etc/passwd`
- **Attack Method:** Manipulating file paths in requests to access unauthorized directories and files.

## **Step-by-Step Approach to Solve the Lab**

1. **Understanding the Application:**
   The lab application displays product images. These images are typically fetched from a directory on the server, and the file paths are likely constructed based on user input.

2. **Identifying the Vulnerability:**
   When users interact with the product images, the file paths may be vulnerable to manipulation. If the application does not sanitize the user input properly, you can traverse the directory structure to access files outside of the intended directory.

3. **Basic Attack Payload:**
   The goal is to reach the root directory `/` and access the sensitive `/etc/passwd` file, which contains information about user accounts on the system.

   A common payload for path traversal is: ../../../../../../etc/passwd

   The `../` components move up the directory structure. The number of `../` components depends on how deep the current working directory is. However, using multiple `../` ensures that you go back to the root directory `/`.

4. **Steps to Solve the Lab:**
- **Inspect the File Request:**
  Use browser tools such as "Inspector" or "Network" tabs to examine the URL or POST request sent when you view a product image.

  **Example request URL:**
  ```
  http://example.com/images?file=product.jpg
  ```

- **Modify the File Path:**
  Replace the legitimate image file with the path traversal payload to attempt accessing `/etc/passwd`.

  **Example modified URL:**
  ```
  http://example.com/images?file=../../../../../../etc/passwd
  ```

- **Send the Request:**
  Upon submitting this request, if the vulnerability exists, the server will respond with the contents of the `/etc/passwd` file.

- **Verify the Response:**
  If successful, you will see the content of `/etc/passwd`, which typically looks like:
  ```
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  ```
  These entries represent user accounts on the system.

## **Preventing Path Traversal Vulnerabilities**
To secure applications against path traversal, consider the following best practices:

1. **Input Validation:**  
Implement strict input validation to reject suspicious characters such as `../` or any sequences that can traverse directories.

2. **Use Safe API Functions:**  
Use secure file-handling APIs that prevent manipulation of file paths, such as using absolute file paths or whitelisting allowed files.

3. **Sanitize User Inputs:**  
Remove or encode characters like `../` from user input to ensure paths are constructed safely.

4. **Least Privilege Principle:**  
Configure the web server to run with the least privileges possible, ensuring that even if a traversal attack occurs, the attacker cannot access sensitive system files.

## **Conclusion**
File path traversal vulnerabilities can lead to serious consequences, such as exposing sensitive files and system data. In this lab, the simple manipulation of file paths allowed us to access the `/etc/passwd` file. Understanding how to detect and exploit path traversal vulnerabilities is crucial in penetration testing, but equally important is learning how to mitigate these attacks in web applications.

