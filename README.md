1. Install WSL 2 [Ubuntu]
2. Install Docker and set it up on your Windows machine.
3. Into Docker Desktop `Settings/Resources/WSL Integration`: Let's configure which WSL 2 distro we want to access Docker from.
    - Disable the check.box "Enable integration with default WSL distro".
    - Enable the selected WSL 2 distro.
4. Create a new directory for your project and navigate to it in your WSL terminal.
    - `mkdir project_name`
5. Create a `.devcontainer` directory into the project directory
6. Create a new file called `devcontainer.json` into the `.devcontainer` directory
7. Create a new file called `Dockerfile` into the `.devcontainer` directory
8. Let's edit the `devcontainer.json` with the following information:
```json
{
    //The name of the development container.
    "name": "nodejs-env", 

    //The name of the Dockerfile that defines the container environment.
    "dockerFile": "Dockerfile", 
    
    //The port that the application inside the container should listen on.
    "appPort": [3000], 
    
    // Command-line arguments to pass when starting the container with `docker run`. 
    //These arguments set the user to "node" and 
    //specify an environment variables file ".devcontainer/devcontainer.env".
    "runArgs": ["--user", "node","--env-file",".devcontainer/devcontainer.env"]
}
```

