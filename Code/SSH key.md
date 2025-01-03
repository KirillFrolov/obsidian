<span style="font-size:12px; color:#888888;">Created: 03.01.2025 11:24</span>

Generate an SSH key (if you don't already have one):  
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Follow the prompts to save the key (usually in ~/.ssh/id_rsa).  
Add the SSH key to your GitLab account:  
Copy the SSH key to your clipboard:
cat ~/.ssh/id_rsa.pub
Go to GitLab, navigate to User Settings > SSH Keys, and paste the key.
Clone the repository:  
Get the SSH URL of the repository from GitLab (it will look like git@gitlab.com:username/repository.git).
Use the git clone command with the SSH URL:
git clone git@gitlab.com:username/repository.git
This will clone the repository to your local machine using SSH.