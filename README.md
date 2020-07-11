# Workbench

Workbench is a collection of configurations for the IT automation tool [Ansible](https://docs.ansible.com/ansible/latest/index.html) which allow me to quickly convert a clean [Ubuntu](https://ubuntu.com/download/desktop) install into my preferred working environment.

In short, this a way to make a brand new laptop work the way I want it to.

This is a highly opinionated project. This is my laptop setup. Take it or leave it. You are free to borrow ideas from it, but I don't intend to support any use cases except for mine and those of personal friends.

Best regards,  
Matt

## Usage

Install [Ubuntu desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop) 20.20 LTS. Then,

```
sudo apt install git python3-venv python3-pip

git clone https://github.com/kangas/workbench.git

cd workbench

pip3 install --user -r requirements.txt

# --user is the default on Debian python3-pip! which means we need...

export PATH=~/.local/bin:$PATH

cd ansible

ansible-playbook -K ./playbooks/laptop/main.yml

# -K is --ask-become-pass
```


## License

   Copyright 2020 Matt Kangas

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
