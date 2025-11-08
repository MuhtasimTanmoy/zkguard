# Dev Container for the Noir subproject

This devcontainer config creates a VS Code development container scoped to the `noir/` folder of the repository.

What it includes
- Ubuntu 22.04 base
- Rust (via rustup - stable)
- Python3 + pip
- Common build tools: build-essential, cmake, clang, libssl-dev, pkg-config

Post-create steps
- Installs Python dependencies from `noir/requirements.txt` (user-local install)
- Attempts to install `nargo` (Noir) via `cargo install --git ...` (may take several minutes)

Notes
- Barretenberg (`bb`) is not installed by default (heavy, native build). If you need to run full prove/verify flows inside the container, install `bb` following the Noir project instructions or run it on the host.
- If `cargo install nargo` fails inside the container, check the logs; missing native deps can be installed by adding them to the Dockerfile.

How to use
1. From VS Code, install the Remote - Containers extension.
2. Open the repository in VS Code.
3. Click the green remote icon in the lower-left and choose "Open Folder in Container..." and select the `noir/` folder.

docker build -f .devcontainer/Dockerfile -t zkguard-noir .