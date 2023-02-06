* Install WSL 2 [Ubuntu]
* Install Docker and set it up on your Windows machine.
* Into Docker Desktop `Settings/Resources/WSL Integration`: Let's configure which WSL 2 distro we want to access Docker from.
  - Disable the check.box "Enable integration with default WSL distro".
  - Enable the selected WSL 2 distro.
* Create a new directory for your project and navigate to it in your WSL terminal.
  - `mkdir project_name`
* Create a `.devcontainer` directory into the project directory
* Create a new file called `devcontainer.json` into the `.devcontainer` directory
* Create a new file called `Dockerfile` into the `.devcontainer` directory
* Let's edit the `devcontainer.json` with the following information:
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
* Install and setup Git:
  - Git comes installed with the base node image <!-- - Install `git-all` in the Dockerfile by adding ther line `git-all \` before `&& rm -rf /var/lib/apt/lists/*`. -->
  - Create a `onCreateCommands` directory to save some bash scripts.
  - Create a `configure-git.sh` file to write the script to setup Git.
    ```bash
      #!/bin/sh

      # Setting up Git
      git config --global user.name "$NAME"
      git config --global user.email $EMAIL
      git config --global core.editor "code --wait"
      echo "Git configured successfully!"
    ```
  - Create a `onCreateCommand.sh` file to call all the scripts from the `"onCreateCommand":` key in `devcontainer.json`.
    ```bash
      #!/bin/sh

      # Setting up Git
      sh .devcontainer/onCreateCommands/configure-git.sh
    ```
  - Edit `devcontainer.json` to call the `onCreateCommand.sh` file.
  ```json
  "onCreateCommand":"sh .devcontainer/onCreateCommands/onCreateCommand.sh"
  ```

* Configure Zsh and Oh_My_Zsh

  - Install Zsh in the `Dockerfile` by adding ther line `zsh \` before `&& rm -rf /var/lib/apt/lists/*`. Then, at the end of the file change the user to `node` and install Oh_My_Zsh as well as its plugins.
  ```Dockerfile
    USER node

    # Oh My ZSH - INSTALLATION
    RUN cd ~ && wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh && sh install.sh
    RUN /bin/zsh ~/.zshrc
    RUN cd ~ .oh-my-zsh/custom/plugins/ && git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    RUN cd ~ .oh-my-zsh/custom/plugins/ && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    RUN cp -r ~/.oh-my-zsh/plugins/fzf ~/.oh-my-zsh/custom/plugins
  ```
  - Create a `config` folder in `.devcontainer` directory
  - Create a file `.zshrc` in the `config` folder with the following info.
  ```bash
    export ZSH="$HOME/.oh-my-zsh"

    ZSH_THEME="robbyrussell"

    plugins=(
      git
      zsh-autosuggestions
      zsh-syntax-highlighting
    )

    source $ZSH/oh-my-zsh.sh

    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
  ```
  
  - Copy this `.zshrc` file into the container creating another bash script file `configure-zsh.sh` 
    ```bash
      #!/bin/sh

      # Copying .zshrc to container
      cp -f ./.devcontainer/config/.zshrc ~/
      echo ".zshrc file copied into container successfully!"
    ```

    and call it from `onCreateCommand.sh` file like this:

    ```bash
      # Copying .zshrc to container
      sh .devcontainer/onCreateCommands/configure-zsh.sh
    ```