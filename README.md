# aztec_backend

This is a backend for the [ACVM](https://github.com/noir-lang/acvm) which allows proving/verifying ACIR circuits against Aztec Protocol's [Barretenberg](https://github.com/AztecProtocol/barretenberg) library.

## Working on this project

Due to the large number of native dependencies, this project uses [Nix](https://nixos.org/) and [direnv](https://direnv.net/) to streamline the development experience.

### Setting up your environment

For the best experience, please follow these instructions to setup your environment:
1. Install Nix following [their guide](https://nixos.org/download.html) for your operating system
2. Create the file `~/.config/nix/nix.conf` with the contents:
```ini
experimental-features = nix-command
extra-experimental-features = flakes
```
3. Install direnv into your Nix profile by running:
```sh
nix profile install nixpkgs#direnv
```
4. Add direnv to your shell following [their guide](https://direnv.net/docs/hook.html)
5. Restart your shell

### Shell & editor experience

Now that your environment is set up, you can get to work on the project.

1. Clone the repository, such as:
```sh
git clone git@github.com:noir-lang/aztec_backend
```
2. Navigate to the directory:
```sh
cd aztec_backend
```
3. You should see a __direnv error__ because projects aren't allowed by default. Make sure you've reviewed and trust our `.envrc` file, then you need to run:
```sh
direnv allow
```
4. Now, wait awhile for all the native dependencies to be built. This will take some time and direnv will warn you that it is taking a long time, but we just need to let it run.
5. Once you are presented with your prompt again, you can start your editor within the project directory (we recommend [VSCode](https://code.visualstudio.com/)):
```sh
code .
```
6. (Recommended) When launching VSCode for the first time, you should be prompted to install our recommended plugins. We highly recommend installing these for the best development experience.

### Building and testing

Assuming you are using `direnv` to populate your environment, building and testing the project can be done
with the typical `cargo build`, `cargo test`, and `cargo clippy` commands. You'll notice that the `cargo` version matches the version we specify in [flake.nix](./flake.nix), which is 1.66.0 at the time of this writing.

If you want to build the entire project in an isolated sandbox, you can use Nix commands:
1. `nix build .` (or `nix build . -L` for verbose output) to build the project in a Nix sandbox
2. `nix flake check` (or `nix flake check -L` for verbose output) to run clippy and tests in a Nix sandbox

### Building against a different local/remote version of Barretenberg

If you are working on this project and want a different version of Barretenberg (instead of the version this project is pinned against), you'll want to replace the lockfile version with your version. This can be done by running:

```sh
nix flake lock --override-input barretenberg /absolute/path/to/your/barretenberg
```

You can also point at a fork and/or branch on GitHub using:

```sh
nix flake lock --override-input barretenberg github:username/barretenberg/branch_name
```

__Note:__ You don't want to commit the updated lockfile, as it will fail in CI!

### Without direnv

If you have hesitations with using `direnv`, you can launch a subshell with `nix develop` and then launch your editor
from within the subshell. However, if VSCode was already launched in the project directory, the environment won't be updated.

__Advanced:__ If you aren't using `direnv` nor launching your editor within the subshell, you can try to install Barretenberg and other global dependencies the package needs. This is an advanced workflow and likely won't receive support!
