# Devops challenge

The Devops Challenge
http://devopschallenge.co/challenge

"It's your first day at Devz Incorporated. You are hired as a DevOps Professional. You will automate everything, increase deployments, decrease holding time in the SDLC, and prosper a better relationship with the developers of Devz Incorporated. Expectations are very high, as you are the only person having an understanding of DevOps ... or haven't you?"

First encounter!
o-oh

First day on the job! After a quick introduction to the team, you have been given a desk. You barely set-up your laptop when one of the lead Devs, a young guy named Jonathan, comes standing next to you...

Jonathan: "Hey new DevOps guy! We're already behind schedule, and we really need to deploy our latest beta version of our app. Can you quickly fix us a server where we can deploy the app?"

You: "No worries, I'll get started with it immediately!"

Jonathan: "Sure, we're using Digital Ocean here, I'll give you the credentials"

First Task! Create new SSH keys so Jonathan will be able to log in to the server you'll be deploying. Paste the public SSH key in the text area below. Check the hints on the right if you're stuck.

"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzcndRtj9Ft5fAqZZ4X9IRDI+un7KRDdyvbjCfu47AFw04BfhmgGrOLUsa/vAX28gIZQiffNmJ+p61ah1I4jLlkvey0IAXshFjEBV2RLR9H1Blbl4kX+1NqONZHi/m2SN7AoLOoYXeAK0MioJ6W/dFcr1qh40vh/uro15xDDNliuoPJgPsmFxiBMEG7WQyAILRkURyijftZTNtWOQj5iwdASFW9qLJrCFoAIAwuSPYcQEkj2iBTvezlVKpLxa6ldXJCFJWUZGAyaVgWCQlyMW29ucQ/uqRFa7WzAIWhWNN+zD+BeSKWqVu3tcgSfOWXCfu0RmXPJF1+omzCrU82Gxb rsa-key-20220603"


Let's spin up some machines
The DevOps way

Now that you have generated the ssh keys. Let's spin up a droplet (server instance) on Digital Ocean. Everything you do on the infrastructure should be repeatable, testable, and version controlled. Let's use Terraform to build the infrastructure in code. Have a look at the documentation how to launch instances on Digital Ocean. I already provided you with a providers.tf file, you need to write the code to launch an instance that has the public key installed on it. You can assume the public key is located in /data/key.pub.

resource "digitalocean_ssh_key" "default" {
  name = "Key1"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzcndRtj9Ft5fAqZZ4X9IRDI+un7KRDdyvbjCfu47AFw04BfhmgGrOLUsa/vAX28gIZQiffNmJ+p61ah1I4jLlkvey0IAXshFjEBV2RLR9H1Blbl4kX+1NqONZHi/m2SN7AoLOoYXeAK0MioJ6W/dFcr1qh40vh/uro15xDDNliuoPJgPsmFxiBMEG7WQyAILRkURyijftZTNtWOQj5iwdASFW9qLJrCFoAIAwuSPYcQEkj2iBTvezlVKpLxa6ldXJCFJWUZGAyaVgWCQlyMW29ucQ/uqRFa7WzAIWhWNN+zD+BeSKWqVu3tcgSfOWXCfu0RmXPJF1+omzCrU82Gxb rsa-key-20220603"
}
resource "digitalocean_droplet" "web" {
  image  = "ubuntu-20-04-x64"
  name   = "web-1"
  region = "nyc2"
  size   = "s-1vcpu-1gb"
  ssh_keys = ["$/data/key.pub"]
}

#output
Refreshing Terraform state prior to plan...


The Terraform execution plan has been generated and is shown below.
Resources are shown in alphabetical order for quick scanning. Green resources
will be created (or destroyed and then created if an existing resource
exists), yellow resources are being changed in-place, and red resources
will be destroyed.

Note: You didn't specify an "-out" parameter to save this plan, so when
"apply" is called, Terraform can't guarantee this is what will execute.

+ digitalocean_droplet.web
    image:                "" => "ubuntu-20-04-x64"
    ipv4_address:         "" => "<computed>"
    ipv4_address_private: "" => "<computed>"
    ipv6_address:         "" => "<computed>"
    ipv6_address_private: "" => "<computed>"
    locked:               "" => "<computed>"
    name:                 "" => "web-1"
    region:               "" => "nyc2"
    size:                 "" => "s-1vcpu-1gb"
    ssh_keys.#:           "" => "1"
    ssh_keys.0:           "" => "$/data/key.pub"
    status:               "" => "<computed>"

+ digitalocean_ssh_key.default
    fingerprint: "" => "<computed>"
    name:        "" => "Terraform Key"
    public_key:  "" => "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzcndRtj9Ft5fAqZZ4X9IRDI+un7KRDdyvbjCfu47AFw04BfhmgGrOLUsa/vAX28gIZQiffNmJ+p61ah1I4jLlkvey0IAXshFjEBV2RLR9H1Blbl4kX+1NqONZHi/m2SN7AoLOoYXeAK0MioJ6W/dFcr1qh40vh/uro15xDDNliuoPJgPsmFxiBMEG7WQyAILRkURyijftZTNtWOQj5iwdASFW9qLJrCFoAIAwuSPYcQEkj2iBTvezlVKpLxa6ldXJCFJWUZGAyaVgWCQlyMW29ucQ/uqRFa7WzAIWhWNN+zD+BeSKWqVu3tcgSfOWXCfu0RmXPJF1+omzCrU82Gxb rsa-key-20220603"
Plan: 2 to add, 0 to change, 0 to destroy.
  
  Deploy!
Infra ready, let's deploy!

You: "Hi Jonathan! I spun up a machine on Digital Ocean. Can you check whether you can log in?"

Jonathan: "That's about time! We want to launch our new app and the only thing blocking us is this infrastructure thing. Gimme a sec, I'll try it out"
  
  Jonathan: "OMG! You didn't install node! Our app is written in node, it's not going to work if you don't install it!"

You will need to install NodeJS. Jonathan didn't specify any version, so you can just use the one that's in the distribution repository. Remember that we want to make everything reproducable and written in code. Let's use Ansible to install any dependencies on the server. You will need to write a playbook that installs nodejs, so Jonathan can run the app.

Operating System
Select your prefered OS (your ansible script will need to use apt or rpm): 
Debian based (apt-get / dpkg)

inventory
[appserver]
198.51.100.1
nodejs-playbook.yaml

  
#Stopped here.
