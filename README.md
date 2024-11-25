
# Easy start

1. [Ensure you have Yarn installed.](https://yarnpkg.com)
2. Run `yarn`
3. :boom: start creating!

## Commands
- `yarn dev` - runs webpack on `https://localhost:8080/` you need to open the link and allow insecured certificates to be able to run it on looker
- `yarn build` - Compiles the code in `/src` to `/dist` via webpack
- `yarn clean` - to delete current `/dist`

# Custom Looker Visualizations with Yarn Dev and VSCode Dev Container
## Introduction

This repository is a fork of Looker's [custom_visualizations_v2](https://github.com/looker-open-source/custom_visualizations_v2) and enhances it by:

- Adding **Yarn development capabilities** with `yarn dev`.
- Providing a working sample visualization in `index.tsx`.
- Including a **VSCode Dev Container** setup for seamless development.

Our goal is to streamline the process of developing custom visualizations for Looker using React and TypeScript, equipped with hot-reloading and modern development tools.

---

**Note on Support**

The visualizations provided in this repository are intended to serve as examples. Looker's support team does not troubleshoot issues relating to these example visualizations or your custom visualization code. Supported visualizations are downloadable through the [Looker Marketplace](https://docs.looker.com/data-modeling/marketplace).

---

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Development Setup](#development-setup)
  - [Running the Development Server](#running-the-development-server)
  - [Configuring Looker](#configuring-looker)
  - [Using the Sample Visualization](#using-the-sample-visualization)
- [Commands](#commands)
- [VSCode Dev Container](#vscode-dev-container)
  - [Benefits](#benefits)
  - [Usage](#usage)
- [Usage](#usage)
  - [Running the Visualization in Looker](#running-the-visualization-in-looker)
  - [Building for Production](#building-for-production)
- [Example](#example)
- [Troubleshooting](#troubleshooting)
  - [Common Issues](#common-issues)
  - [Tips](#tips)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Features

- **Yarn Dev Server with Hot Reloading**: Run a development server with live reloading using `yarn dev`.
- **Sample Visualization**: A fully functional sample visualization in `index.tsx` to kickstart your development.
- **VSCode Dev Container Support**: Develop inside a containerized environment with Node.js 23, ensuring consistency across development environments.
- **Node.js 19+ Compatibility**: Compatible with Node.js version 19 and above.

## Getting Started

### Prerequisites

- **Node.js**: Version 19 or higher. [Download Node.js](https://nodejs.org/en/download/).
- **Yarn**: Package manager for Node.js. [Install Yarn](https://yarnpkg.com/getting-started/install).
- **Visual Studio Code**: [Download VSCode](https://code.visualstudio.com/).
- **Dev Containers Extension**: Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) for VSCode.
- **Docker**: Required for the VSCode dev container. [Install Docker](https://docs.docker.com/get-docker/).

### Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/datadriven-works/looker_custom_visualizations_v2
   cd looker_custom_visualizations_v2
   ```

2. **Open in VSCode**

   Open the repository folder in Visual Studio Code.

3. **Open in Dev Container**

   - When prompted by VSCode, click on "Reopen in Container".
   - Alternatively, press `F1` and select `Dev Containers: Reopen in Container`.

4. **Install Dependencies**

   Inside the dev container terminal, run:

   ```bash
   yarn
   ```

## Development Setup

### Running the Development Server

Start the development server with:

```bash
yarn dev
```

The development server will start at `https://localhost:8080/`. Since it uses a self-signed SSL certificate, you'll need to accept the security warning in your browser.

**Accepting the Self-Signed SSL Certificate:**

1. Open `https://localhost:8080/` in your web browser.
2. Proceed to the advanced settings.
3. Accept the security warning to trust the self-signed certificate.

### Configuring Looker

To use the custom visualization in Looker, you need to add it to your LookML manifest file (`manifest.lkml`):

```lkml
visualization: {
  id: "custom_viz_localhost"
  label: ". Dev Custom Viz"
  url: "https://localhost:8080/bundle.js"
}
```

**Steps:**

1. **Edit Your Manifest File**

   - Locate your `manifest.lkml` file in your Looker project.
   - Add the visualization block shown above.

2. **Ensure Looker Can Access Localhost**

   - Since Looker may not be able to access `localhost` directly, you might need to use a tool like `ngrok` to expose your local server to the internet.
   - Alternatively, if your Looker instance is running locally, you can proceed.

3. **Accept Insecure Content**

   - In your browser, ensure that insecure content is allowed for your Looker domain to load the visualization from `https://localhost:8080/`.

4. **Refresh Looker**

   - Refresh your Looker instance to load the new visualization.

For more information, refer to the [Looker documentation on visualization manifests](https://cloud.google.com/looker/docs/reference/param-manifest-visualization).

### Using the Sample Visualization

The sample visualization demonstrates how to create a custom visualization using React and TypeScript.

```typescript
// @ts-ignore
looker.plugins.visualizations.add({
  id: "react_test",
  label: "React Test",
  options: {
    font_size: {
      type: "string",
      label: "Font Size",
      values: [{ Large: "large" }, { Small: "small" }],
      display: "radio",
      default: "large",
    },
  },
  create: function (element, config) {
    ...
  },
  updateAsync: function (data, element, config, queryResponse, details, done) {
    this.clearErrors();

    if (queryResponse.fields.dimensions.length == 0) {
      this.addError({
        title: "No Dimensions",
        message: "This chart requires dimensions.",
      });
      return;
    }

    this.chart = this.root.render(
      <AdvancedTable queryfields={config["query_fields"]} dataLooker={data} />
    );

    done();
  },
});
```

This code initializes a visualization that displays data using React components.

## Commands

- `yarn dev` - Runs the webpack development server at `https://localhost:8080/`.
- `yarn build` - Compiles the code in `/src` to `/dist` via webpack.
- `yarn clean` - Deletes the `/dist` directory.
- `yarn lint` - Runs ESLint across the codebase.
- `yarn lint-fix` - Attempts to fix any linter errors automatically.

## VSCode Dev Container

This repository includes a pre-configured dev container for VSCode, defined in `.devcontainer/devcontainer.json`.

### Benefits

- **Consistent Environment**: All developers use the same Node.js version and dependencies.
- **Pre-installed Extensions**: Includes useful VSCode extensions for development.
- **Automatic Setup**: Simply open the project in VSCode and reopen in the container.

### Usage

1. **Install the Dev Containers Extension**

   - Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) in VSCode.

2. **Reopen in Container**

   - Open the project in VSCode.
   - When prompted, click "Reopen in Container".
   - If not prompted, press `F1`, type `Dev Containers: Reopen in Container`, and select it.

3. **Start Developing**

   - The container will build and start automatically.
   - All dependencies and extensions are pre-configured.

## Usage

### Running the Visualization in Looker

After configuring the manifest and accepting the SSL certificate, you can use the custom visualization in your Looker dashboards.

1. **Add a Visualization to Your Dashboard**

   - In Looker, create a new dashboard or edit an existing one.
   - Add a new tile and select the custom visualization labeled ". Dev Custom Viz".

2. **Select Data**

   - Choose the dimensions and measures for your visualization.
   - Run the query to see the data.

3. **Interact with the Visualization**

   - The visualization should display using the React components defined in your code.
   - Any changes to the code will automatically reload in Looker.

### Building for Production

To build the visualization for production deployment, run:

```bash
yarn build
```

This will compile the code into the `dist/` directory. You can host `bundle.js` from a secure, publicly accessible URL and update your LookML manifest to point to it.

## Troubleshooting

### Common Issues

- **Visualization Not Appearing in Looker**: Ensure that the manifest is correctly configured and that Looker can access the URL specified.
- **SSL Certificate Errors**: You must accept the self-signed SSL certificate by visiting `https://localhost:8080/` in your browser.
- **Port Not Accessible**: Make sure that port 8080 is open and not blocked by a firewall.

### Tips

- Use `ngrok` to expose your local development server if Looker cannot access `localhost`.
- Check the browser console for any error messages.
- Ensure Docker is running if using the Dev Container.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

### Steps to Contribute

1. **Fork the Repository**

   Click the "Fork" button at the top right of the repository page.

2. **Create a New Branch**

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make Your Changes**

   Implement your feature or fix.

4. **Run Linter**

   ```bash
   yarn lint
   ```

5. **Commit and Push**

   ```bash
   git commit -m "Add your commit message"
   git push origin feature/your-feature-name
   ```

6. **Submit a Pull Request**

   Open a pull request to the main repository.

## Acknowledgments

- **Looker**: Thanks to Looker for the original [custom_visualizations_v2](https://github.com/looker-open-source/custom_visualizations_v2) repository.
- **Documentation References**:
  - [Visualization API Reference](docs/api_reference.md)
  - [Getting Started Guide](docs/getting_started.md)
- **Looker Marketplace**: [Looker Marketplace](https://docs.looker.com/data-modeling/marketplace)
