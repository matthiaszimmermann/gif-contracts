# GIF Core Contracts

This repository holds the necessary contracts and tools to interact with an existing GIF instance.

## Setup

Clone this repository

```bash
git clone https://github.com/matthiaszimmermann/gif-contracts.git
cd git-contracts
```

Use [Brownie](https://github.com/matthiaszimmermann/brownie-docker) and start a brownie container

```bash
docker run -it --rm -v $PWD:/projects brownie
```

<!-- docker run -it --rm -v $PWD:/app gif-truffle bash -->

Inside the brownie container compile the contracts/interfaces

```bash
brownie compile -all
```

## Project Strucutre

The project structure matches with the [Brownie project structure](https://eth-brownie.readthedocs.io/en/stable/structure.html#structure)