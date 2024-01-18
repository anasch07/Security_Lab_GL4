

1. **Prerequisites**

   Before proceeding, ensure that you have the following prerequisites:
    - A server with a static IP address.
    - Ubuntu or a compatible Linux distribution installed on your server.
    - Administrative privileges on the server.

2. **Setting up Hostname**

    - Check your server's IP address using the `ifconfig` command.
    - Set the hostname and Fully Qualified Domain Name (FQDN) for your server:

      ```bash
      hostnamectl set-hostname server
      ```

    - Modify the `/etc/hosts` file to add the FQDN entry:

      ```bash
      sudo vim /etc/hosts
      ```

      Add the following line:
      ```
      127.0.1.1 server.insat.tn server
      ```

3. **Installing OpenLDAP**

    - Install the OpenLDAP package:

      ```bash
      sudo apt install slapd ldap-utils -y
      ```

    - Start the OpenLDAP service:

      ```bash
      sudo service slapd start
      ```

4. **Installing Apache and LDAP Account Manager (LAM)**

    - Install Apache web server and PHP modules:

      ```bash
      sudo apt install apache2 php php-cgi libapache2-mod-php php-mbstring php-common php-pear -y
      ```

    - Install LDAP Account Manager (LAM):

      ```bash
      sudo apt install ldap-account-manager -y
      ```

    - Enable the PHP-CGI extension:

      ```bash
      sudo a2enconf php*-cgi
      ```

   ![Untitled](files/Untitled.png)
   ![Different digests but both refer to word “Bonjour”](images/Untitled%201.png)
   ![Untitled](files/Untitled%201.png)
   ![Untitled](files/Untitled%202.png)
   ![Untitled](files/Untitled%203.png)
   ![Untitled](files/Untitled%204.png)
   ![Untitled](files/Untitled%205.png)

5. **Adding SSL Certificates**

    - Create an LDIF file named `ssl_ldap.ldif` to add SSL certificates (details not provided in this document).

    - Execute the following command to add SSL certificates:

      ```bash
      ldapmodify -x -D cn=admin,dc=insat,dc=tn -W -f ssl_ldap.ldif
      ```

6. **Configuring LDAPS**

    - Modify the `/etc/default/slapd` file and configure LDAPS (details not provided in this document).

    - Modify the `/etc/ldap/ldap.conf` file for LDAPS (details not provided in this document).

7. **Configuring Client Machine**

    - Modify the `/etc/hosts` file on the client machine to include the server's IP and FQDN:

      ```bash
      sudo vim /etc/hosts
      ```

      Add an entry like this:

      ```
      SERVER_IP server.insat.tn server
      ```

    - You should now be able to ping the server from the client machine.

8. **Testing LDAP and LDAPS**

    - Install LDAP client packages on the client machine:

      ```bash
      sudo apt install libnss-ldap libpam-ldap ldap-utils nscd -y
      ```

    - Configure `/etc/nsswitch.conf` for LDAP (details not provided in this document).

    - Add the necessary line to `/etc/pam.d/common-session` for LDAP authentication (details not provided in this document).

    - You can now perform LDAP queries and log in from the client machine.

    - To test LDAPS, execute the following command:

      ```bash
      ldapsearch -x -H ldaps://server.insat.tn -b dc=insat,dc=tn '(uid=user1)'
      ```
