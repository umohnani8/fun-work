## Fork and clone PacMan repository

```
# first, fork https://github.com/sallyom/mongo-pacman.git

# from the terminal, run
cd /your/git/repos
git clone git@github.com:yourname/mongo-pacman.git
cd mongo-pacman
```

## Check out a new branch

```bash
git checkout -b [new-branch]
```

We'll roughly follow the [README.md](https://github.com/sallyom/mongo-pacman/tree/main/frontend)
for the program's frontend.

## Build PacMan

Use the commands and submit the form listed below.
If you forget to add the `--it` flags to the run command, it will be tricky to stop the container.
If this happens, open another terminal and run `podman stop --all`

```bash
cd frontend
podman build -t docker.io/yourname/pacman:latest -f docker/Dockerfile .
cd ../
```

### Note: For people on windows, create another Containerfile with the following lines and build it
```bash
vi windows-containerfile
```
Add these lines to the windows-containerfile
```
FROM docker.io/yourname/pacman:latest
RUN dnf -y install dos2unix
RUN dos2unix /usr/local/bin/pacman.sh && chmod +x /usr/local/bin/pacman.sh
```
Then run the following command to build your image
```
podman build -t docker.io/yourname/pacman:latest -f windows-containerfile .
```

## Run the built image

Now run the newly built image. `--rm` ensures the container will be cleaned up after you stop the container.
Add the `-d` to run this container in the background.
With `-p 8080:8080` port-forward port 8080 from the running container to port 8080 on your local system.
Name your container `pacman` to easily view the logs.

```bash
podman run --rm -d -it --name pacman -p 8080:8080 docker.io/yourname/pacman:latest
```

You can see the container logs by running:

```bash
$ podman logs pacman
NODE VERSION:
v18.14.2
NPM  VERSION:
9.5.0
run pacman
PACMAN auth_details =  pacman:pacman@
DATABASE URL  mongodb://pacman:pacman@mongodb:27017/pacman
CONNECTING  mongodb://pacman:pacman@mongodb:27017/pacman
Options  { readPreference: 'secondaryPreferred' }
Listening on port 8080
```

To play `PACMAN`, head to your browser at `http://localhost:8080`

* `EXTRA CREDIT`: Make a change (any change!) that is visible when the program is run. Build and push that image.
* `EXTRA EXTRA CREDIT`: Configure the mongo database to keep records of games played with best scores. Explain how you did it.

You can use Podman, Docker, or any other container build tool.

**To clean up your system after running and building the pacman image (after you're done with the assignment):**

```bash
podman stop --all
podman system prune --all
# you'll be asked to confirm by typing 'y'
```

![pacman screen shot](./screenshot.png)
