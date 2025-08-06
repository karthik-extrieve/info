### ✅ Local SSL Certificate Setup Guide

#### **1. Create a Self-Signed SSL Certificate**

* Open **PowerShell as Administrator**.
* Run the following command, replacing `"domainname.com"` with your desired domain:

  ```powershell
  New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName "domainname.com" -FriendlyName "SSL Certificate"
  ```

---

#### **2. Allow HTTPS Port in Windows Firewall**

* To allow access via the browser, you must open a port (such as **443**, **8443**, or **9443**) in the firewall.
* Run this command (replace `443` with your desired port if needed):

  ```cmd
  netsh advfirewall firewall add rule name="Allow HTTPS 443" dir=in action=allow protocol=TCP localport=443
  ```

---

#### **3. Move Certificate to Trusted Root Store**

* Open **Manage User Certificates** (search for it in the Start menu).
* Navigate to:

  ```
  Intermediate Certification Authorities > Certificates
  ```
* Locate the certificate with your domain name:

  * Right-click it and choose **Cut**.
* Now go to:

  ```
  Trusted Root Certification Authorities > Certificates
  ```

  * Right-click and choose **Paste**.
* Refresh the list and close the Certificate Manager.

---

#### **4. Map Domain to Localhost**

* Navigate to the following path:

  ```
  C:\Windows\System32\drivers\etc
  ```
* Open the `hosts` file in **Notepad as Administrator**.
* At the end of the file, add the following line:

  ```
  127.0.0.1 domainname.com
  ```
* Save and close the file.
* Reopen the file to confirm the changes were saved.

---

#### **5. Configure SSL Binding in IIS**

* Open **IIS Manager** and select the desired site.
* Click on **Bindings** and edit/add an entry:

  * Set the **port** you opened in Step 2.
  * Enter your **domain name** (`domainname.com`) in the **Host name** field.
  * Choose the correct **SSL certificate** from the dropdown.
  * Leave **Require Server Name Indication** unchecked.
* Click **OK**, then run:

  ```cmd
  iisreset
  ```
* Now you can visit your site.
