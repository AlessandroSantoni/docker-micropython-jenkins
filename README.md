# Docker, Micropython, Jenkins

This repository allows a Jenkinsfile to run a Docker container building a Micropython image for multiple targets. This specific example retrieves an ESP8266 image from the Docker container.
Checkout the [Additional Notes](#additional-notes) section to learn that you could build Micropython images for targets other than the ESP8266.

If you like the project, give a look at the [bottom section](#i-would-love-to-help-more)! :wave:

> **Disclaimer:** I'm not a software engineer nor a computer scientist. I just do it out of passion. Share advice and feedback and I'll make use of it!

## Preparation

First off, make sure to:
1. [Install Docker](https://www.docker.com/)
2. [Install Jenkins](https://www.jenkins.io/doc/book/installing/)

Depending on your platform of choice, installations may vary slightly. I'm using Ubuntu 20.04.2 LTS and it's smooth sailing.

## Setup

To create a Jenkins pipeline building Micropython images for you, execute the following steps.

> **Note:** The steps below will deploy Jenkins on your local machine. Follow the guide provided on Jenkins' website if you want to deploy it elsewhere.

* Launch Jenkins and log in
* Create a new Jenkins "item". Select "pipeline". From within the pipeline settings:
    * Tick "GitHub project" and insert `git@github.com:AlessandroSantoni/docker-micropython-jenkins.git/`
    * No need to select any "Build Triggers" (unless you have a good reason for it, of course!)
    * From the "Pipeline" section, select the following
        * Definition &#8594; Pipeline script from SCM
        * SCM &#8594; Git
        * Repository URL &#8594; `git@github.com:AlessandroSantoni/docker-micropython-jenkins.git`
        * Credentials &#8594; None
        * Branches to build &#8594; `*/main`
        * Script path &#8594; Jenkinsfile


## Building

Following the instructions in the  section [above](#setup) will result in having a pipeline ready to run in your Jenkins.
Therefore, just run it, let it finish and retrieve your artifact upon successful completion of your build!

> **Note:** Feel free to install the add-on "Blue Ocean", if you want your Jenkins to have a more modern "feel" and UI.

## Additional Notes

In truth, this same set of files can be exploited to generate Micropython images for targets other than the ESP8266.
As of now, the file `make_n_extract_esp8266_image.sh` extracts an image from the path `micropython/ports/esp8266/`. However, one could grab images for different "ports" in the respective sub-folders.

To embed in the image one's specific pieces of custom-made codes, it is enough to drop in the folder `micropython/ports/esp8266/modules/` said  pieces of `*.py` scripts. Once the custom image will be deployed on the target, to use your custom scripts just call them with `import`.
Clearly, change the port according to the target your are building an image for.

Finally, if you want your target to automatically run a specific script at startup, drop a `script.py` file in the folder above, and have the `inisetup.py` file call `import script` at its end.

### Pro Tip

You could automate embedding of custom code from specific repositories into your Micropython build. To do so, just add a `RUN` instruction in your Dockerfile, mentioning to clone your repo of interest.
As an example: `RUN git clone git@github.com:<user>/<repo>.git \ && cd <folder> && git checkout <branch>`

Then, still from the Dockerfile, pick whichever file you want to embed in the Micropython image and add another dedicated `RUN` instruction.
As an example: `RUN cp <folder>/<script1>.py micropython/ports/esp8266/modules/ && cp <folder>/<script2>.py micropython/ports/esp8266/modules/`

#### Cloning from a private repository?

If you need an ssh key to clone your repository of interest (as mentioned in the section [above](#pro-tip)), then add the following in your Dockerfile _before_ cloning:
* `RUN mkdir /root/.ssh/`
* `ADD id_rsa /root/.ssh/id_rsa`
* `RUN ssh-keyscan github.com >> /root/.ssh/known_hosts`

> **Note 1:** To import the `id_rsa` in your Docker container generate a pair of public/private key, drop the private in the same folder as the Dockerfile and register the public on your Github profile.

> **Note 2:** Make sure **not to commit** and push your id_rsa key to any repo. In fact, use the `.gitignore` file to make sure the ssh key files are ignored by Git.

## I would love to help more

[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/donate?hosted_button_id=U8DJSW3DP3XQ4)

I've decided to set this repo up, and share it with the community, because I couldn't find a _working_ Docker container to build a Micropython image, let alone integrating it with Jenkins. In fact, there's many more of such little projects I'd like to share with everyone on GitHub.
Getting your donation would mean appreciation for my work, but would also allow me to dedicate more time into these projects.
_I have only a limited amount of time-off from work, you know..._ :blush:
