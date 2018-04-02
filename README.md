Parallelepiped (p13d) Piper
===========================

It's a playground for learning about the inner workings of k8s.


Runtime Requirements
--------------------

* Ansible v2.4.2.0 or higher


Development Requirements
------------------------

* All Runtime Requirements
* Virtualbox v5.1.0 or later
* Vagrant v2.0.0 or later
* Packer v1.0.0 or later (For building the Vagrant box)
* sshpass (For building the Vagrant box)


For MacOS
---------

We only want to work with GNU tools in these parts...

* `brew install gnu-sed --with-default-names`
* `brew install grep --with-default-names`


What the Files and Directories Mean
-----------------------------------

```
./
├── README.md         # You're looking at it!
│
├── baker/            # Packer template and files needed to build the local
│                     # Vagrant box.
│
├── configurator/     # Ansible playbooks and roles needed to configure the
│                     # p13dp environment. These playbooks are parameterized
│                     # such that you can use it to configure ANY p13dp environment
│                     # See examples/localdev for a sample config.
│
├── examples/         # Contains example p13dp cluster configs. See
│   │                 # "What is a Cluster Config?" below for a definition.
│   │
│   ├── localdev/     # Use this cluster config when developing p13d
│   │                 # on your local machine.
│   │
│   └── simple/       # The simplest example cluster config. Copy this to some
│                     # location outside of this repo and play around with it.
│
├── lib/              # Libraries shared by various scripts in this repo.
│
├── script/           # Scripts used to manage a p13dp cluster.
│
└── tmp/              # Temporary files generated while developing p13dp.
```


What is a Cluster Config?
-------------------------

It's a directory that contains certain files and subdirectories that
p13dp needs to configure an environment. See the examples/ directory
for some, well, examples.


Local Development Quick Start
-----------------------------

Make sure you have a base Vagrant box.

```
baker/create-box
```

You typically only need to do the above once. Unless, of course, if you're
modifying stuff under the `baker/` directory. In that case, re-build the
box as needed!

Next, create your `vars.yml` via the following command

```
cp examples/localdev/vars.yml.example examples/localdev/vars.yml
```

Modify the file you just created.

Next, initialize the vault file where you're going to store passwords and
other sensitive information:

```
script/init-vault examples/localdev
```

Modify the vault file you just created via:

```
script/edit-vault examples/localdev
```

Now you can finally bring up your development cluster! How about that, huh?
Give yourself a pat on the back. You are awesome!

```
script/start examples/localdev
```

Work on the playbook and roles under `configurator/` and then apply the changes:

```
script/configure examples/localdev
```

Want to limit execution to a task or a set of tasks? Add a tag to the
task (or an include\_tasks) entry and then run:

```
script/configure examples/localdev --tags TAGHERE
```

Want to SSH to your kube cluster? A couple of entries should've been added
to your `~/.ssh/config` file allowing you to just run:

```
ssh kubemaster
ssh kubeworker
```

When you're done with the machine:

```
script/destroy examples/localdev
```

If you don't want to have to confirm the destroy command all the time,
set the `force` env variable to whatever value:

```
force=t script/destroy examples/localdev
```
