### GIT setup

#### Installation
[Homebrew installation guide](https://phoenixnap.com/kb/install-homebrew-on-mac)
[GIT installer download](https://git-scm.com/downloads)


#### Initial configuration
```
git -v						#Verify installation
git config user.name "Alex Petrushchak" 	#Add name
git config user.name				#View name
git config user.email "myEmail@gmail.com"	#Add Email
git config user.email				#Verify email
```

#### SSH Authentication
[GitHub documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

* Generate key (MAC commands)
```
ssh-keygen -t ed25519 -C "myEmail@gmail.com"
```

* Start the ssh-agent in the background.
```
$ eval "$(ssh-agent -s)"
```
Output: > Agent pid 59566

* Add ssh key to agent
```
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```
* Add the SSH public key to your account on GitHub.



