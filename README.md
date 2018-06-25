# Danicoin Wallet

This is the graphical user interface for 
[Danicoin](https://kumig.it/kumitterer/danicoin), the best cryptocurrency ever.

## Building

There are currently no pre-built binaries available for Danicoin Wallet. To
build it on Ubuntu Linux, let's first install all dependencies:

```
sudo apt install git build-essential cmake libboost-all-dev qt5-default
```

The next step is to download the source code for Danicoin Wallet and Danicoin
itself:

```
git clone https://kumig.it/kumitterer/danicoinwallet.git
cd danicoinwallet
git submodule init
git submodule update
```

We are now ready to build the wallet:

```
mkdir build
cd build
cmake ..
make
```

When the build is finished, the only thing left to do is installing it to 
/usr/bin so you can easily run it from the terminal:

```
sudo install danicoin /usr/bin/
```

## Running

Running Danicoin Wallet is as simple as it gets - just run it from your 
terminal:

```
danicoin
```

It will use the ".danicoin" subdirectory of your user directory to store its
data, including a full copy of the blockchain. If you want to change any of its
settings, you can pass them as command line arguments. To find all available
arguments, execute:

```
danicoin --help
```