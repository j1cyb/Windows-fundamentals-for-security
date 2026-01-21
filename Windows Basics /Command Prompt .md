 Command Prompt (CMD)
 
What is CMD?
 CMD (Command Prompt) is a command-line interface in Windows
 that allows users to interact directly with the operating
 system using text-based commands instead of a GUI.

 
 It usually appears like this:
 C:\Users\Username>

 This is called the:
 Current Working Directory
 which is the folder where commands are executed.


 Why use CMD?

 - Easy to use
 - Commands execute directly on the system
 - Provides faster and more precise control over:
     * Files
     * Users
     * Network
     * System configuration

  CMD vs PowerShell


 CMD:
  - Older tool
  - Uses text-based commands
  - Limited functionality
  - Used for basic and quick tasks
  - Security-wise: commonly used in simple attacks

 PowerShell:
  - Newer and more advanced
  - Uses commands and objects (Objects)
  - Very powerful
  - Used for automation, system administration, and scripting
  - Security-wise: used in advanced attacks

  Summary:
  CMD        -> Basic and fast tool
  PowerShell -> Powerful tool for administration, offense, and defense



  Why is CMD important for security?

  - Available by default on all Windows systems (no installation needed)
  - Used by:
    * Attackers to control the system
    * Defenders for investigation and analysis
   
  - Many malicious programs:
    * Execute CMD commands in the background
    * Rely on CMD for movement inside the system

 Note:
    Any CMD activity = direct system activity




 Basic CMD Commands


 cd - Change Directory
 
 Used to navigate between folders
 

 Examples:
 
         cd Files
         
         cd ..
         
         cd \
         
         cd C:\Windows

  Security perspective:
  
  - Attackers move through directories to search for valuable files
  - Frequent access to sensitive folders (System32, Users) is suspicious




 dir - List files and folders

 dir
 dir C:\Windows\System32

 Security perspective:
  - Used for system reconnaissance
  - Repeated use in sensitive directories indicates suspicious behavior


  type - Display file contents
  
  type file.txt

 Security perspective:
 
 - Reading configuration files
 - Reading stored credentials
 - Reading scripts


 echo - Create / Modify files
 
 Create a file:
      echo hello > file.txt

 Append to a file:
      echo test >> file.txt

 Security perspective:
  - Creating malicious scripts
  - Creating backdoors
  - Frequent use of > or >> is a red flag



 
  System Information Commands
  
whoami

  whoami
  whoami /all

Security perspective:
  - Usually the first command executed by an attacker
  - Used to determine:
    * Current user
    * Privileges
    * Administrative access

  ipconfig
  
ipconfig
ipconfig /all

  Security perspective:
  - Understanding the internal network
  - Identifying the default gateway
  - Checking domain membership

  arp
  
 arp -a

  Security perspective:
  - Discovering other devices on the network
  - Supporting lateral movement


   set / echo %VAR%
   
 set
 echo %APPDATA%

  Security perspective:
  - Malware often stores files in %APPDATA%
  - Environment variables are used to hide paths
  
  nslookup
  
 nslookup example.com
 
 nslookup -type=mx example.com

 Security perspective:
 - Domain reconnaissance
 - Identifying mail servers
 - External recon



 Common CMD Commands Used Maliciously

 cd / dir      -> System reconnaissance
 whoami        -> Privilege enumeration
 echo >        -> Script creation
 type          -> Reading sensitive files
 ipconfig/arp  -> Network reconnaissance
 nslookup      -> External reconnaissance
 set           -> Hidden path discovery



 Detecting Suspicious CMD Usage (Defensive View)

 Red flags:
  - CMD launched without a clear reason
  - CMD executed from:
    * Word
    * Excel
    * PDF
  - Frequent use of redirection (> or >>)
  - CMD suddenly running as Administrator
  - Network commands on a standard user machine
  - CMD running in the background (Task Manager)



   
  Important CMD Flags (Parameters)
   

  A flag is an additional option used with a command
  to modify its behavior or provide more detailed output

  Example:
ipconfig /all



   
  General Flags
 command /?

 Security perspective:
  - Used by attackers to explore command capabilities
  - Repeated usage may indicate reconnaissance



   dir Flags
   
 dir /a      // Show hidden files
 dir /s      // Recursive listing
 dir /b      // Bare output

  Security perspective:
  - Discover hidden malware
  - Perform wide reconnaissance
  - Enable scripting



   cd Flag
   
 cd /d D:\Tools

  Security perspective:
  - Switching drives during an attack



   ipconfig Flags
   
 ipconfig /all
ipconfig /release
ipconfig /renew

  Security perspective:
  - Gathering detailed network info
  - Temporarily disrupting network connectivity



   arp Flags
 arp -a
 arp -d *

 Security perspective:
 - Network discovery
 - Attempting to erase traces



 Security Summary

  CMD is a double-edged sword
  - Attackers use it for reconnaissance, control, and malware creation
  - Defenders monitor it using:
    * Logs
    * Command-line auditing
    * EDR solutions
