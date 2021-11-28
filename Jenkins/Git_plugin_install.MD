# Configure Git pulgin on Jenkins
Git is one of the most popular tools for version control system. you can pull code from git repositories using jenkins if you use github plugin. 


#### Prerequisites
1. Jenkins server 

#### Install Git on Jenkins server
1. Install git packages on jenkins server
   ```sh
   yum install git -y
   ```

#### Setup Git on jenkins console
- Install git (GitHub) plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `GitHub`

- Configure git path
     Only add name github abd setup path
     In the latest version of Jenkins, we should provide the full path of git. Please change the "path to Git executable" to /usr/bin/git rather than just "git"
  - `Manage Jenkins` > `Global Tool Configuration` > `git`


