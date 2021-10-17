# Platform Hardening & Software Patch Management

**System hardening** is the process of ensuring that a system only runs the services and functions that it is supposed to. That is, removing any unnecessary applications and services that do not fall within the system's planned use. By having fewer programs and less functionality, a system has less risk of affecting operational issues, misconfigurations, incompatibilities and possible compromise. This **improved security** reduces the viable attack surfaces, which can lower the statistical likelihood of data breaches, unauthorized access, system hacking or malware. Additionally, system hardening paves the way for a simplified process for compliance and auditability, meaning a cleaner environment leads to quick and easy indentification of all the moving parts helping with transparency.

Software patch management is the process of ensuring that all services and applications running within a given system has been updated with the latest security patches in order to make sure that there are as few security holes as possible. Many data breaches are due to a company's lack of adherence to this rule.

Some major security configurations that one must be aware of are:
- **Disabling unnecessary services and applications**<br/>
Depending on your security architecture, it may be more prudent to strip applications from a system that has already been set up. Otherwise, it is always good practice to start with a minimum installation of the given OS and installing only applications and services directly related to the system's planned use. It is important to note that service and application configuration must be done with security in mind.
- **Change system defaults (e.g. credentials, service ports, permissions)**<br/>
It is crucial that system defaults are changed, meaning default credentials for applications and services as well as service ports used. Additionally, ensuring that proper permissions are set for vital components of the system directory.
- **Configure and install security applications and anti-virus/malware**<br/>
Depending on the platform installed, it may be crucial to install an anti-virus/malware program ensuring detection and protection from known threats plagueging the web. Additionally, extra security steps should be taken when attempting to access a given system, such as installing multi-factor authentication.

In this step, we will go through with creating a new user and setting up multi-factor authentication access to the platform.

1. First, we need to create a user with Superuser permissions to act as the admin account. For this example we will use the username `master` and password `class`. Creating a new sudo user can be done with the command `useradd -m master && adduser master sudo`. We will then need to create a password for the newly created sudo user with command `passwd master`, you may use the password `class` or your own. If you forget the password, you may use the root account with the same password command to reset it. After creating a new user and setting the password, you can now login with the command `login master` and enter the password set.

2. Now that we have an admin user, we can begin with installing a form of multi-factor authentication that is time-based. First we will need to ensure that the apt package repository is updated, then we can install the multi-factory authentication application in one command, `sudo apt update && sudo apt install libpam-google-authenticator -y`.

3. Once google-authenticator has been installed, you can begin setting up TOTP codes for each user by logging into their respective accounts and running the `google-authenticator` command. Try running this command in the terminal now.

4. You will be presented with a set of questions, it is important to read and properly configure these setttings based on your specific use-case, however, for this scenario, following this configuration will suffice. Answer yes to enabling time-based authentication tokens. You will then be presented a QR code that can be scanned with your chosen MFA application, if the QR code doesn't work, you may attempt entering the key maunally. Then select yes for updating the `~/.goole_authenticator` file, otherwise OTP will not work. Then yes to disallow multiple uses of the same token. No to generating every 30 seconds and finally, yes to the last question regarding rate limiting.

5. At this point, you have completed the initial configuration for setting up TOTP for the user master. Now we need to configure two more files to ensure that this works. First navigate to the `/etc/pam.d/sshd` file and add these two lines at the bottom of the file, `auth required pam_google_authenticator.so` and `auth required pam_permit.so` These two lines will allow the PAM module to be aware of the google-authenticator installation.

6. Then navigate to the `/etc/ssh/sshd_config` file and change these two lines to `yes`; `ChallengeResponseAuthentication yes` and `PasswordAuthentication yes`. After this, at the bottom of the file, add this line to enable both password and TOTP authentication via SSH, `AuthenticationMethods keyboard-interactive`

7. At this point, you can restart the ssh daemon with `sudo systemctl restart sshd` and try logging into the master account. Open a new terminal tab in Katacoda, and type `ssh master@localhost`. You should be prompted with the password challenge, and then the TOTP code.

Ensuring multi-factor authentication hardens your given platform from the most trivial of exploits, specially regarding brute-force techniques.
